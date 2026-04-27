# Changelog

All notable changes to AutoQC are documented here.

**Product**: [autoqc.app](https://www.autoqc.app) · **Full release notes with screenshots**: [autoqc.app/resource/whats-new](https://www.autoqc.app/resource/whats-new)

---

## [v1.03] — April 2026

### Logo & Brand Detection

Auto QC now checks every sampled frame for brand logos and on-screen products. Anything that may need a rights review gets a timecoded marker in the report. No configuration required.

**Logo Copyright Risk**
- Detects brand logos and trademarks that may require licensing clearance
- Each detection appears as an **Amber** timeline marker with timecode and duration
- A dashed bounding box tracks the logo position in the video player as you scrub through the flagged segment
- Error card shows the model confidence score and detected label for manual review

**Brand Product Risk**
- Flags full branded products in frame (laptops, phones, packaging) that may imply brand endorsement
- **Orange** markers distinguish product placement risk from logo copyright risk on the timeline
- Each flag recommends confirming rights or obscuring the product before publishing

**How to enable**
- Upload Settings → PlanD & AI Detection → Logo Detection
- No threshold to set — runs automatically with fixed defaults
- Results appear in the same QC report alongside all other timeline errors

---

## [v1.02] — April 2026

### Version Control — Multi-version QC

Upload a revised version of any video directly from the dashboard or analysis result page. Auto QC runs a fresh quality check on the new version and automatically re-applies any errors you previously dismissed — even when the edit shifted the timeline.

**Version control**
- Upload a new version from the dashboard — no need to reconfigure quality check settings
- Version badge on each dashboard card showing V1, V2, V3...
- Dashboard always shows the latest version; older versions are archived
- Analysis result page shows version history and lets you switch between versions

**Smart inheritance**
- Dismissed errors are automatically re-applied to the new version
- Audio cross-correlation aligns timecodes across re-encoded or trimmed revisions
- Global errors (aspect ratio, loudness) are inherited by type — no timecode needed
- Timeline errors are matched with ±0.5s tolerance after alignment
- Low-confidence alignment falls back gracefully with a manual review prompt

**Analysis settings**
- Quality check settings are automatically inherited from the previous version
- Platform compliance preset, detector toggles, and PlanD config all carry over

---

## [v1.0.1] — March 2026

### Subscription Plans, Billing Portal & Account Management

Auto QC now supports Free, Pro, and Ultra subscription tiers. Manage your plan, track quota usage in real time, download invoices, and update payment methods — all from the new Settings pages.

**Subscription tiers**

| Plan | Quota | Storage | Price |
|------|-------|---------|-------|
| Free | 10 min / month | 1 GB | $0 |
| Pro | 60 min / month | 10 GB | $29.99/month |
| Ultra | 300 min / month | 50 GB | $89.99/month |

- One-click upgrade via Stripe Checkout
- Cancel anytime — subscription continues until end of billing period
- Automatic downgrade to Free when subscription expires

**Billing**
- Invoice history with PDF download
- Stripe Customer Portal — update payment method, manage subscription
- Payment status tracking: active / past due / canceling / action required
- Automatic recovery after failed payment (Dunning support)
- 3D Secure authentication support

**Account**
- Real-time quota usage: analysis minutes and storage
- Visual quota progress bars with usage percentage
- Settings sidebar: Subscription, Billing, Profile, Notifications, Security

---

## [v1.0.0] — March 2026

### Initial Release

The first public version of AutoQC. Upload a video, run automated quality checks, and get a broadcast-grade QC report in minutes — no manual frame scrubbing required.

**Video checks**
- Black Frame Detection
- Flash Frame Detection
- Bad Edit Detection
- Freeze Frame Detection
- Blur Detection
- Brightness Analysis
- Border (Black Bar) Detection
- Aspect Ratio Check

**Audio checks**
- Loudness Compliance (EBU R128 / LKFS)
- Audio Clipping Detection
- Background Noise Analysis
- Silence Detection
- Speech Clarity Analysis

**AI Detection (PlanD Engine)**
- Subtitle & on-screen text check across 109 languages
- Typo Detection
- Safe Zone Violation Check
- Subtitle Extraction & Refinement

**Platform compliance**
- Presets: YouTube · TikTok · Instagram · Broadcast TV · Custom
- Timeline annotations with frame-accurate timecodes
- Broadcast-grade PDF report export
