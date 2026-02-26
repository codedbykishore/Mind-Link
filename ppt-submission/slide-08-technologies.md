# SLIDE 08 — Technologies Used

---

> **4 AI APIs. 8 Chrome APIs. 0 external dependencies. Pure JavaScript. One mission.**

---

## Complete Technology Stack

| Layer | Technology | What It Does | Lines of Code |
|-------|-----------|-------------|---------------|
| **AI Engine** | Chrome Built-in AI (Gemini Nano) | All AI inference — 100% on-device | — (browser native) |
| **Platform** | Chrome Extension Manifest V3 | Extension framework, permissions, lifecycle | ~80 (manifest.json) |
| **Service Worker** | background.js | Screenshots, DNR rules, AI proxy | ~200 |
| **AI Bridge** | api.js + api-bridge.js | MAIN world injection, CustomEvent bridge, fallbacks | ~400 |
| **Detection** | phishing-detector.js | 3-tier analysis: domain → text → visual | ~450 |
| **T&C Pipeline** | terms-analyzer.js | 3-stage: Summarizer → Rewriter → Prompt | ~500 |
| **Ad Blocking** | ads-learner.js + cosmetic.js | AI pattern learning + CSS hiding + DNR rules | ~350 |
| **Utilities** | translator.js, dictionary.js, jargon-simplifier.js, utils.js | Translation, lookup, simplification, helpers | ~400 |
| **UI** | popup/index.html + popup.js | Dashboard, trust score circle, stats | ~1,000 |
| **Ad Rules** | rules/*.json (3 files) | Static blocklists: basic, extended, bulletproof | ~3,000 rules |
| **Language** | Vanilla JavaScript (ES2020+) | Everything — zero frameworks | **~3,400 total** |
| **Markup** | HTML5 + CSS3 | Popup UI + programmatic DOM overlays | inline |
| **Storage** | chrome.storage.local | Caching, preferences, counters | — (Chrome API) |

---

## Chrome APIs Used (8 total)

| Chrome API | Purpose | Used By |
|-----------|---------|---------|
| `LanguageModel` (Prompt API) | Text + multimodal AI inference | phishing-detector, ads-learner, terms-analyzer, dictionary |
| `Summarizer` (Summarizer API) | Text condensation (T&C) | terms-analyzer |
| `Rewriter` (Rewriter API) | Text simplification | terms-analyzer, jargon-simplifier |
| `Translator` (Translator API) | Multi-language support | translator |
| `chrome.tabs.captureVisibleTab` | Screenshot capture for visual AI | background → phishing-detector |
| `chrome.declarativeNetRequest` | Network-level ad blocking | background ← ads-learner |
| `chrome.storage.local` | Data persistence & caching | All scripts |
| `chrome.runtime.sendMessage` | Content script ↔ service worker | All communication |

---

### CHART 9: Tech Stack Layer Pyramid
**Type:** Stacked pyramid / layer diagram (bottom = foundation, top = user)

**Data (bottom to top):**

| Layer | Components | Width | Color |
|-------|-----------|-------|-------|
| **Layer 1 (Foundation)** | Chrome Built-in AI Engine (Gemini Nano) | Widest | Deep Blue |
| **Layer 2 (API)** | Prompt + Summarizer + Rewriter + Translator APIs | Wide | Blue |
| **Layer 3 (Bridge)** | api.js (MAIN) ↔ api-bridge.js (ISOLATED) + CustomEvent | Medium | Teal |
| **Layer 4 (Logic)** | phishing-detector + terms-analyzer + ads-learner + 4 more | Medium | Green |
| **Layer 5 (Storage)** | chrome.storage.local (caching, prefs, counters) | Narrower | Yellow |
| **Layer 6 (UI)** | popup + warning banners + modal + buttons | Narrowest | Orange |
| **Layer 7 (User)** | Elderly user, student, institution | Top | Purple |

**Design:** Classic pyramid. Each layer labeled with its components. Arrow on the side labeled "100% On-Device."

---

### CHART 10: Zero Dependencies — Stacked Bar Comparison
**Type:** Stacked horizontal bar chart comparing extension complexity

**Data:**

| Extension | npm Packages | Framework Size | Build Tools | External APIs | Total Deps |
|-----------|-------------|----------------|-------------|---------------|------------|
| **PhishGuard Vision** | **0** | **0 KB** | **0** | **0** | **0** |
| Typical React Extension | 187 | 142 KB | 5 (Webpack, Babel...) | 2-3 | ~200 |
| Typical Angular Extension | 312 | 310 KB | 8 | 3-5 | ~320 |
| Typical Vue Extension | 94 | 86 KB | 4 | 2-3 | ~100 |

**Colors:**
- npm packages: Blue stack
- Framework: Green stack
- Build tools: Orange stack
- External APIs: Red stack
- PhishGuard: Single flat GREEN bar at zero

**Callout:** `"Zero dependencies = zero supply chain vulnerabilities. Zero build time. Zero attack surface."`

---

## Architecture Patterns — Engineering Decisions

| Pattern | Why We Chose It | Benefit |
|---------|----------------|---------|
| **IIFE Encapsulation** | Every script `(function(){ ... })()` — no module system needed | Zero build step, no bundler |
| **Two-World Bridge** | MAIN world for AI access, ISOLATED for DOM safety | Security + AI access coexistence |
| **Graceful Degradation** | Every API has a fallback chain (Summarizer → Prompt, etc.) | Works even if one API is unavailable |
| **Cache-First** | Check storage before calling AI | 80-90% fewer AI calls |
| **Rate Limiting** | 5s phishing, 1s screenshots | Prevents quota exhaustion |
| **Whitelist Skip** | 60+ trusted domains bypass analysis entirely | Zero overhead on Google, GitHub, etc. |
| **Token Estimation** | `text.length / 5` for conservative token counting | Smart chunking for long documents |
| **Payload Optimization** | Cap: 5 buttons, 3 links, 200-char descriptions, 80 ad candidates | 30-40% smaller prompts |
