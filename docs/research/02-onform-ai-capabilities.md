# 02 — OnForm AI Capabilities: Real vs. Marketing

**Research question:** How much of OnForm's "AI" is genuine automated computer vision vs.
marketing language? How does it compare to competitors' AI claims?

**Compiled:** 2026-07-09 · agent-gathered web research.

---

## Bottom line

OnForm is **not** a pure manual telestration tool, and its "AI" is **substantially real**. As
of 2025–2026 it ships three genuinely automated CV features: (1) on-device 2D **skeleton/pose
tracking**, (2) **auto-detect recording**, and (3) a **markerless single-camera 3D motion-capture**
system that auto-computes 12 golf-swing metrics. That said, most *measurement* tools outside
those features (angles, distances, timing) remain **manual**, and OnForm's core identity is
still a coaching/telestration + messaging workflow. Notably, OnForm built the 3D system
**in-house** (its own motion-capture lab) with **no third-party CV/AI partner** and published a
**model card** — transparency rare in this space.

## 1. Confirmed automated AI functionality

### A. Skeleton Tracking (2D pose) — CONFIRMED AUTOMATED
- Auto-overlays a skeleton, marks joints, tracks them frame-by-frame; tapping a joint shows its
  **auto-computed angle**.
- Runs **offline, on-device**, requires Apple Neural Engine-class chip (iPhone XS / A12 / M1+).
- Limitation: "works best when there's only one person clearly visible."
- Sources: https://onform.com/blog/visualize-movement-with-onforms-skeleton-tracking-feature/ , https://support.onform.com/article/116-analysis-tools

### B. Auto-Detect Recording — CONFIRMED AUTOMATED
- AI detects a golf swing / baseball-softball swing / pitch and **automatically records** it,
  hands-free; up to 1080p @ 120 fps; voice-dictated notes per clip.
- Limited to **Golf, Baseball, Softball**.
- Sources: https://onform.com/blog/record-your-golf-swing-with-auto-detect/ , https://onform.com/blog/auto-detect-recording-now-available-for-baseball/ , https://support.onform.com/article/101-recording-modes

### C. Markerless 3D Motion Capture / "3D Data" — CONFIRMED AUTOMATED (biggest claim)
- Launched **September 30, 2025**. Single **face-on 2D video** (120/240 fps) → **3D model + 12
  swing metrics in seconds**, on-device, no markers/sensors, no internet.
- Auto-computed metrics: **torso rotation, hip/pelvis rotation, forward bend, side bend, pelvis
  lift, torso lift, thrust, sway**.
- **Built in-house**: OnForm constructed its own motion-capture lab using **OptiTrack**, validated
  against **electromagnetic (Polhemus EMS)** and **optical marker (Qualisys)** systems.
- Claims to be "the first markerless motion capture app to publish a complete model card"
  (published Sept 30, 2025; updated April 13, 2026).
  - **Note:** the model card's numeric error/MPJPE figures could not be extracted — the PDFs are
    image-based/rendered, not machine-readable. Specific accuracy numbers live only inside those
    PDFs: [Apr 2026](https://onform.com/wp-content/uploads/2026/04/Onform-3D-Golf-Swing-Pose-Estimation-Model_Apr-13-2026-v2.pdf)
    and the Sept 2025 version. **A human should open these to grade OnForm's 3D accuracy.**
- Sources: https://onform.com/blog/onform-launches-fast-reliable-and-accessible-markerless-3d-motion-capture-for-golf/ , https://onform.com/blog/feature-release-3d-data-now-available-for-golf/ , https://onform.com/sports/golf/

## 2. What is still MANUAL (not AI)

Per OnForm's KB (https://support.onform.com/article/116-analysis-tools): **Angles** (drawn by
hand outside skeleton/3D), **Measure** (manual calibration), **Stopwatch**, **Drawing/telestration**,
**Side-by-Side**, **Overlay**, **Voiceover**. Independent write-ups characterize OnForm as
**automated biomechanical visualization layered on a manual coaching/telestration + messaging
workflow**, not an "AI-first" auto-diagnosis engine.
([DeepSwing comparison](https://deepswing.io/blog/deepSwing-vs-onform-comparison-of-the-best-ai-golf-swing-analysis-apps))

## 3. Marketing-language caveats
- The "12 swing metrics in seconds" tagline is **backed by a real shipping feature** — but it is
  **golf-only** and **face-on only**.
- **Launch-monitor integrations** (Full Swing KIT, Garmin R10) are **hardware sensor feeds, NOT
  AI/CV**. https://onform.com/sports/golf/
- Skeleton tracking is marketed "any sport," but auto-detect and 3D are limited to
  golf/baseball/softball, and 3D metrics are **golf-only**.

## 4. AI/CV partnerships
**None found.** OnForm attributes the 3D system to its **own** lab and models — apparently
proprietary in-house tech, no external CV/AI vendor.

## 5. Competitor AI comparison

| Product | Real automated AI? | What it actually does |
|---|---|---|
| **OnForm** | Yes | On-device 2D pose + auto joint angles; auto-detect recording; **markerless single-camera 3D w/ 12 auto golf metrics** (Sept 2025); rest is manual telestration. |
| **Sportsbox AI (3D Golf)** | Yes (pioneer) | Patent-pending single-camera **markerless 3D** golf; 6 view angles; metrics in inches/degrees. The category creator OnForm is cloning. Described as a **measurement tool, not a coaching tool**. https://www.sportsbox.ai/ |
| **Hudl Technique (ex-Coach's Eye)** | Mostly NO | Primarily **manual slow-mo + telestration**. Separate **Hudl Studio** adds AI player-tracking for telestration graphics (team sports), not biomechanics. |
| **Dartfish** | Yes | Markerless AI that **auto-tracks joints, bones, angles, trajectories**. https://www.dartfish.com/motion/ |
| **SwingVision** | Yes (heavy) | Single-camera AI for tennis/pickleball: automated scoring, stats, shot tracking, real-time line calls. The most autonomous of the group. https://swing.vision/ |
| **Uplift Labs** | Yes | Markerless **3D biomechanics** in seconds, single camera, MLB/Driveline-trusted. |
| **OnBaseU** | N/A | Not a software product — a physical-assessment **certification/education** body. https://www.onbaseu.com/ |

## 6. User-described experience
- Coach reviews describe skeleton tracking as genuinely automatic. https://fastpitchlane.softballsuccess.com/tag/skeleton-tracking/
- App Store sentiment mixed-positive; some say it's "very golf specific," layout "clunky."
  https://apps.apple.com/us/app/onform-video-analysis-app/id1490334045
- No substantive independent hands-on accuracy tests of OnForm's 3D output surfaced (feature is
  recent, late 2025); third-party discussion is heavier on Sportsbox.

## Verdict
- **Real automated AI:** skeleton/pose tracking + auto joint angles; auto-detect recording;
  markerless single-camera 3D with 12 auto golf metrics (in-house, lab-validated, public model card).
- **Manual (not AI):** angle drawing, distance measure, stopwatch, telestration, overlays, voiceover.
- **Marketing framing to watch:** breadth overstated by omission — auto-detect and 3D are
  golf/baseball/softball-specific (3D = golf face-on only); launch-monitor "data" is sensor
  hardware, not AI.
- **Key open item:** the quantitative 3D accuracy (degrees vs. Qualisys/Polhemus) exists only
  inside the image-based model-card PDFs.
