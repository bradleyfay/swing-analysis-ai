# Research Dossier — Feasibility of an Open Baseball Swing Analyzer

This directory preserves the primary research behind the Sandlot vision and its two founding
questions. It exists so the reasoning and sources survive into technical design and are not
lost to conversation history.

**Compiled:** 2026-07-09
**Method:** Seven parallel research agents, each assigned one facet of the problem, sweeping
the public web (product docs, academic papers, model repos, licenses, reviews). Every
significant claim carries a source URL. Vendor/marketing claims are flagged as such and
treated with the skepticism they deserve; distinguish them from confirmed documentation and
from analyst inference.

**Citations verified:** 2026-07-09. A second pass of four agents fetched **every cited URL**
(~110 links) and checked each for (a) liveness and (b) whether the page actually supports the
attributed claim. Results and the corrections applied are in the **[Verification log](#verification-log)**
at the bottom of this file. Inline `[VERIFIED …]` / `[CORRECTED …]` notes mark the specific
claims that were adjusted.

> **Caveat on freshness & accuracy.** These are agent-gathered web-research summaries, not
> peer-reviewed. As of the verification pass above, **no cited URL was dead or fabricated**, and
> **every model/dataset license in the decisive tables checked out against its actual repo.** But
> some citations supported the *topic* rather than the *exact figure* attached to them (now
> annotated inline). Prices, features, and licenses change — re-verify anything load-bearing
> before you build on it, especially exact license terms.

---

## The two questions, answered

**Q1 — Is it feasible to replicate OnForm's analysis/coaching capabilities independently?**
**Yes, decisively.** There is no killer proprietary component — no gated database of
pre-analyzed swings, no "ideal technique" library the product depends on, and OnForm holds
**zero patents**. Its core feature set (telestration, slow-mo, frame-stepping, side-by-side,
cloud sharing) is commoditized to the point that free open-source software (Kinovea) already
replicates most of it. OnForm's moat is **business, not technical** (brand + an inherited
user base). Its one real technical asset — a markerless 3D *golf* pose model backed by an
in-house motion-capture lab — is golf-only, recent, buildable from published techniques, and
already has direct competitors.

**Q2 — Minimum functionality to analyze a baseball swing with AI feedback?**
A six-stage on-device pipeline: **capture (high-FPS) → pose extraction → phase segmentation →
compare-to-reference (DTW) → root-cause fault mapping → grounded LLM narration.** The exact
product (markerless 3D baseball-*swing* analysis from phone video) does not yet exist as a
mature product, but every component is de-risked by adjacent work (Sportsbox AI, Mustard,
SwingVision, Driveline). See [`07`](07-ai-coaching-feedback.md) and
[`05`](05-swing-biomechanics-and-datasets.md) for the pipeline detail.

---

## Contents

| # | File | What's in it |
| --- | --- | --- |
| 01 | [`01-onform-product.md`](01-onform-product.md) | OnForm's complete feature set and coaching workflow (manual telestration vs. automated analysis) |
| 02 | [`02-onform-ai-capabilities.md`](02-onform-ai-capabilities.md) | How much of OnForm's "AI" is real vs. marketing; competitor AI comparison |
| 03 | [`03-onform-moat-and-competitors.md`](03-onform-moat-and-competitors.md) | The proprietary-moat question: patents, datasets, company background, competitive landscape |
| 04 | [`04-pose-estimation-models.md`](04-pose-estimation-models.md) | Open-source pose models with a hard focus on commercial licensing and fast-motion suitability |
| 05 | [`05-swing-biomechanics-and-datasets.md`](05-swing-biomechanics-and-datasets.md) | Swing biomechanics, which metrics are measurable from video, and open reference datasets |
| 06 | [`06-existing-products-and-research.md`](06-existing-products-and-research.md) | Existing baseball/adjacent swing-analysis products and academic research; closest analogs |
| 07 | [`07-ai-coaching-feedback.md`](07-ai-coaching-feedback.md) | Turning pose/biomechanics data into coaching feedback (the "AI coach" layer) |

---

## The load-bearing findings (quick reference)

**Licensing landmines** (from [`04`](04-pose-estimation-models.md)):
- **OpenPose** — commercial license is $25k/yr *and its terms explicitly exclude the field of
  Sports.* Disqualified regardless of intent.
- **Ultralytics YOLO (v8/11-pose)** — AGPL-3.0; forces open-sourcing the whole app unless you
  buy the Enterprise license. (For an open project this is acceptable, but know it going in.)
- **VideoPose3D** — CC BY-NC 4.0, non-commercial. Prefer **MotionBERT** (Apache-2.0).
- **Recommended permissive core:** RTMPose/RTMO/RTMDet (Apache-2.0), MediaPipe (Apache-2.0),
  MotionBERT (Apache-2.0), Pose2Sim (BSD-3).

