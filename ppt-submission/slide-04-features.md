# SLIDE 04 — Features Offered

---

## Feature 1: Multi-Tier Phishing Detection 🛡️
**API: Prompt API (text + multimodal)**

| Tier | Analysis | Trigger |
|------|----------|---------|
| Tier 1 | **Domain Analysis** — lookalike domains, suspicious TLDs, URL obfuscation | Always |
| Tier 2 | **Text Analysis** — urgency language, scam patterns, suspicious forms | Score ≤ 3 |
| Tier 3 | **Visual Analysis** — AI screenshot analysis for spoofed logos & fake UI | Score ≤ 2 |

→ **Output: Trust Score 1–5** (Red → Orange → Green)

---

## Feature 2: Hidden Fee Detector & T&C Simplifier 💰
**APIs: Summarizer + Rewriter + Prompt (3-Stage Pipeline)**

| Stage | API | What It Does |
|-------|-----|-------------|
| 1 | Summarizer | 5,000 words → 200 words (94% reduction) |
| 2 | Rewriter | Legal jargon → plain, elderly-friendly language |
| 3 | Prompt | Extracts hidden fees, auto-renewals, traps |

→ **Result: 25 min reading → 15 seconds. 4 clear bullet warnings.**

---

## Feature 3: AI-Learned Ad Blocker 🚫
**API: Prompt API + declarativeNetRequest**

- AI analyzes DOM to learn ad patterns dynamically
- Blocks malvertising & fake download buttons
- Safety-first: only learns from trusted sites (score ≥ 3)

---

## Feature 4: Multi-Language Support 🌍
**API: Translator API**

- 10 languages — EN, ES, FR, DE, IT, PT, JA, ZH, AR, HI
- All warnings & UI automatically translated

---

## Feature 5: Jargon Simplifier & Dictionary 📖
**APIs: Rewriter API + Prompt API**

- Select text → Ctrl+Shift+S → instant simplification
- Word lookup with plain-language AI definitions
