# 06 — Existing Baseball Swing AI Products & Research (Closest Analogs)

**Research question:** What products and academic work already analyze baseball (and adjacent)
swings with AI/CV? What are the closest analogs to "AI analysis of a baseball swing from phone
video," and what can we learn from how they were built?

**Compiled:** 2026-07-09 · agent-gathered web research.

---

## Headline finding

The exact product — **"3D biomechanical analysis of a baseball swing from ordinary phone video"**
— **does not yet exist as a mature consumer product, but every technical piece needed to build it
has already been demonstrated.** Closest working analogs: **Sportsbox AI** (golf, single phone
video → 3D), **Mustard** (baseball pitching, phone video → skeletal biomech report card),
**SwingVision** (tennis, on-device phone AI), **Theia Markerless** (lab-grade video bat tracking,
multi-camera). Driveline and MLB Statcast prove the bat-tracking-from-video component works.

## 1. Landscape map — data source is the key axis

| Product | Data source | What it measures | "AI" content | Analog strength |
|---|---|---|---|---|
| **Blast Motion** | Bat-knob IMU | Bat speed, attack angle, time-to-contact, VBA, on-plane %, rot. accel | Low (signal processing) | Weak (sensor) |
| **Diamond Kinetics** | Bat-knob IMU | Barrel speed, max accel, attack angle, hand speed, 3D bat path | Low (sensor fusion) | Weak |
| **HitTrax** | Stereo cameras (ball) | Exit velo, launch angle, spray, distance | Low | Weak (ball, not swing) |
| **K-Motion / K-Vest** | 4 wearable IMUs | Kinematic sequence, segment velocities, X-factor | Low-med | Medium (right thing, wrong hardware) |
| **Uplift Labs** | 2 iPads (markerless) | 3D kinematics, sequencing | **High** | **Strong** |
| **Mustard** | Phone video (1–2 angles) | 11 biomech variables, pass/fail report card, key-frame skeleton | **High** | **VERY STRONG (baseball, phone)** |
| **Sportsbox AI** | Single phone video | 3D turn/bend/side-bend/sway/lift, 6 view angles | **High** | **VERY STRONG (single-video 3D)** |
| **SwingVision** | Single phone camera (+ Watch IMU) | Ball tracking, line calls, shot stats, swing video | **High** | **Strong (product/build model)** |
| **Theia Markerless** | 8+ high-speed cameras (300fps) | 3D bat trajectory + 124-pt body kinematics, <3° bat-plane error | **High** | Strong (accuracy ceiling), not phone |
| **MLB Statcast (Hawk-Eye)** | 12 stadium cameras (5 @ 300fps) | Bat speed, swing length, full limb + bat tracking | **High** | Reference (infrastructure) |

Key insight: **sensor products (Blast, DK, K-Vest) are barely "AI"** (signal processing on IMU
streams). The **camera/vision products (Mustard, Sportsbox, Uplift, SwingVision, Theia) are the
real AI plays**, measuring via pose estimation + 3D reconstruction.

## 2. The two closest analogs, in depth

### Sportsbox AI (golf) — closest for "single video → 3D"
- **Input:** a single 2D phone video (ideally slow-mo, face-on). No markers/sensors.
- **Pipeline:** (1) **Video→3D** tracking **30+ keypoints on body, club, ball**; (2) **Visualization**
  from **6 virtual camera angles** from that one real video; (3) **Measurement** in inches/degrees
  (turn, bend, side-bend, flexion/extension, sway, lift, thrust).
- **Feedback:** coaches set "goal ranges"; compare against tour-pro model ranges; auto-detects swings.
- Built in-house by AI scientists + a biomechanist. Holds a **cluster of US patents
  (11,941,916 B2 – 12,008,839 B2).**
- **Caveat:** publishes **no quantified accuracy/validation**. Integrated with **Foresight Sports**
  launch monitors.
- **Lesson:** monocular-video→3D is commercially viable and patent-defensible for a rotational
  swing; a baseball equivalent is a near-direct port. https://www.sportsbox.ai/ · https://www.prnewswire.com/news-releases/foresight-sports-and-sportsbox-ai-industry-first-launch-monitor-technology-integrated-with-3d-motion-swing-capture-available-now-302173803.html

