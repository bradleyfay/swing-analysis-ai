# 03 — OnForm's Proprietary Moat & the Competitive Landscape

**Research question:** Is there a killer proprietary component (internal database, pre-analyzed
data, patents) that would make replicating OnForm infeasible for an independent developer?

**Compiled:** 2026-07-09 · agent-gathered web research.

---

## Bottom line

For the core product — telestration, slow-motion, frame-by-frame, side-by-side comparison,
cloud sharing/coaching messaging — there is **no proprietary data moat**. No secret database of
pre-analyzed swings, no proprietary "ideal technique" reference library gating the product, and
**no patents held by OnForm**. That entire category is commoditized (free open-source Kinovea
does most of it). The **only** hard-to-replicate asset is a recent (late-2025/2026) **markerless
3D golf-swing pose-estimation model**, whose moat is a physical motion-capture lab used to
generate ground-truth data — a real but narrow capital/time barrier, not an unobtainable secret dataset.

## 1. Proprietary database of pre-analyzed swings / "ideal" models?

**For the core app: No.** OnForm frames the product as software tooling (1080p/240fps capture,
playback, annotation, comparison, cloud). The "compare to a pro model" feature is comparison
against user-supplied or generic reference clips, not a gated proprietary library.
- https://onform.com/ · https://onform.com/sports/golf/ · https://www.techlearning.com/how-to/onform-how-to-use-it-for-school-athletics

**The one exception — a genuine proprietary dataset — is the 3D golf model.** To build/validate
it OnForm built their own **OptiTrack motion-capture lab** and validated against electromagnetic
(Polhemus) and optical marker (Qualisys) systems. This ground-truth effort is the closest thing
to a proprietary moat.
- https://onform.com/blog/onform-launches-fast-reliable-and-accessible-markerless-3d-motion-capture-for-golf/
- Model cards (unusually transparent): https://onform.com/wp-content/uploads/2026/04/Onform-3D-Golf-Swing-Pose-Estimation-Model_Apr-13-2026-v2.pdf

**Caveats limiting this moat:** golf-only, brand-new, and built on widely-published techniques.
Monocular 3D human-pose estimation is an active academic field with off-the-shelf building
blocks (MediaPipe/BlazePose) and golf-specific research (GolfPose; "Evaluating monocular 3D pose
models for golf"; GolfMate). Competitors already ship comparable 3D (Sportsbox AI, DeepSwing).
An independent developer could approximate it; matching OnForm's *validated accuracy* needs a
comparable (but achievable) data-collection/validation effort.
- https://dl.acm.org/doi/abs/10.1007/978-3-031-78305-0_25 (GolfPose)
- https://deepswing.io/blog/deepSwing-vs-onform-comparison-of-the-best-ai-golf-swing-analysis-apps

## 2. Patents
**No patents found assigned to OnForm / Onform Inc.** Strategy is public transparency (model
cards), not patent protection. Relevant patents belong to **competitors**:
- SwingVision markets "patented AI" for single-camera tennis. https://swing.vision/
- US 12,412,428 "AI-powered sports training system…" — assignee **Direct Technology Holdings Inc**, not OnForm.

## 3. Company background
- **Founded 2020**, HQ Colorado, **~13 employees**, small funding (~$300K early; a reported
  ~$2.67M round in 2024). Small team, not a deep-pocketed AI lab.
- **Founders: Krishna Ramachandran and Gear Fisher.** Ramachandran founded **Ubersense** (2011,
  2.5M+ users) → acquired by **Hudl** in 2014 → rebranded **Hudl Technique**. Fisher was at
  TrainingPeaks/Peaksware.
- **Key strategic point:** In **May 2021 OnForm re-acquired the Hudl Technique app and its user
  base from Hudl.** OnForm's real defensibility is this **installed user base, brand, and
  coaching-network distribution**, not proprietary technology.
- https://onform.com/about/ · https://onform.com/blog/onform-acquires-hudl-technique/ · https://siliconprairienews.com/2014/09/hudl-acquisition-fourth-company/

Tech-stack signals: cloud/AI/ML positioning; 3D work implies a CV/pose-estimation stack
(industry-standard Python/PyTorch/OpenCV + on-device mobile inference). No unique stack surfaced.

## 4. Competitive landscape — how differentiated is OnForm?

The video-coaching tooling market is crowded and partly commoditized:
- **Coach's Eye (TechSmith): discontinued** — sunset 2021, fully shut down **Sept 15, 2022**.
  https://www.jasongaylord.com/blog/2021/09/17/techsmith-coaches-eye-retiring
- **Hudl Technique: absorbed into OnForm** (2021).
- **Kinovea: free and open-source** — frame-by-frame, angle/kinematic measurement, comparison.
  Proves the core feature set has zero data moat. https://speedendurance.com/2012/03/15/dartfish-alternative-kinovea/
- **Dartfish** (deeper pro tooling), **V1 Sports** (golf, instructor marketplace),
  **SwingVision** (AI tennis/pickleball, patented, ex-Tesla/Apple team).

OnForm's differentiation vs. these is mostly **UX polish, multi-cam/240fps capture, coaching
workflow/messaging, cloud sync, inherited user base** — plus the golf 3D model. None of the
first group is a proprietary-data barrier.

## 5. Software tooling vs. proprietary data/algorithms
- **Core value ≈ software tooling + distribution/user base.** Fully replicable.
- **Emerging value = the 3D biomechanics algorithm**, backed by an in-house mocap-lab dataset —
  the single component that costs real time/money to match, but golf-only, buildable with public
  methods, and already has competitors.

## Directly addressing the concern

> "I'm confident I can build this UNLESS there's a killer proprietary component like an internal
> database or pre-analyzed data."

**There is no such killer component for the core coaching app.** OnForm has no patents, no gated
database of pre-analyzed swings, no proprietary "ideal technique" library the product depends on,
and a feature set free/open-source software already replicates. Durable advantages are **brand +
inherited Ubersense/Hudl Technique user base + coaching distribution** — business/GTM moats, not
technical ones. The **one** technical asset with a real barrier is the markerless 3D golf model +
OptiTrack lab; matching validated 3D accuracy means a data-collection/validation effort (or
leaning on existing pose SDKs + published golf-pose research). For the 2D
telestration/comparison/coaching-sharing experience, there is **no proprietary data or algorithm
standing in the way**.
