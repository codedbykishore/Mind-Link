# SLIDE 07 — Architecture Diagram

---

> **Chrome MV3 + Gemini Nano + Two-World Bridge = the most sophisticated on-device AI security architecture in any Chrome extension.**

---

## High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                             CHROME BROWSER                               │
│                                                                          │
│  ┌──────────────────────┐      ┌───────────────────────────────────────┐│
│  │   POPUP (popup.js)   │      │   SERVICE WORKER (background.js)      ││
│  │   443 lines HTML     │      │   Rate-limited, event-driven          ││
│  │                      │      │                                        ││
│  │ • Circular Trust UI  │      │ • captureVisibleTab (screenshots)     ││
│  │ • Real-time Stats    │◄────►│ • declarativeNetRequest (ad rules)    ││
│  │ • Language Selector  │      │ • AI request proxy (fallback)         ││
│  │ • Recheck Button     │      │ • 1s rate limit on captures           ││
│  └──────────┬───────────┘      └──────────────┬────────────────────────┘│
│             │ chrome.storage.onChanged          │ chrome.runtime.message  │
│             ▼                                   ▼                        │
│  ┌──────────────────────────────────────────────────────────────────────┐│
│  │                  CONTENT SCRIPTS (ISOLATED World)                    ││
│  │  7 scripts, all IIFE-wrapped, communicating via shared namespace    ││
│  │                                                                      ││
│  │  🛡️ phishing-detector.js ── 3-tier: domain → text → visual (5s)   ││
│  │  💰 terms-analyzer.js ───── 3-stage: Summ → Rewrite → Prompt (15s)││
│  │  🚫 ads-learner.js ──────── DOM → AI → DNR rules (dynamic)        ││
│  │  ✏️ jargon-simplifier.js ── Selection → Rewriter API (instant)     ││
│  │  📖 dictionary.js ──────── Selection → Prompt API (instant)        ││
│  │  🌍 translator.js ──────── 10 languages via Translator API        ││
│  │  🎨 cosmetic.js ─────────── CSS selectors + learned patterns       ││
│  │  🔧 utils.js ────────────── escapeHtml, renderMarkdown, helpers    ││
│  │                                                                      ││
│  │            │ CustomEvent bridge (120s timeout)                       ││
│  │            ▼                                                         ││
│  │  ┌──────────────────────────────────────────────────────────────┐   ││
│  │  │              api.js (MAIN World) — injected by api-bridge.js │   ││
│  │  │                                                              │   ││
│  │  │  window.__notesio_api = {                                    │   ││
│  │  │    callChromeAI(),    ← Prompt API (Gemini Nano)            │   ││
│  │  │    summarizeText(),   ← Summarizer API (chunk fallback)     │   ││
│  │  │    rewriteText(),     ← Rewriter API                        │   ││
│  │  │    simplifyJargon(),  ← Rewriter → Prompt fallback chain    │   ││
│  │  │  }                                                           │   ││
│  │  │                                                              │   ││
│  │  │  API FALLBACK CHAIN:                                         │   ││
│  │  │  Summarizer → chunk-based Summarizer → Prompt → error msg   │   ││
│  │  │  Rewriter → Prompt → error msg                               │   ││
│  │  └──────────────────────────────────────────────────────────────┘   ││
│  └──────────────────────────────────────────────────────────────────────┘│
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────────┐│
│  │         🧠 CHROME BUILT-IN AI ENGINE (Gemini Nano on-device)         ││
│  │  ┌──────────┐ ┌────────────┐ ┌────────────┐ ┌──────────────┐       ││
│  │  │ Prompt   │ │ Summarizer │ │ Rewriter   │ │ Translator   │       ││
│  │  │ API      │ │ API        │ │ API        │ │ API          │       ││
│  │  │ (60%)    │ │ (15%)      │ │ (15%)      │ │ (10%)        │       ││
│  │  └──────────┘ └────────────┘ └────────────┘ └──────────────┘       ││
│  │              All processing: LOCAL. Zero network. Zero cloud.        ││
│  └──────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────┘
```

---

### CHART 8: Performance Timeline — Horizontal Gantt Chart
**Type:** Horizontal stacked Gantt — shows parallel and sequential processing times

**Data (phishing detection — worst case, all 3 tiers triggered):**

| Stage | Start (s) | End (s) | Duration | Color |
|-------|-----------|---------|----------|-------|
| Whitelist Check | 0.0 | 0.1 | 0.1s | Gray |
| Tier 1: Domain Scan | 0.1 | 0.6 | 0.5s | Light Blue |
| Tier 2: Text Scan | 0.6 | 2.6 | 2.0s | Blue |
| Tier 3: Visual Scan (screenshot + AI) | 2.6 | 7.6 | 5.0s | Dark Blue |
| Trust Score Calculation | 7.6 | 7.8 | 0.2s | Green |
| Warning Display | 7.8 | 8.0 | 0.2s | Green |
| **TOTAL (cold)** | 0.0 | 8.0 | **8.0s** | — |
| **TOTAL (cached)** | 0.0 | 0.1 | **< 0.1s** | Green |

**Second row (T&C pipeline):**

| Stage | Start (s) | End (s) | Duration | Color |
|-------|-----------|---------|----------|-------|
| Content Detection | 0.0 | 0.5 | 0.5s | Gray |
| Stage 1: Summarizer | 0.5 | 5.0 | 4.5s | Orange |
| Stage 2: Rewriter | 5.0 | 8.5 | 3.5s | Yellow |
| Stage 3: Prompt | 8.5 | 14.0 | 5.5s | Red |
| Cache + Display | 14.0 | 14.5 | 0.5s | Green |
| **TOTAL (cold)** | 0.0 | 14.5 | **14.5s** | — |
| **TOTAL (cached)** | 0.0 | 0.3 | **< 0.3s** | Green |

**Design:** Two horizontal Gantt rows. Show the dramatic improvement of "cached" below "cold" — tiny green bar vs. long multi-color bar.

---

## Two-World Bridge — How Scripts Talk to AI

```
  ISOLATED World                              MAIN World
  (7 content scripts)                         (api.js — injected by api-bridge.js)
       │                                           │
       │── CustomEvent ──────────────────────────►│
       │   __notesio_api_request                   │
       │   {action: "callChromeAI",                │
       │    prompt: "Analyze this domain..."}      │
       │                                           │──► LanguageModel.create()
       │                                           │──► Summarizer.create()
       │◄── CustomEvent ──────────────────────────│──► Rewriter.create()
       │   __notesio_api_response                  │──► Translator.create()
       │   {result: "Trust score: 2/5..."}         │
       │                                           │
       │   ⏱️ Timeout: 120s per request            │
       │   🔄 Fallback: service worker proxy       │
```

---

## Data Persistence — What We Cache

```
  chrome.storage.local (6 key patterns)
  ├── trustScore_{hostname}     ← Phishing scores  │ 24h TTL
  ├── termsAnalysis_{hostname}  ← T&C results       │ 24h TTL
  ├── adsBlocked_{hostname}     ← Per-site ad count │ Persistent
  ├── totalAdsBlocked           ← Global counter    │ Persistent
  ├── totalThreatsBlocked       ← Global counter    │ Persistent
  └── userLanguage              ← UI preference     │ Persistent
```

**Cache impact:** 80–90% reduction in AI API calls. 70% cache hit rate.