### Mustard (baseball) — closest by sport & form factor
- **Input:** cell-phone 2D video, **two angles**. Any phone, no sensors.
- **Pipeline:** pose estimator extracts a 2D skeleton → **projects onto a 3D model**; auto-detects
  key still frames; overlays a skeleton.
- **Output:** grades **11 biomechanical variables** into a weighted **"Mustard Report Card"
  efficiency score in ~45 seconds**, then prescribes corrective drills.
- **The "expert-in-the-loop" moat:** anchored on **Tom House's ~40-year database of 3D analyses of
  ~900 pro pitchers**. The domain expertise defines "good"; the AI recognizes deviations.
  Integrated with **Pocket Radar** for velocity.
- **Important caveat:** shipping product is **pitching-first**; hitting is the weaker/less-proven
  side — **a live gap a swing-focused product could own.**
- **Lesson:** phone-video → pose → 3D-projection → scoring-against-expert-baseline is already
  shipping for baseball. The differentiator is the **expert-defined rubric**, not the raw CV.
  https://teammstrd.com/ai_change_your_game/ · https://www.baseballamerica.com/stories/new-mustard-app-giving-tom-house-specific-mechanics-coaching-via-phone/

## 3. SwingVision (tennis) — the model for HOW to BUILD a single-app AI sports product
- **Founders:** Swupnil Sahai (led object tracking for Tesla Autopilot) + Richard Hsu; App Store
  ~2019/2020.
- **Architecture:** **fully on-device** via **Apple Neural Engine / Core ML** across
  iPhone/iPad/Watch. Real-time, **no internet**. Single camera behind the baseline. Optional Apple
  Watch IMU for swing speed.
- **Training scale:** ball model trained on **500M+ shots from 200,000+ players**; 97–99% line-call
  accuracy.
- **Business:** raised **~$10M**; a **$6M Series A** (Andy Roddick wrote the first $50k check).
  Apple has featured it repeatedly.
- *[VERIFIED note: the linked Apple Newsroom piece confirms the founders, on-device Neural Engine
  approach, and Roddick as an investor/advisor, but does NOT contain the funding figures ($10M /
  $6M Series A / $50k first check) or the training-scale/accuracy numbers (500M shots, 200k
  players, 97–99%) — those trace to other sources and should be re-confirmed before quoting.]*
- **Lessons:** (1) on-device inference removes cloud cost/latency and is a differentiator; (2) a
  **data flywheel** (user videos → better models) is the moat; (3) **athlete/celebrity investors**
  double as distribution; (4) single fixed camera + strong CV beats requiring extra hardware.
  https://www.apple.com/newsroom/2022/05/swupnil-sahai-and-his-co-founder-serve-an-ace-with-ai-powered-swingvision/ · https://swing.vision/

## 4. Bat tracking from video — the hardest baseball-specific sub-problem, solved at 3 tiers

