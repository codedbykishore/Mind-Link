# SLIDE 11 — Prototype Assets (Optional)

---

## Working Prototype — v3.1.0

**Status:** ✅ Fully functional Chrome extension
**Repository:** https://github.com/HIRU-VIRU/Mind-Link
**Install:** `chrome://extensions` → Developer Mode → Load Unpacked

---

## Demo Test Sites (Included in Repo)

| Test Site | What It Simulates | File |
|-----------|-------------------|------|
| **Fake PayPal** | Phishing site with lookalike domain, blurry logo, urgency language | `demo-test-sites/fake-paypal.html` |
| **Fake Antivirus** | $1 trial with 5,000-word T&C hiding $99.99/mo auto-renewal | `demo-test-sites/fake-antivirus-subscription.html` |
| **Complex Legal Terms** | Dense EULA with hidden $500 fees & binding arbitration | `demo-test-sites/complex-legal-terms.html` |

---

## Live Demo Scenarios

1. **Phishing Detection** → Open `fake-paypal.html` → Red warning banner, Trust Score 1/5
2. **Hidden Fee Detection** → Open `fake-antivirus-subscription.html` → 3-stage analysis → fee warnings
3. **Ad Blocking** → Browse any ad-heavy site → AI learns & removes ads
4. **Multi-Language** → Switch language in popup → warnings translate instantly

---

## Performance Metrics

| Metric | Value |
|--------|-------|
| Phishing detection accuracy | **95%** |
| Page load impact | **< 500ms** |
| Memory usage | **< 100MB** |
| Cache hit rate | **70%** |
| AI quota reduction (caching) | **80–90%** |
| External API calls | **0** |

---

## Key Files (17 total)

```
Mind-Link/
├── manifest.json              # MV3 config
├── background.js              # Service worker
├── popup/
│   ├── index.html             # Dashboard UI
│   └── popup.js               # Trust score + stats
├── content/
│   ├── api.js                 # MAIN world AI wrapper
│   ├── api-bridge.js          # ISOLATED↔MAIN bridge
│   ├── phishing-detector.js   # 3-tier detection
│   ├── terms-analyzer.js      # 3-stage T&C pipeline
│   ├── ads-learner.js         # AI ad learning
│   ├── jargon-simplifier.js   # Text simplifier
│   ├── dictionary.js          # Word lookup
│   ├── translator.js          # 10-language support
│   ├── cosmetic.js            # CSS ad hiding
│   └── utils.js               # Shared helpers
├── rules/                     # Static ad blocklists
└── demo-test-sites/           # 3 test HTML pages
```
