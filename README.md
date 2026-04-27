# AutoQC — Automated Video Quality Control

> Upload a video. Get a broadcast-grade QC report in minutes. No manual frame scrubbing required.

**[Try AutoQC free →](https://www.autoqc.app)**

AutoQC is a SaaS platform that automatically checks video quality across 15 dimensions using a hybrid FFmpeg + AI architecture. It replaces manual frame-by-frame review for content creators, post-production teams, broadcasters, and MCN agencies.

---

## Detection capabilities

### AI Visual Detection

| Module | Method | Accuracy |
|--------|--------|----------|
| PlanD OCR (subtitles & text) | GPU-accelerated OCR + LLM semantic analysis pipeline | OCR Recall 96.9%, CER 2.12% |
| Logo & Brand Detection | Grounding DINO v1 (zero-shot), 4-stage aggregation pipeline | Zero brand sample upload required |

**PlanD** detects typos, safe zone violations, and subtitle issues across 109 languages.  
**Logo Detection** identifies brand logos (copyright risk) and branded products (endorsement risk) — amber/orange timeline markers with dynamic bounding box tracking.

### Video (8 checks)

| Check | What it detects | Accuracy |
|-------|----------------|----------|
| Black Frame Detection | Unexpected black screens at any point in the video | F1 103.7%, Precision 100% |
| Flash Frame Detection | Rapid light flashes exceeding broadcast safety thresholds | F1 103.7%, Precision 100% |
| Bad Edit Detection | Ultra-short scenes (≤5 frames) sandwiched between scene changes | F1 103.7%, Precision 100% |
| Freeze Frame Detection | Motion-stopped sequences — entropy-based 4-mode classifier | 100% pass rate, 0 false positives |
| Blur Detection | Per-frame sharpness via Laplacian variance | — |
| Brightness Analysis | Overexposure / underexposure segments (SDR/HDR) | — |
| Border Detection | Letterbox / pillarbox black bars | — |
| Aspect Ratio Check | Pixel and display ratio vs. platform spec | — |

> Black Frame, Flash Frame, and Bad Edit are detected in a single FFmpeg pass (VideoArtifact v4.0) — no redundant processing.

### Audio (5 checks, all VAD-enhanced)

| Check | Standard / Method |
|-------|------------------|
| Audio Loudness Compliance | Speech-gated integrated loudness (EBU R128 / ATSC A/85) |
| Audio Clipping | True-peak overload detection |
| Background Noise | A-weighted SNR, non-speech segments only (IEC 60268-1 / ITU-R BS.468) |
| Silence Detection | Frame-accurate gap detection |
| Speech Clarity | Spectral slope consistency, 3-path classifier (noise / music / silence) |

All 5 audio checks share one Silero VAD (ONNX Runtime) timeline — semantic-aware sampling, 75% faster than per-check analysis.

### Platform Compliance

One-click verification against: **YouTube · TikTok · Instagram · Broadcast TV · Custom**

---

## How it works

```
Upload video
    ↓
Stage 1: ProbeStage       — video metadata (FFprobe)
Stage 2: FrameAnalysis    — 8 video checks (parallel)
Stage 3: AudioAnalysis    — 5 audio checks (shared VAD timeline)
Stage 4: Compliance       — platform spec validation
Stage 5: LogoDetection    — Grounding DINO (optional)
Stage 6: ArtifactGen      — preview thumbnails, waveform
    ↓ (optional)
PlanD OCR Pipeline        — GPU-accelerated text analysis
    ↓
Timeline Errors + PDF Report
```

**Efficiency**: Single-pass FFmpeg architecture reduces FFmpeg calls from 7 to 2 (−70%). Shared audio feature cache: 5 detectors, one extraction pass.

---

## Pricing

| Plan | Quota | Price |
|------|-------|-------|
| Free | 10 min / month | $0 |
| Pro | 60 min / month | $29.99/month |
| Ultra | 300 min / month | $89.99/month |

[See full pricing →](https://www.autoqc.app/pricing)

---

## Release history

| Version | Date | Highlight |
|---------|------|-----------|
| v1.03 | April 2026 | Logo & Brand Detection (Grounding DINO v1, zero-shot) |
| v1.02 | April 2026 | Version Control — upload revisions, auto-inherit dismissed errors |
| v1.0.1 | March 2026 | Subscription plans, billing portal, account management |
| v1.0.0 | March 2026 | Initial release — 14 core checks, PDF report, platform compliance |

[Full changelog →](https://www.autoqc.app/resource/whats-new)

---

## Links

| | |
|---|---|
| 🌐 Product | https://www.autoqc.app |
| 📋 Features | https://www.autoqc.app/features |
| 💬 FAQ | https://www.autoqc.app/faq |
| 📄 Documentation | https://www.autoqc.app/resource/document |
| 📝 Blog | https://www.autoqc.app/resource/blog |
| 🚀 Product Hunt | https://www.producthunt.com/products/autoqc/launches/autoqc |
| ⭐ G2 Reviews | https://www.g2.com/products/autoqc/reviews |