**Tier 1 — Single camera, buildable (Driveline's public writeup):**
- **A custom 2-keypoint pose model** to track **cap (barrel end) + knob (handle)**. *[CORRECTED
  2026-07-09: an earlier draft said "YOLOv8 pose" — the article explicitly took "a different
  approach" and rejected YOLOv8 in favor of a custom pose model. All other Tier-1 numbers below
  are confirmed verbatim against the article.]*
- Bootstrapped from ~1,000 manually-annotated side-view images (~10,000 for the ball via
  **semi-automatic annotation**).
- **Single side-view camera** (an Edgertronic high-speed camera); **known bat length as in-frame
  scale reference** to recover real-world dimensions from 2D.
- **Motion-blur insight:** even when bounding-box confidence dropped from blur, **keypoint
  predictions stayed accurate** — filter on per-keypoint confidence to salvage frames.
- **Validation:** ~2.4–2.6 px mean keypoint error, ~10s turnaround. https://www.drivelinebaseball.com/2025/02/bat-tracking-computer-vision-and-the-next-frontier/

**Tier 2 — Lab-grade multi-camera (Theia Markerless):** 8+ cameras @ 300fps, 124+ body points + bat
handle/tip, **<3° bat-plane error** over 2,000+ swings; joints within ~1cm. Needs a rig, not a
phone. https://www.theiamarkerless.com/blog/markerless-bat-tracking-q-a-for-pro-baseball-performance-staff

**Tier 3 — Infrastructure (MLB Statcast/Hawk-Eye):** **5 high-frame-rate cameras** (the cited
MLB.com article specifies five; Hawk-Eye deploys ~12 cameras total per park, of which the five
high-FPS units drive bat tracking). MLB spent **2+ years refining the bat-tracking model** before
release — a signal of how hard metric-grade bat tracking is. https://baseballsavant.mlb.com/leaderboard/bat-tracking

**Open-source building blocks:** **BaseballCV** (Dylan Drummey & Carlos Marcano) — free
baseball-specific CV models + datasets (pitcher/hitter/catcher detectors, downloadable weights).
https://github.com/BaseballCV/BaseballCV · https://baseballcv.com/datasets
*(Note: the previously-listed `kushaldev75/Bat-Ball-Tracking-System` repo is a **cricket** project,
not baseball — removed here to avoid miscategorization.)*

## 5. Academic / research foundations
- **The rotation problem (critical):** pose estimators output **one point per joint**, but swings
  are dominated by **rotational motion** needing **≥2 points per joint**. Solutions: **MeTRAbs**
  (multiple points per joint) and **OpenCap** (LSTM trained on paired 3D-pose + Vicon to infer
  marker positions). *The core technical hurdle for a phone-based swing biomechanics tool.*
  *[VERIFIED note: the linked PMC12378739 is a real general pose-framework mini-review that
  supports the out-of-plane/rotation limitation but does NOT itself name MeTRAbs or OpenCap —
  those specific solutions come from their own primary sources and should be cited directly.]*
  https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12378739/
- **Single-camera 3D lift validated in literature:** https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10951609/
  (genuine single-camera validation — supports the claim). *[CORRECTED 2026-07-09: PMC7739760 was
  also cited here but it is a **multi-camera** (5-camera) OpenPose accuracy study — it does NOT
  validate single-camera lift and was removed to avoid contradicting the claim.]*
- **Baseball-specific:** "Baseball Action Classification Based on OpenPose"; "Baseball Batting
  Movement Analysis System."
- **3D biomech from a SINGLE broadcast angle:** Baseball Prospectus extracted **3D MLB biomech data
  from single-angle broadcast feeds** — proof monocular 3D lift works on real baseball footage.
  https://www.baseballprospectus.com/news/article/93823/
- **Metric reliability caution:** arXiv "Swinging, Fast and Slow" — even MLB's metrics carry
  meaningful variance; manage accuracy expectations. https://arxiv.org/pdf/2507.01238
- **Frameworks:** OpenPose, MediaPipe, AlphaPose, DensePose; **MeTRAbs** and **OpenCap** for the
  rotational/3D-marker problem.

## 6. Synthesis — building "AI baseball swing analysis from phone video"
1. **It's a monocular-3D-lift problem, solved next door.** Sportsbox (golf) and Mustard (baseball
   pitching) both ship "single/dual phone video → 3D biomechanics." The baseball *swing* version is
   a recognized gap, not an unsolved research problem.
2. **Two-part pipeline:** (a) body pose → 3D (MediaPipe/MeTRAbs/OpenCap-style lifting), (b) bat
   keypoint tracking (Driveline's YOLO-pose cap+knob, bat length as scale). Bat = baseball-specific
   hard part; budget for motion blur + per-keypoint confidence filtering.
3. **The moat is the biomechanical rubric, not the raw CV.** (Mustard/Tom House; Sportsbox/biomechanist.)
4. **Ship on-device (Apple Neural Engine / Core ML).** (SwingVision.)
5. **Build the data flywheel early** — bootstrap with a few thousand annotated frames (Driveline
   ~1k→10k via semi-auto labeling) and compound.
6. **Manage accuracy honestly** — decide consumer-aid tier (Mustard/Sportsbox) vs. lab-replacement
   (Theia/Uplift/K-Motion).
7. **Leverage existing open source:** BaseballCV + MediaPipe/MeTRAbs/OpenCap cut the cold-start.
8. **Distribution playbook:** athlete-endorser strategy + hardware partnerships (ball data the
   camera can't see).

**Bottom line:** a real white space — **no mature phone-only markerless 3D baseball-*swing*
analyzer** — but Sportsbox, Mustard, Driveline, and SwingVision collectively de-risk essentially
every component.
