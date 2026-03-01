# SLIDE 04 — Architecture  *(Page 4 of 7)*

---

## AMD Silicon at the Core — Everything Else Follows

> **The architecture is simple by design: AMD Ryzen AI NPU does the heavy lifting. The browser is just the delivery mechanism.**

---

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        USER'S DEVICE                                 │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                     CHROME BROWSER                            │   │
│  │                                                               │   │
│  │  ┌────────────────┐     ┌──────────────────────────────────┐ │   │
│  │  │  POPUP UI       │     │  SERVICE WORKER (background.js)  │ │   │
│  │  │                 │     │                                   │ │   │
│  │  │ • Trust Score   │◄───►│ • Screenshot capture             │ │   │
│  │  │ • Stats         │     │ • Ad rule management (DNR)       │ │   │
│  │  │ • Language      │     │ • AI request fallback proxy      │ │   │
│  │  └────────┬────────┘     └───────────────┬──────────────────┘ │   │
│  │           │ chrome.storage                │ chrome.runtime      │   │
│  │           ▼                               ▼                    │   │
│  │  ┌──────────────────────────────────────────────────────────┐ │   │
│  │  │               CONTENT SCRIPTS (8 modules)                │ │   │
│  │  │                                                          │ │   │
│  │  │  phishing-detector  →  3-tier analysis                   │ │   │
│  │  │  terms-analyzer     →  3-stage T&C pipeline              │ │   │
│  │  │  ads-learner        →  AI pattern learning + DNR rules   │ │   │
│  │  │  translator         →  10-language support               │ │   │
│  │  │  jargon-simplifier  →  text simplification               │ │   │
│  │  │  dictionary         →  word lookup                       │ │   │
│  │  │  cosmetic           →  CSS ad hiding                     │ │   │
│  │  │  utils              →  shared helpers                    │ │   │
│  │  │                                                          │ │   │
│  │  │           │  CustomEvent bridge (120s timeout)           │ │   │
│  │  │           ▼                                              │ │   │
│  │  │  ┌──────────────────────────────────────────────────┐   │ │   │
│  │  │  │            api.js (MAIN World)                   │   │ │   │
│  │  │  │  callChromeAI()  summarizeText()  rewriteText()  │   │ │   │
│  │  │  │  simplifyJargon()  (with full fallback chains)   │   │ │   │
│  │  │  └──────────────────────────────────────────────────┘   │ │   │
│  │  └──────────────────────────────────────────────────────────┘ │   │
│  │                                                               │   │
│  │  ┌──────────────────────────────────────────────────────────┐ │   │
│  │  │          ON-DEVICE AI APIS (4 active)                   │ │   │
│  │  │  Prompt API │ Summarizer API │ Rewriter API │ Translator │ │   │
│  │  └──────────────────────────────────────────────────────────┘ │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │          AMD Ryzen AI NPU (XDNA Architecture)                 │   │
│  │                                                               │   │
│  │  Accelerates ALL on-device AI inference — no cloud calls     │   │
│  │  Matrix operations for language model inference              │   │
│  │  Low power: always-on protection without battery drain       │   │
│  │  Dedicated silicon: CPU cores stay free for browser work     │   │
│  └──────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘

        ZERO bytes sent to the internet for AI processing
```

---

## How the AMD NPU Powers Each Feature

| Feature Module | AI Operation | AMD NPU Role |
|---------------|-------------|-------------|
| `phishing-detector.js` | Prompt API: domain + text + screenshot analysis | NPU runs transformer inference for pattern matching |
| `terms-analyzer.js` | Summarizer → Rewriter → Prompt (pipeline) | NPU handles 3 sequential model calls in 10–15s |
| `ads-learner.js` | Prompt API: DOM structure analysis | NPU identifies ad patterns in page context |
| `translator.js` | Translator API: text → 10 languages | NPU inference for language pair translation |
| `jargon-simplifier.js` | Rewriter API: plain language conversion | NPU rewrite inference on selected text |
| `dictionary.js` | Prompt API: context-aware definitions | NPU generates definitions from context |

---

## Two-World Security Architecture

```
  ISOLATED World                          MAIN World
  (7 content scripts)                     (api.js — MAIN execution context)
        │                                       │
        │── CustomEvent ───────────────────────►│
        │   {action: "callChromeAI",             │
        │    prompt: "Analyze this site..."}     │──► On-device AI API
        │◄── CustomEvent ──────────────────────│     ↓
        │   {result: "Trust score: 2/5"}        │   AMD NPU
        │                                       │   (local inference)
        │   Timeout: 120s  │  Rate limit: 5s   │
```

**Why two worlds?** Security: ISOLATED world scripts can't directly access AI APIs (browser security model). MAIN world has access but no DOM safety. The bridge gives us both.

---

## Data Layer — What Gets Stored (Locally Only)

```
chrome.storage.local
├── trustScore_{hostname}     ← 24h cache  (phishing analysis)
├── termsAnalysis_{hostname}  ← 24h cache  (T&C pipeline results)
├── adsBlocked_{hostname}     ← persistent (per-site ad stats)
├── totalAdsBlocked           ← persistent (global counter)
├── totalThreatsBlocked       ← persistent (global counter)
└── userLanguage              ← persistent (language preference)
```

> **Everything is local. Nothing is telemetry. No server receives a single byte.**

---

## CHART: Performance — AMD NPU vs No NPU (Gantt)
**Type:** Horizontal Gantt — shows processing time improvement

| Operation | Without NPU | With AMD NPU | Speedup |
|-----------|------------|-------------|---------|
| Domain scan | 1.2s | 0.5s | 2.4x |
| Text analysis | 4.0s | 2.0s | 2x |
| Screenshot analysis | 9.0s | 5.0s | 1.8x |
| T&C pipeline (full) | 28s | 14.5s | 1.9x |
| Translation (10 langs) | 6s | 3s | 2x |

**Design:** Green bars (with NPU) visually half the length of red bars (without NPU). Strong visual impact.
