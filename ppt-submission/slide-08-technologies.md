# SLIDE 08 — Technologies Used

---

## Core Technology Stack

| Category | Technology | Purpose |
|----------|-----------|---------|
| **Platform** | Chrome Extension (Manifest V3) | Extension framework |
| **AI Engine** | Chrome Built-in AI (Gemini Nano) | 100% on-device AI |
| **Language** | Vanilla JavaScript (ES2020+) | All application code |
| **UI** | HTML5 + CSS3 (programmatic DOM) | Popup & overlay UI |
| **Storage** | chrome.storage.local | Caching & user prefs |
| **Ad Blocking** | chrome.declarativeNetRequest | Network-level blocking |
| **Visual Analysis** | chrome.tabs.captureVisibleTab | Screenshot capture |

---

## Chrome Built-in AI APIs Used (4 of 6 available)

| API | What We Use It For | % Usage |
|-----|-------------------|---------|
| **Prompt API** | Phishing detection (domain, text, visual), ad pattern learning, T&C red-flag analysis | **60%** |
| **Summarizer API** | Condense T&C documents (5,000 → 200 words) | **15%** |
| **Rewriter API** | Simplify legal jargon to plain language | **15%** |
| **Translator API** | Translate warnings into 10 languages | **10%** |

---

## Architecture Patterns

| Pattern | How We Use It |
|---------|--------------|
| IIFE Encapsulation | Every script wrapped in `(function(){ ... })()` |
| Event-Driven Bridge | CustomEvent pairs for cross-world communication |
| Graceful Degradation | Summarizer → Prompt fallback; Rewriter → Prompt fallback |
| Cache-First Strategy | 24h cache for trust scores & T&C results |
| Rate Limiting | 5s phishing checks; 1s screenshots |
| Whitelist Optimization | 60+ trusted domains skip all analysis |

---

## Zero External Dependencies

- ❌ No build system (Webpack, Vite, Rollup)
- ❌ No frameworks (React, Vue, Angular)
- ❌ No external APIs or API keys
- ❌ No npm packages
- ❌ No servers or cloud infrastructure
- ✅ **Pure vanilla JavaScript — everything runs in-browser**
