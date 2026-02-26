# SLIDE 09 — Usage of AMD Products/Solutions

---

## How AMD Accelerates PhishGuard Vision

PhishGuard Vision processes **all AI locally** using Gemini Nano — making AMD's on-device AI hardware directly relevant.

---

### AMD Technology Alignment

| AMD Product | How It Helps PhishGuard Vision |
|-------------|-------------------------------|
| **AMD Ryzen AI (NPU)** | Hardware AI accelerator speeds up Gemini Nano inference — our 3-stage T&C pipeline (10–15s) runs faster on NPU |
| **AMD APUs (Integrated AI)** | No discrete GPU needed — on-device AI runs on integrated compute, matching our "works everywhere" goal |
| **AMD ROCm (Software Stack)** | Future potential: fine-tune custom phishing detection models on AMD GPUs |
| **AMD Instinct (Data Center)** | Enterprise scale: train/fine-tune models on AMD Instinct for large-scale deployment |
| **AMD Adaptive SoCs** | Edge deployment: kiosks, shared lab machines, low-power always-on protection |

---

### Synergy with AMD's Vision

1. **Local AI Processing** — AMD's NPU philosophy matches our zero-cloud, privacy-first approach
2. **Power Efficiency** — AMD Ryzen AI NPU enables always-on protection without battery drain
3. **Performance** — Faster NPU inference = faster threat detection for end users
4. **Scalability** — AMD hardware spans laptop → desktop → kiosk → cloud — same as our deployment targets
5. **Privacy First** — AMD's on-device compute ensures user data never leaves the machine

---

### Performance on AMD Hardware

| Metric | Without NPU | With AMD Ryzen AI NPU |
|--------|-------------|----------------------|
| T&C Analysis Pipeline | 10–15s | ~5–8s (estimated) |
| Phishing Detection | 2–5s | ~1–3s (estimated) |
| Memory Overhead | < 100MB | < 100MB |
| Power Impact | Medium | Low (NPU offload) |
