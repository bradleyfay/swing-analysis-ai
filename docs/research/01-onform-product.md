# 01 — OnForm Core Product & Coaching Workflow

**Research question:** What is OnForm's complete feature set, and is its analysis primarily
*manual* (coach draws + records voice feedback) or *automated* (software auto-detects and
analyzes)?

**Compiled:** 2026-07-09 · agent-gathered web research. Vendor/marketing claims flagged inline.

---

## Bottom line

OnForm is fundamentally a **manual video-coaching tool with an AI-assisted layer bolted on
top** — not an automated swing-analysis engine. The core workflow is: a coach records or
receives an athlete's video, then **manually** scrubs frame-by-frame, draws lines/angles/circles,
records a voiceover, and sends it back. This is the same "telestration + voice feedback"
paradigm as the old V1 Golf / Coach's Eye / CoachNow category. The genuinely automated pieces
are three narrower AI features — **skeleton-tracking overlay**, **auto-detect recording**, and
(golf-only) **markerless 3D motion capture** — which augment but do not replace the coach's
manual interpretation. OnForm's own competitive marketing notably avoids claiming automated
analysis and instead emphasizes manual annotation tools and convenience automation
([onform.com competitor blog](https://onform.com/blog/onform-vs-v1-coachnow-and-sportsbox-the-best-video-analysis-tool-for-coaches/)).

## 1. Complete feature set

### Video capture ([onform.com](https://onform.com/), [iOS user guide](https://support.onform.com/article/153-user-guide-onform-video-analysis-app))
- **1080p HD at up to 240 fps**, "5 recording modes."
- **Manual shutter speed** control to eliminate motion blur on fast movements.
- **Five recording modes**: Manual, One-Tap (buffer before/after tap), **Auto-Detect** (AI
  auto-clips athletic movements), and **Multi-Cam** (up to 4 synced iOS devices). Also
  captures/imports from external cameras; supports wireless multi-cam.

### Playback & analysis tools ([Analysis Tools KB](https://support.onform.com/article/116-analysis-tools))
Every one of these is **coach-operated / manual** except skeleton tracking:
- **Slow motion & frame-by-frame scrubbing** — manual.
- **Drawing / telestration**: lines, arrows, circles, angles, text — manual.
- **Angles tool**: coach places the markers manually.
- **Side-by-side comparison** — manual selection and sync.
- **Overlay / ghosting** — manual.
- **Stopwatch** — manual.
- **Measure tool**: coach manually calibrates against a known reference length — manual.
- **Skeleton tracking** — *AI/automated* (see below).
- **Voiceover**: "Record Screen & Microphone" or "Screen Only"; creates a new independent
  file, leaving original footage untouched — manual.
- **Multi-angle 4-Way Video Player**: up to 4 angles (+ 3D model) synced.

### The three genuinely automated (AI) features
1. **Skeleton Tracking** ([blog](https://onform.com/blog/visualize-movement-with-onforms-skeleton-tracking-feature/), [KB](https://support.onform.com/article/116-analysis-tools)):
   AI overlays a skeleton tracking key body points. Runs **offline, on-device**. Coach can tap
   joints to read angles. Limitation: best with **one clearly visible person**; degrades with
   multiple people or occlusion.
2. **Auto-Detect Recording** ([user guide](https://support.onform.com/article/153-user-guide-onform-video-analysis-app)):
   AI detects the movement, auto-clips it; for golf, computes 3D data automatically.
3. **Markerless 3D Motion Capture (Golf only)** ([3D launch blog](https://onform.com/blog/onform-launches-fast-reliable-and-accessible-markerless-3d-motion-capture-for-golf/),
   [golf page](https://onform.com/sports/golf/),
   [model card PDF Apr 2026](https://onform.com/wp-content/uploads/2026/04/Onform-3D-Golf-Swing-Pose-Estimation-Model_Apr-13-2026-v2.pdf)):
   - Marketing claim: **"Turn simple 2D video into detailed 3D analysis with 12 swing metrics in seconds."**
   - AI pose estimation builds a full 3D skeleton (~10 s) from a **single face-on iPhone/iPad video**; two devices improve accuracy.
   - Outputs: torso & hip rotation, forward/side bends, pelvis & torso lift, thrust, sway.
   - **Processing is on-device.**
   - **Validation claim (vendor-reported):** trained in OnForm's own motion-capture lab using
     **OptiTrack**, tested against electromagnetic (EMS) and optical markered systems.

**Caveat:** The 3D metric generation is genuinely automated but confined to **golf** and gated
behind the top-tier **Coach Pro** plan. There is no auto-generated coaching *verdict* or
corrective instruction — the software surfaces data/visuals; the human coach interprets and instructs.

## 2. The coaching workflow ([iOS user guide](https://support.onform.com/article/153-user-guide-onform-video-analysis-app))

The KB describes the blend explicitly as **"semi-automated: AI handles detection and skeleton
computation, but coaches manually apply drawings, voiceovers, and interpretation."** Typical loop:
1. Record (choosing a mode) or receive an athlete-uploaded video.
2. Scrub frame-by-frame / slow-mo.
3. Manually draw; optionally toggle skeleton overlay or (golf) 3D model.
4. Record a voiceover walkthrough while drawing → new shareable file.
5. Share into the athlete's workspace or via secure link; attach chat text.

This is a coach-driven feedback cycle, **not** an automated report generator.

## 3. Sports targeted ([onform.com/sports](https://onform.com/sports/))
Golf, Baseball, Softball, Swimming, Track & Field, Weightlifting, Rowing, Gymnastics, Soccer,
Running, Skiing. **Golf is the flagship** — the only sport with 3D metrics and launch-monitor
integration (**Full Swing KIT**, **Garmin R10**). Other sports get the generic toolset.

## 4. Collaboration ([iOS user guide](https://support.onform.com/article/153-user-guide-onform-video-analysis-app))
- **Workspaces** (Individual 1:1 or Team), **Collections**, **Library – All Files**.
- **Messaging**: 1:1 and group chat, threads, pinned messages, scheduled delivery, broadcast.
- **Sharing**: in-app or external secure links (email/SMS).
- **Cloud storage**: automatic backup; markets "unlimited cloud storage" (claim; per-plan video
  caps below suggest "unlimited" applies to higher tiers).
- **Team management**: groups, bulk communication/export.

## 5. Platform ([web sign-in](https://onform.com/web-app-sign-in/), [Android launch](https://onform.com/blog/onform-for-coaches-is-now-on-android/), [Google Play](https://play.google.com/store/apps/details?id=com.app.OnForm))
- **iOS = primary, full-featured.** Multi-cam, 3D, auto-detect, manual shutter are iOS-centric.
- **Android = limited.** Confirmed: coach account, invite students, record/review, voiceover +
  drawing, slow-mo/frame-by-frame, side-by-side, chat. OnForm states Android "doesn't have every
  feature." (Inference: multi-cam, 3D, manual-shutter are iOS-only.)
- **Web app = "lite"**, requires an account first created via iOS.
- **Processing**: skeleton tracking and 3D run on-device/offline; storage/sharing is cloud-backed.

## 6. Pricing & target market ([pricing](https://onform.com/pricing/))

**Coach plans:**

| Plan | Price | Key limits/features |
|---|---|---|
| Coach Basic | $19.99/mo ($199.99/yr) | Up to 2,000 videos, 20 voiceovers/mo, 20 workspaces, 2-angle multi-cam |
| Coach Standard | $39.99/mo ($399.99/yr) | Unlimited videos & voiceovers, unlimited teams, 2-angle multi-cam, launch-monitor compatible |
| Coach Pro | $59.99/mo ($599.99/yr) | **Full 3D golf visualizations + data**, 4-angle multi-cam, 60-min recording, studio solution |

**Athlete plans:** Individual Basic (free when linked to a Coach Basic subscriber; review-only);
Individual Standard $9.99/mo (free with Coach Standard/Pro); Individual Premium $14.99/mo.

**Positioning:** cheapest/most usable vs V1 ($69.99), Sportsbox ($79.99), matching CoachNow
($49.99) ([competitor blog](https://onform.com/blog/onform-vs-v1-coachnow-and-sportsbox-the-best-video-analysis-tool-for-coaches/)).
Target = individual private coaches/instructors, academies, school/team programs.

## Confirmed vs. claim vs. inference

- **Confirmed (KB/docs):** manual drawing/angle/measure/overlay/side-by-side/voiceover; skeleton
  tracking is AI on-device; auto-detect recording; workflow is "semi-automated" per OnForm's own
  words; platform tiering; pricing.
- **Vendor marketing claims (skeptical):** "12 swing metrics in seconds," 3D accuracy vs
  OptiTrack/EMS, "unlimited cloud storage."
- **Inference:** 3D auto-analysis is golf-only and Pro-tier-gated; the product does **not**
  auto-diagnose faults or generate coaching instructions; specific iOS-only Android gaps.

## Sources
- https://onform.com/ · https://onform.com/sports/ · https://onform.com/sports/golf/
- https://support.onform.com/article/116-analysis-tools
- https://support.onform.com/article/153-user-guide-onform-video-analysis-app
- https://onform.com/pricing/
- https://onform.com/blog/onform-vs-v1-coachnow-and-sportsbox-the-best-video-analysis-tool-for-coaches/
- https://onform.com/blog/onform-launches-fast-reliable-and-accessible-markerless-3d-motion-capture-for-golf/
- https://onform.com/blog/visualize-movement-with-onforms-skeleton-tracking-feature/
- https://onform.com/blog/onform-for-coaches-is-now-on-android/
- https://onform.com/web-app-sign-in/
- https://play.google.com/store/apps/details?id=com.app.OnForm
- https://onform.com/wp-content/uploads/2026/04/Onform-3D-Golf-Swing-Pose-Estimation-Model_Apr-13-2026-v2.pdf
