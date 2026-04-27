# AutoQC — Automated Video Quality Control

> Upload a video. Get a broadcast-grade QC report in minutes. No manual frame scrubbing required.

**[Try AutoQC free →](https://www.autoqc.app)**

AutoQC is a SaaS platform that automatically checks video quality across 15 dimensions using a hybrid FFmpeg + AI architecture. It replaces manual frame-by-frame review for content creators, post-production teams, broadcasters, and MCN agencies.

---

## Detection capabilities

### Video (8 checks)

| Check | What it detects | Accuracy |
|-------|----------------|----------|
| VideoArtifact | Black frames, flash frames, bad edits — single FFmpeg pass | F1 103.7%, Precision 100% |
| FreezeFrame | Motion-stopped sequences — entropy-based 4-mode classifier | 100% pass rate, 0 false positives |
| Blur | Per-frame sharpness via Laplacian variance | — |
| Brightness | Overexposure / underexposure segments | — |
| Border | Letterbox / pillarbox black bars | — |
| Aspect Ratio | Pixel and display ratio vs. platform spec | — |

### Audio (5 checks, all VAD-enhanced)

| Check | Standard / Method |
|-------|------------------|
| Loudness Compliance | EBU R128 / ATSC A/85, speech-gated integrated loudness |
| Audio Clipping | True-peak overload detection |
| Background Noise | IEC 60268-1 / ITU-R BS.468, A-weighted SNR, non-speech segments only |
| Silence Detection | Frame-accurate gap detection |
| Speech Clarity | Spectral slope consistency, 3-path classifier (noise / music / silence) |

All 5 audio checks share one Silero VAD (ONNX Runtime) timeline — semantic-aware sampling, 75% faster than per-check analysis.

### AI Visual Detection

| Module | Method | Accuracy |
|--------|--------|----------|
| PlanD OCR (subtitles & text) | GPU-accelerated OCR + LLM semantic analysis pipeline | OCR Recall 96.9%, CER 2.12% |
| Logo & Brand Detection | Grounding DINO v1 (zero-shot), 4-stage aggregation pipeline | Zero brand sample upload required |

PlanD detects typos, safe zone violations, and subtitle issues across 109 languages.  
Logo Detection identifies brand logos (copyright risk) and branded products (endorsement risk) in frame.

### Platform Compliance

One-click verification against: **YouTube · TikTok · Instagram · Broadcast TV · Custom**

---

## How it works

```
Upload video
    ↓
Stage 1: ProbeStage       — video metadata (FFprobe)
Stage 2: FrameAnalysis    — 6 video checks (parallel)
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



