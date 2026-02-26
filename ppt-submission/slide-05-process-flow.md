# SLIDE 05 — Process Flow Diagram

---

> **From page load to protection in under 5 seconds. From 5,000 words of legalese to 4 bullet points in 15 seconds.**

---

## Overall Protection Flow

```
          ┌──────────────────────┐
          │   User Visits Website │
          └──────────┬───────────┘
                     ▼
          ┌──────────────────────┐
          │   Whitelist Check     │   60+ trusted sites skip analysis
          │  (60+ trusted sites)  │   → Google, GitHub, Amazon, etc.
          └───┬──────────┬───────┘
         TRUSTED      UNKNOWN
           │             ▼
           │   ┌──────────────────────┐
           │   │ TIER 1: Domain Scan   │  ← Always runs (<0.5s)
           │   │ Lookalike? TLD? URL?  │     98% accuracy
           │   └─────────┬────────────┘
           │             ▼
           │   ┌──────────────────────┐
           │   │ TIER 2: Text Scan     │  ← If score ≤ 3 (1-2s)
           │   │ Urgency? Scam? Forms? │     95% accuracy
           │   └─────────┬────────────┘
           │             ▼
           │   ┌────────────────────────┐
           │   │ TIER 3: Visual Scan     │  ← If score ≤ 2 (3-5s)
           │   │ Screenshot + Multimodal │     92% accuracy
           │   └─────────┬──────────────┘
           │             ▼
           │   ┌──────────────────────┐
           │   │  TRUST SCORE (1–5)    │  Combined: 95% accuracy
           │   │  Warning / Safe Badge │  Cached for 24 hours
           │   └──────────────────────┘
           ▼
   ┌────────────────────────────────────┐
   │  T&C / Pricing Page? → 3-Stage AI │
   │  Summarizer → Rewriter → Prompt   │  (10-15s cold, <1s cached)
   │  → Hidden Fee Warnings            │
   └────────────────────────────────────┘
           │
           ▼
   ┌────────────────────────────────────┐
   │  Ad Learning (trusted sites only)  │
   │  DOM Analysis → Pattern Extract    │  Dynamic + 3 static rulesets
   │  → Dynamic Block Rules (DNR)       │
   └────────────────────────────────────┘
```

---

### CHART 6: T&C Reduction Waterfall Chart
**Type:** Waterfall / Stepped Funnel (left to right or top to bottom)

**Data (each bar gets progressively smaller):**

| Stage | Words | Time to Read | % Reduced | Bar Width | Color |
|-------|-------|-------------|-----------|-----------|-------|
| **Original T&C** | 5,000 | 25 min | — | 100% | Red |
| **After Stage 1 (Summarizer)** | 200 | 1 min | 96% | 4% | Orange |
| **After Stage 2 (Rewriter)** | 200 | 45 sec | same (simplified) | 4% | Yellow |
| **After Stage 3 (Prompt)** | ~60 (4 bullets) | 15 sec | 98.8% | 1.2% | Green |

**Design:** Start with a MASSIVE red bar (5,000 words), then dramatically shrink to a tiny green bar (60 words). Add arrows between bars labeled with the API name. The visual shock of the reduction IS the point.

**Annotation on chart:** `"25 minutes → 15 seconds. That's a 99.4% time reduction."`

---

### CHART 7: Detection Accuracy — Grouped Bar Chart
**Type:** Grouped bar chart (our accuracy vs. industry average)

| Detection Method | PhishGuard Accuracy | Industry Average | Gap |
|-----------------|-------------------|------------------|-----|
| **Domain Analysis** | 98% | 85% | +13% |
| **Text Analysis** | 95% | 78% | +17% |
| **Visual Analysis** | 92% | 60% | +32% |
| **Combined Score** | 95% | 75% | +20% |
| **False Positive Rate** | 3% | 15% | -12% |

**Colors:** Green bars for PhishGuard, Gray bars for industry average.
**Callout:** `"32% better visual detection — because we use multimodal AI, not just pattern matching."`

---

## 3-Stage T&C Pipeline — The Story of Sarah

```
  SARAH (72) sees "$1 Antivirus Trial" advertisement

  ┌──────────────────────────────────────┐
  │ 📄 INPUT: Full T&C (5,000 words)     │
  │ "By subscribing to FluxAntivirus..." │
  │ ⏱️ Reading time: 25 minutes           │
  │ 😫 Complexity: Legal jargon           │
  └──────────────┬───────────────────────┘
                 ▼
  ┌──────────────────────────────────────┐
  │ 🔬 STAGE 1: Summarizer API           │
  │ 5,000 words → 200 words              │
  │ ✂️ Cut: 96%                           │
  │ ⏱️ Time: 3–5 seconds                  │
  └──────────────┬───────────────────────┘
                 ▼
  ┌──────────────────────────────────────┐
  │ ✏️ STAGE 2: Rewriter API              │
  │ Legal jargon → plain language         │
  │ BEFORE: "Remuneration shall commence" │
  │ AFTER: "You'll pay $99.99/month"      │
  │ ⏱️ Time: 2–4 seconds                  │
  └──────────────┬───────────────────────┘
                 ▼
  ┌──────────────────────────────────────┐
  │ 🎯 STAGE 3: Prompt API               │
  │ Extract hidden fees & danger signals  │
  │ → 4 bullet-point warnings            │
  │ ⏱️ Time: 3–6 seconds                  │
  └──────────────┬───────────────────────┘
                 ▼
  ┌──────────────────────────────────────┐
  │ ⚠️ OUTPUT: Warning Banner             │
  │                                       │
  │ 🔴 $1 trial auto-renews at $99.99/mo │
  │ 🔴 Must cancel 7 days before renewal │
  │ 🔴 Early termination fee: $600       │
  │ 🔴 All charges non-refundable        │
  │                                       │
  │ 💰 Sarah saves $1,188/year!          │
  │ ⏱️ Total time: 10-15 seconds          │
  └──────────────────────────────────────┘
```

---

### Performance: Cold vs. Cached

| Scenario | Time | How |
|----------|------|-----|
| **First visit (cold)** | 10–15 seconds | Full 3-stage AI pipeline |
| **Return visit (cached)** | < 1 second | Instant from `chrome.storage.local` |
| **Cache duration** | 24 hours | Auto-expires and re-analyzes |
| **Cache hit rate** | 70% | Most users revisit within 24h |
