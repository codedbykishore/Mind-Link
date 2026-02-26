# SLIDE 04 — Features Offered

---

> **6 features. 4 AI APIs. 1 mission: protect the most vulnerable users on the internet.**

---

## Feature 1: Multi-Tier Phishing Detection 🛡️
**API: Prompt API (text + multimodal)**

| Tier | Analysis | What It Catches | Trigger | Speed |
|------|----------|----------------|---------|-------|
| Tier 1 | **Domain Scan** | Lookalike domains (`paypa1.com`), suspicious TLDs (`.tk`, `.ml`), URL obfuscation, homograph attacks | Always | < 0.5s |
| Tier 2 | **Text Scan** | "Act now!", fake prizes, password requests, suspicious form fields | Score ≤ 3 | 1–2s |
| Tier 3 | **Visual Scan** | Blurry logos, fake security badges, scareware popups, deceptive UI | Score ≤ 2 | 3–5s |

**Output:** Trust Score 1–5 → Color-coded (Red / Orange / Green)
**Innovation:** First extension to use **multimodal screenshot analysis** for phishing — no one else does this.

---

## Feature 2: Hidden Fee Detector & T&C Simplifier 💰
**APIs: Summarizer + Rewriter + Prompt (3-Stage Pipeline)**

| Stage | Chrome AI API | Input → Output | Reduction | Time |
|-------|--------------|----------------|-----------|------|
| 1 | **Summarizer** | 5,000 words → 200 words | 96% | 3–5s |
| 2 | **Rewriter** | Legal jargon → 5th-grade language | Simplification | 2–4s |
| 3 | **Prompt** | Plain text → 4 bullet warnings | 98% total | 3–6s |

**Bottom line:** 25 minutes reading → 15 seconds. **$1,188/year saved** per user from exposed hidden fees.

---

## Feature 3: AI-Learned Ad Blocker 🚫
**API: Prompt API + chrome.declarativeNetRequest**

| Capability | How It Works |
|-----------|-------------|
| AI DOM Analysis | Prompt API scans page structure to identify ad containers |
| Dynamic Rule Creation | Learned patterns → `declarativeNetRequest` network rules |
| Static Blocklists | 3 rule sets (basic, extended, bulletproof) as fallback |
| Safety-First | Only learns from trusted sites (trust score ≥ 3) |
| Malvertising Detection | Blocks fake download buttons, clickjack overlays |

---

## Feature 4: Multi-Language Support 🌍
**API: Translator API**

| Language | Code | Language | Code |
|----------|------|----------|------|
| English | EN | Japanese | JA |
| Spanish | ES | Chinese | ZH |
| French | FR | Arabic | AR |
| German | DE | Hindi | HI |
| Italian | IT | Portuguese | PT |

**What gets translated:** Warnings, trust scores, T&C findings, popup UI — everything the user sees.

---

## Feature 5: Jargon Simplifier ✏️
**API: Rewriter API** | Select text → Ctrl+Shift+S → instant plain-language simplification

## Feature 6: Dictionary Lookup 📖
**API: Prompt API** | Select any word → AI definition in context, plain language

---

### CHART 4: Feature-Quality Radar Chart
**Type:** Radar/Spider chart — 6 axes, each scored 1–10

| Axis | PhishGuard Score | Competitor Avg |
|------|-----------------|----------------|
| **Privacy** | 10 | 4 |
| **Accuracy** | 9.5 | 7 |
| **Speed** | 8 | 6 |
| **Accessibility** | 10 | 3 |
| **Feature Coverage** | 9 | 5 |
| **Cost Efficiency** | 10 | 4 |

**Design:** Our polygon (green, filled) should dramatically engulf the competitor polygon (red, outline only). Maximum visual contrast.

---

### CHART 5: API Usage Distribution — Pie Chart
**Type:** Pie chart with labels showing feature mapping

| API | % Usage | Features Powered | Color |
|-----|---------|-----------------|-------|
| **Prompt API** | 60% | Phishing detection (3 tiers), ad learning, T&C red-flag extraction, dictionary | Blue |
| **Summarizer API** | 15% | T&C condensation (5000→200 words) | Green |
| **Rewriter API** | 15% | T&C simplification, jargon simplifier | Orange |
| **Translator API** | 10% | 10-language warning translation | Purple |

**Center label:** `4 APIs = 6 Features`
