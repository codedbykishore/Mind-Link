# PhishGuard Vision — Copilot Instructions

> Chrome MV3 extension protecting elderly users from phishing/scams using **on-device Chrome Built-in AI** (Gemini Nano). No build system, no external APIs, no bundler — plain JavaScript only.

## Architecture Overview

**Two execution worlds** communicate via events and messages:

1. **MAIN world** — `content/api.js` (injected via `api-bridge.js`). Has direct access to `LanguageModel`, `Summarizer`, `Rewriter`, `Translator` browser globals. This is where all AI API calls execute.
2. **ISOLATED world** — All other `content/*.js` files. They call AI through `window.__notesio_api` which bridges to MAIN world via `CustomEvent` pairs (`__notesio_api_request` / `__notesio_api_response`).
3. **Service worker** — `background.js`. Handles screenshot capture (`chrome.tabs.captureVisibleTab`), dynamic `declarativeNetRequest` ad-blocking rules, and fallback AI calls (service workers also have AI API access).

```
content script (ISOLATED) ──CustomEvent──▶ api.js (MAIN world)  ──▶ Chrome AI APIs
content script (ISOLATED) ──chrome.runtime.sendMessage──▶ background.js  ──▶ Screenshots / DNR rules
popup/popup.js ──chrome.storage.local──▶ reads trust scores, stats
```

## Code Conventions

- **IIFE pattern everywhere:** Every content script is `(function(){ ... })();` — no ES modules, no imports/exports.
- **Global namespace:** Shared state uses `window.__notesio_*` properties:
  - `window.__notesio_api` — AI call bridge (`callChromeAI`, `summarizeText`, `rewriteText`, `simplifyJargon`)
  - `window.__notesio_apiAvailable` / `window.__notesio_summarizerAvailable` — feature flags set by `api-bridge.js`
  - `window.__notesio_utils` — shared helpers (`escapeHtml`, `renderMarkdown`, `getSelectedText`)
- **Console logging:** Always prefix with `[Mind-Link]` or `[Mind-Link FeatureName]`.
- **Message types** (content ↔ background): `CAPTURE_SCREENSHOT`, `AI_REQUEST`, `AD_RULES_LEARNED`, `GET_COSMETIC_SELECTORS`.
- **Storage key conventions** (`chrome.storage.local`): `trustScore_{hostname}`, `adsBlocked_{hostname}`, `termsAnalysis_{hostname}`, `totalAdsBlocked`, `totalThreatsBlocked`.
- **All UI is DOM-created programmatically** — no frameworks, no templates. Use `Object.assign(el.style, {...})` for inline styles.

## Key Files

| File | Role |
|------|------|
| `content/api.js` | MAIN world AI wrapper — `callChromeAI()`, `summarizeText()`, `rewriteText()`, `simplifyJargon()`, chunk-based summarization fallback |
| `content/api-bridge.js` | Injects `api.js` into MAIN world, bridges requests via CustomEvents, 120s timeout |
| `content/phishing-detector.js` | 3-tier analysis: domain → text → visual (screenshot). Confidence-weighted trust score 1–5 |
| `content/terms-analyzer.js` | T&C detection + 3-stage pipeline: Summarizer → Rewriter → Prompt API. Manual trigger button |
| `content/jargon-simplifier.js` | Text selection → Rewriter API simplification (Ctrl+Shift+S shortcut) |
| `content/ads-learner.js` | AI-powered ad pattern learning, sends selectors/URL filters to background for DNR rules |
| `content/cosmetic.js` | Static + learned CSS ad hiding, trusted domain whitelist |
| `content/translator.js` | `self.translation` API wrapper for multi-language warnings |
| `content/dictionary.js` | Selected word → Prompt API definition lookup |
| `background.js` | Screenshot capture (rate-limited 1s), dynamic DNR rule management, AI request proxy |
| `popup/popup.js` | Dashboard UI with trust score, stats, language selector; listens to `chrome.storage.onChanged` |
| `documentations/` | Chrome API reference docs (prompt-api.txt, summarizer_api.txt, rewriter-api.txt, etc.) — **read these before using any AI API** |
| `rules/*.json` | Static declarativeNetRequest blocklists (basic, extended, bulletproof) |

## Critical Patterns

### AI API Access
Content scripts **cannot** call AI APIs directly. Always go through:
```js
const result = await window.__notesio_api.callChromeAI(prompt);        // Prompt API
const summary = await window.__notesio_api.summarizeText(text);        // Summarizer
const simplified = await window.__notesio_api.simplifyJargon(text);    // Rewriter (with Prompt fallback)
```
Check availability first: `if (!window.__notesio_apiAvailable) return;`

### Fallback Chain
Every AI feature degrades gracefully: Summarizer → chunk-based Summarizer → Prompt API fallback → error message. Rewriter → Prompt API fallback. See `content/api.js` for implementation.

### Performance Guards
- **Whitelist:** Trusted domains (Google, GitHub, Outlook, etc.) skip phishing analysis and ad learning — lists are in `phishing-detector.js`, `ads-learner.js`, `cosmetic.js`.
- **Rate limiting:** 5s between phishing checks per domain; 1s between screenshots.
- **24h caching:** Domain trust scores and T&C analysis results cached in `chrome.storage.local`.
- **Payload limits:** DOM collection capped (5 buttons, 3 links, 200-char descriptions, 80 ad candidates).
- **Token estimation:** Summarizer uses `text.length / 5` as conservative token estimate, chunks at 2500 tokens.

### `file://` Guard
All content scripts skip initialization on `file://` URLs to prevent extension context errors.

## Development & Testing

- **No build step.** Load as unpacked extension via `chrome://extensions` → Developer mode.
- **No test framework.** Test manually using `demo-test-sites/*.html` (fake PayPal, antivirus subscription, legal terms).
- **AI prerequisites:** Enable `chrome://flags/#optimization-guide-on-device-model`, `#prompt-api-for-gemini-nano`, `#summarization-api-for-gemini-nano`, `#rewriter-api-for-gemini-nano`. Download model via `chrome://on-device-internals`.
- **Debugging:** All features log to console with `[Mind-Link]` prefix. Check background service worker console separately.

## Rules

- **No external APIs or API keys.** All AI runs on-device via Chrome Built-in AI.
- **Do not create .md files** without explicit permission.
- **Max 500 lines per file.** Split features into separate content scripts.
- **Read `documentations/` folder** for API parameter details before implementing AI calls.
- **Keep UI elderly-friendly:** large text, high contrast, simple language, ARIA labels.