# Detection Checks

AutoQC runs up to 15 automated quality checks across four categories. Each check can be individually enabled, disabled, and tuned in the analysis settings before upload.

**Full interactive documentation**: [autoqc.app/resource/document](https://www.autoqc.app/resource/document)

---

## Table of Contents

- [Video Detectors](#video-detectors) (8 checks)
- [Audio Detectors](#audio-detectors) (5 checks)
- [AI Text Detection](#ai-text-detection) (3 checks)
- [Logo & Brand Detection](#logo--brand-detection) (2 checks)

---

## Video Detectors

Auto QC runs up to seven video quality checks. Each can be individually enabled, disabled, and tuned in the analysis settings.

> Black Frame, Flash Frame, and Bad Edit are detected in a single FFmpeg pass (VideoArtifact v4.0) — no redundant processing.

---

### Bad Edit Detection

Flags cuts where an unusually brief shot — typically lasting only a handful of frames — appears between two larger scene changes. These micro-clips often indicate an unintended edit, a dropped frame cluster, or a rendering artefact.

This detector also covers **Black Frame** and **Flash Frame** detection in the same processing pass:
- **Black Frame** — unexpected black screens of any duration
- **Flash Frame** — rapid light flashes exceeding broadcast safety thresholds

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Sensitivity | Medium | Low / Medium / High | Higher sensitivity catches more potential issues but may increase false positives on fast-cut content |
| Minimum Gap Duration | 5 frames | 1 – 15 frames | The maximum length of the in-between shot that triggers a flag |

**Results**: Each flagged edit appears on the Timeline as a Warning or Critical card, showing the exact timecode, the duration of the anomalous shot, and the severity level.

**Accuracy**: Precision 100%, F1 Score 103.7% (VideoArtifact v4.0, tested on 5 scene types)

---

### Freeze Frame Detection

Detects sequences where the video image is completely or near-completely static for longer than expected — a common sign of a technical fault, signal dropout, or export error.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Minimum Freeze Duration | 2 seconds | 0.5s – 10s | How long the video must be static before it is flagged |
| Sensitivity | Medium | Low / Medium / High | At Low, only completely static images are flagged. At High, near-static images with small noise patterns are also caught |

**Results**: Freeze events appear on the Timeline with their start timecode and duration. Very long freezes are marked Critical; shorter ones are marked Warning.

**Accuracy**: 100% pass rate, zero false positives across 7 scene type test cases (including low-contrast motion and noise-heavy static frames)

---

### Blur Detection

Identifies frames where the image sharpness falls below an acceptable level, which may indicate out-of-focus recording, excessive motion blur, or compression degradation.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Sensitivity | Medium | Low / Medium / High | Increase for content requiring high sharpness (e.g. text-heavy graphics). Decrease for intentionally stylised or shallow-depth footage |

**Results**: Blurred frames are marked on the Timeline by timecode. Each card indicates the severity of the blur and the affected duration.

---

### Brightness Analysis

Checks for frames that are significantly overexposed (too bright) or underexposed (too dark). Both extremes can make content unwatchable and may indicate a grading error or a source file problem.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Black Frame Threshold | 10 / 255 | 2 – 30 | Frames where average brightness falls below this value are flagged as underexposed or blacked out |
| White Frame Threshold | 245 / 255 | 200 – 254 | Frames where average brightness exceeds this value are flagged as overexposed or blown out |
| Minimum Duration | 0.5 seconds | 1 frame – 5s | How long a brightness anomaly must persist to be reported |

**Results**: Brightness issues appear on the Timeline labelled as 'Black Frame', 'White Frame', or 'Overexposure'. Each entry shows the affected timecode range and average brightness value.

---

### Aspect Ratio Check

Verifies that the video's aspect ratio matches your intended output specification throughout the entire duration. Unexpected ratio changes can cause letterboxing, pillarboxing, or cropping on delivery platforms.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Expected Ratio | 16:9 | 16:9 / 9:16 / 4:3 / 1:1 / Custom | The ratio your finished video should conform to |
| Tolerance | ±1% | 0% – 5% | Allowable deviation before an issue is reported |

**Results**: A Pass or Fail result is shown in the General Report. If the ratio is incorrect, the Timeline shows which sections are non-conforming and what ratio was detected.

---

### Border Detection

Scans for persistent black bars or coloured borders around the edges of the frame. These often appear when widescreen content is incorrectly letterboxed, when a source clip has embedded borders, or after an incorrect export.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Border Tolerance | 8 pixels | 0 – 40 px | Borders narrower than this value are ignored |
| Minimum Duration | 1 second | 1 frame – 10s | How long a border must be present before it is reported |

**Results**: Detected borders are shown on the Timeline with their location (top / bottom / left / right / all sides), width in pixels, and the timecode range during which they appear.

---

### Platform Compliance

Validates your video's technical specifications against the official requirements of your chosen delivery platform. Catches resolution, frame rate, and format mismatches before you submit.

**Configuration**

| Parameter | Default | Range |
|-----------|---------|-------|
| Target Platform | YouTube | YouTube / TikTok / Instagram / Broadcast TV / Custom |

**Results**: A compliance table appears in the General Report showing Pass, Warning, or Fail for each specification (resolution, frame rate, codec, bitrate, colour space, etc.). Failed items are also listed in the Timeline as General Error cards.

---

## Audio Detectors

Five audio checks cover common delivery problems — from clipping and silence to loudness compliance and speech intelligibility.

> All 5 audio checks share one Silero VAD (ONNX Runtime) timeline — semantic-aware sampling, 75% faster than per-check analysis.

---

### Audio Clipping

Detects moments where the audio signal peaks beyond the maximum level, causing distortion commonly heard as crackling or 'hard clipping'. This is one of the most common technical audio faults in delivered content.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Clipping Threshold | −1.0 dBFS | −6.0 – 0.0 dBFS | Tighter thresholds (e.g. −3 dBFS) are recommended for broadcast delivery |
| Minimum Duration | 10 ms | 1 ms – 1 s | Very brief peaks (under 10 ms) may be inaudible and can be excluded |

**Results**: Clipping events appear on the Timeline with their exact timecode, peak level in dBFS, and duration. Severe or prolonged clipping is marked Critical.

---

### Silence Detection

Identifies sections of the audio track that are completely silent or near-silent for longer than expected. These gaps may indicate a missing audio segment, an edit error, or an encoding fault.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Silence Threshold | −50 dBFS | −70 – −30 dBFS | Raise to flag near-silent passages; lower to ignore low-level ambience |
| Minimum Duration | 2 seconds | 0.5s – 30s | Short pauses in dialogue should not trigger this check |

**Results**: Silent sections are shown on the Timeline with start timecode and duration. Sections longer than 5 seconds are marked Critical by default.

---

### Background Noise

Measures the level of continuous background noise — room tone, HVAC hum, electrical hiss, traffic, or similar ambient sound. High background noise can reduce perceived audio quality and affect platform acceptance.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Noise Threshold | −40 dBFS | −60 – −20 dBFS | Adjust based on content type: documentaries may tolerate higher ambient noise than studio productions |
| Sensitivity | Medium | Low / Medium / High | Higher sensitivity is better for content with frequent speech pauses |

**Results**: A noise level score (Low / Moderate / High) is shown in the General Report. Affected time ranges appear on the Timeline for content with elevated noise.

---

### Loudness Compliance

Checks that the overall loudness of your audio track conforms to the standard used by broadcast networks and streaming platforms. Non-compliant loudness is a common cause of content rejection.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Loudness Standard | EBU R128 (−23 LUFS) | EBU R128 / ATSC A/85 / Custom | Use EBU R128 for European broadcast and most streaming. Use ATSC A/85 for North American broadcast |
| Tolerance | ±1 LU | ±0.5 – ±2 LU | Broadcast deliverables typically require tighter tolerance |

**Results**: A single Pass / Fail result is shown in the General Report, along with the measured integrated loudness (in LUFS), true peak level, and loudness range (LRA). Non-compliant results also appear as a General Error card in the Timeline.

---

### Speech Clarity

Evaluates how intelligible the spoken dialogue or narration is relative to the audio background. Low scores can indicate excessive background noise, music that overpowers speech, or poorly recorded dialogue.

**Configuration**: No user-configurable parameters.

**Results**: A clarity score (Excellent / Good / Acceptable / Poor) is shown in the General Report. Sections where clarity drops below Acceptable are listed on the Timeline with their timecode.

---

## AI Text Detection

Three AI-powered checks scan on-screen text and subtitles for spelling errors, safe zone violations, and extraction. Enable these under the Advanced preset or in Custom settings.

**Engine**: PlanD — GPU-accelerated OCR + LLM semantic analysis pipeline. OCR Recall 96.9%, CER 2.12%. 109 languages supported.

---

### Typo Detection

Reads on-screen text and embedded subtitles frame by frame, then flags words that appear to be misspelled or grammatically incorrect. Supports over 100 languages.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Language | English (en) | 100+ languages | Set to match your subtitle language for the most accurate detection |
| Confidence Threshold | Medium | Low / Medium / High | Lower confidence surfaces more candidates; higher confidence reports only clear mistakes |

**Results**: Each flagged word appears as a Timeline card with the timecode, the detected text, the suspected error highlighted in context, and a suggested correction. For subtitle content, the full subtitle line is shown with the problematic word underlined.

---

### Safe Zone Violation

Checks whether on-screen text, graphics, or subtitles are positioned within the platform's required safe zone. Content outside the safe zone may be cut off, overlapped by platform UI, or flagged during QC review.

**Configuration**

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| Safe Zone Standard | Action Safe (90%) | Action Safe (90%) / Title Safe (80%) / Platform Specific / Custom | Title Safe is stricter and recommended for subtitle-heavy content |
| Target Platform | All Platforms | YouTube / TikTok / Instagram / Broadcast / All | Validates against overlay areas specific to the chosen platform |

**Results**: Violations appear on the Timeline with their timecode, a description of what element is outside the zone, and its distance beyond the boundary in pixels. A frame screenshot is included in Professional PDF exports.

---

### Subtitle Extraction

Automatically reads and extracts all subtitles and on-screen text from your video into a structured list. Useful for reviewing subtitle timing, content accuracy, and for generating a reference transcript.

**Configuration**

| Parameter | Default | Notes |
|-----------|---------|-------|
| Language Filter | All Languages | Filter extracted text to a specific language for mixed-language content |

**Results**: All recognised text events are listed in the Timeline tab under a dedicated Subtitles section, each with its timecode, duration, and the detected text. The full list can be downloaded as a plain-text file from the Export tab.

---

## Logo & Brand Detection

Two AI-powered checks scan every sampled frame for brand logos and on-screen branded products, flagging potential copyright and endorsement risks before publishing.

**Engine**: Grounding DINO v1 (zero-shot object detection) with a 4-stage aggregation pipeline. No brand sample upload required — detects logos without prior training on specific brands.

Enable these under Upload Settings → PlanD & AI Detection → Logo Detection.

---

### Logo Copyright Risk

Detects brand logos and trademarks visible in the frame that may require licensing clearance. Each detection is traced across consecutive frames to confirm it is a genuine, sustained appearance rather than a flash or artefact.

**Configuration**: No user-configurable parameters.

**Results**: Each detection appears as an **Amber** timeline marker with the timecode and duration of the logo appearance. A dashed bounding box tracks the logo position in the video player as you scrub through the flagged segment. The error card shows the model confidence score and detected label.

---

### Brand Product Risk

Identifies full branded products in frame — laptops, phones, packaging, and similar items — where the brand is clearly visible and may imply endorsement. Distinct from Logo Copyright Risk, which targets trademark symbols specifically.

**Configuration**: No user-configurable parameters.

**Results**: Flagged segments appear as **Orange** timeline markers. Each card includes a recommendation to confirm rights or obscure the product before publishing. The bounding box in the video player shows the tracked product region during playback.

---

## Check availability by plan

All 15 checks are available on every plan (Free, Pro, Ultra). Quota limits apply per plan — see [pricing](https://www.autoqc.app/pricing).

| Category | Checks | Requires PlanD enabled |
|----------|--------|------------------------|
| Video | 8 | No |
| Audio | 5 | No |
| AI Text Detection | 3 | Yes |
| Logo & Brand Detection | 2 | Yes |
