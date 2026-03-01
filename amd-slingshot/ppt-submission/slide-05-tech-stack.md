# SLIDE 05 — Tech Stack  *(Page 5 of 7)*

---

## AMD First. Then Everything Else.

> **Our stack is a deliberate choice: AMD silicon at the base, Chrome as the platform, vanilla JavaScript as the glue. No unnecessary complexity. Every component earns its place.**

---

## The Stack — Layer by Layer

### Layer 1: Hardware (AMD)
| AMD Technology | Role in PhishGuard Vision |
|---------------|--------------------------|
| **AMD Ryzen AI NPU (XDNA)** | PRIMARY: Accelerates all on-device AI model inference |
| **AMD APU (integrated GPU)** | Secondary compute for parallel AI tasks |
| **AMD ROCm** | Future: Custom model fine-tuning for phishing-specific patterns |
| **AMD Instinct** | Future: Large-scale model training for institutional deployment |

### Layer 2: AI Runtime (On-Device)
| AI API | Purpose | Inference via |
|--------|---------|--------------|
| **Prompt API** | Phishing detection (text + multimodal), ad learning, T&C analysis | AMD NPU |
| **Summarizer API** | T&C condensation: 5,000 → 200 words | AMD NPU |
| **Rewriter API** | Legal jargon → plain language | AMD NPU |
| **Translator API** | Warnings in 10 languages | AMD NPU |

### Layer 3: Platform
| Technology | Version | Role |
|-----------|---------|------|
| **Chrome Extension** | Manifest V3 | Extension framework and lifecycle |
| **Service Worker** | MV3 background | Screenshots, DNR rules, AI proxy |
| **chrome.declarativeNetRequest** | v3 | Network-level ad blocking (3 rulesets) |
| **chrome.storage.local** | — | All persistence and caching |

### Layer 4: Application
| Technology | Role |
|-----------|------|
| **Vanilla JavaScript (ES2020+)** | All application code — no framework overhead |
| **IIFE pattern** | Encapsulation without a module bundler |
| **HTML5 + CSS3** | Popup UI and programmatic DOM overlays |
| **Custom Event Bridge** | ISOLATED ↔ MAIN world communication |

---

## CHART: Tech Stack Pyramid
**Type:** Stacked pyramid (base = hardware, peak = user)

| Level | Layer | Components | Color |
|-------|-------|-----------|-------|
| Base | **AMD Hardware** | Ryzen AI NPU (XDNA), APU, ROCm | Red (AMD brand color) |
| 2 | **On-Device AI** | Prompt + Summarizer + Rewriter + Translator | Orange |
| 3 | **Chrome Platform** | MV3, Service Worker, DNR, storage | Blue |
| 4 | **Application Logic** | 8 content scripts, api bridge, popup | Green |
| Peak | **User** | Zero config, automatic protection | Purple |

**Design note:** Base layer (AMD) should be widest, deepest colored, most prominent. The pyramid visually communicates that AMD is the foundation everything else rests on.

---

## CHART: Dependency Comparison
**Type:** Stacked bar — Us vs. typical security extensions

| Extension | npm deps | Framework (KB) | Build tools | Cloud APIs | AMD NPU? |
|-----------|---------|---------------|------------|------------|---------|
| **PhishGuard Vision** | **0** | **0** | **0** | **0** | **Yes** |
| Norton Browser Ext | ~50+ | 180KB | Yes | Yes | No |
| McAfee Browser Ext | ~80+ | 320KB | Yes | Yes | No |
| Bitdefender Ext | ~40+ | 150KB | Yes | Yes | No |

**Takeaway:** We run leaner than any competitor, AND we use AMD hardware that they don't.

---

## Why Zero External Dependencies?

| Choice | Reason |
|--------|--------|
| No npm packages | Zero supply chain attack vectors |
| No framework | No 180KB overhead; instant load |
| No build step | Load as unpacked — no compilation errors |
| No cloud APIs | Privacy by design; AMD NPU handles everything |
| No external servers | Zero operating cost; infinite scalability |

> **The leanest possible stack sitting on the most powerful possible hardware: AMD silicon.**