**Reference data** (from [`05`](05-swing-biomechanics-and-datasets.md)):
- **Driveline OpenBiomechanics Project** — ~98 elite hitters with synchronized marker mocap,
  force plates, bat kinematics, Blast cross-reference, HitTrax outcomes, defined swing events,
  *and* a computer-vision module with synchronized video. **License: CC BY-NC-SA 4.0, with an
  explicit bar on pro-sports-orgs and commercial use.** For a non-commercial project this is
  usable both as normative "good swing" ranges and as ground truth to validate a markerless
  pipeline.
- **BaseballCV** — permissive baseball-specific CV models/datasets; a real cold-start head-start.

**The core technical split** (from [`05`](05-swing-biomechanics-and-datasets.md)):
Body-pose keypoints strongly support the **body-mechanics** side (kinematic sequence,
hip-shoulder separation, head stability, front-leg block, posture, phase timing). True
**bat metrics** (bat speed, attack angle, swing plane) require a **separate bat-tracking CV
model** (Driveline's cap+knob keypoint approach, using known bat length as scale) or an on-bat
sensor. Treat body mechanics and bat metrics as two distinct measurement problems; body
mechanics first.

**The closest existing analogs** (from [`06`](06-existing-products-and-research.md)):
Sportsbox AI (golf, single phone video → 3D), Mustard (baseball, phone video → biomech report
card, pitching-first — *hitting is an open gap*), SwingVision (tennis, fully on-device Core ML).

**The feedback architecture** (from [`07`](07-ai-coaching-feedback.md)):
SportsGPT's align → assess → narrate. Deterministic modules do the biomechanics; the LLM is a
strictly-grounded narration layer fed structured facts, never raw keypoints. Diagnose
root-cause not symptom; surface one fix at a time; prescribe a drill.

---

## Verification log

On **2026-07-09**, four agents fetched every cited URL across all seven reports (~110 links) and
checked each for liveness and claim-support. Summary:

**What held up (the load-bearing findings):**
- **No dead links and no fabricated sources** across the whole dossier. Every URL resolves (a few
  return bot-level 403/paywall redirects but are genuinely live).
- **Every model/dataset license in the decisive tables is correct** against its actual repo:
  RTMPose/MMPose/MotionBERT/MediaPipe (Apache-2.0), MHFormer (MIT), Pose2Sim (BSD-3), ViTPose code
  (Apache-2.0), Ultralytics YOLO (AGPL-3.0), VideoPose3D (CC BY-NC).
- **OpenBiomechanics** — repo path `github.com/drivelineresearch/openbiomechanics` resolves and its
  **MIT-code / CC BY-NC-SA-4.0-data / pro-org+financial-firm exclusion** license is confirmed nearly
  verbatim. This is the dossier's most load-bearing claim.
- **All four recent arXiv papers are real** with matching titles: SportsGPT (2512.14121), BioCoach
  (2603.26938), Talking Tennis (2510.03921), "Swinging, Fast and Slow" (2507.01238). AgentCoach
  (CHI 2026 ACM DOI) and the Nature/MDPI/PMC papers in files 05/07 are all real.

**Corrections applied inline (search the files for `[VERIFIED …]` / `[CORRECTED …]`):**
| File | Correction |
|---|---|
| 04 | **OpenPose** "$25k/yr + excludes field of Sports" is TRUE but was **miscited** — those terms are on OpenPose's commercial-licensing page, not the linked LICENSE file (which is noncommercial-only). *Conclusion (avoid OpenPose) unchanged.* |
| 04 | BlazePose accuracy %s are from Google's model card, not the linked blog. Monocular error figures (mm/°) are from a paywalled study — marked as reported-not-confirmed. |
| 06 | Driveline used a **custom pose model**, not "YOLOv8 pose" (the article explicitly rejected YOLOv8). |
| 06 | Removed `kushaldev75/Bat-Ball-Tracking-System` — it's a **cricket** project, miscategorized under baseball. |
| 06 | Removed **PMC7739760** as support for single-camera 3D lift — it's a *multi-camera* study (contradicted the claim). PMC10951609 does support it. |
| 06 | PMC12378739 supports the rotation limitation but doesn't name MeTRAbs/OpenCap; SwingVision funding/scale figures aren't in the linked Apple page — both annotated. |
| 05 | MLB Statcast: cited source specifies **5 high-FPS cameras**; removed the weak everchem.com repost and the unsupported "full limb orientation" quote. Theia3D r²/RMSE figures aren't in the ISBS abstract — annotated. |
| 01 | The "semi-automated…" line was **not a direct OnForm quote** (quotation marks removed); and OnForm does **not** downplay automation (its marketing promotes auto-detect/3D) — reframed. |
| 02 / 03 | OnForm's 3D validation systems are described generically as "EMS" and "optical markered" in the source — the brand names **Polhemus/Qualisys** were unconfirmed and removed. |
| 07 | Cleanest file — no corrections. The darts paper (2604.01130) is off-domain (cited for method); NCC/z-score technique traces to the PT/darts sources, not SportsGPT. |

**Net:** the two founding conclusions — (Q1) OnForm has no killer proprietary moat, and (Q2) the
open-source pipeline is feasible with permissively-licensed components — are **unaffected**. The
corrections tightened over-attributed figures and one wrong technical detail; none reversed a finding.
