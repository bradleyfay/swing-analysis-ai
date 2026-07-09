# 04 — Open-Source Pose Estimation for Fast Swing Analysis

**Research question:** Which open-source pose models suit analyzing a fast baseball swing from
high-fidelity video, and — critically — which are usable **commercially/openly** given their licenses?

**Compiled:** 2026-07-09 · agent-gathered web research. **Re-verify exact license terms before
shipping anything.**

---

## Bottom line

Viable stack: **RTMPose/RTMO (Apache-2.0) or MediaPipe BlazePose (Apache-2.0) for 2D/on-device
→ MotionBERT (Apache-2.0) for 2D→3D lifting, OR Pose2Sim (BSD-3) + OpenSim for true multi-camera
3D → MediaPipe Hands (Apache-2.0) for grip → a custom-trained bat-keypoint model.** Avoid
**OpenPose** (commercial license explicitly **excludes the field of Sports**) and avoid shipping
**Ultralytics YOLO** weights without an Enterprise license (AGPL-3.0). The dominant challenge is
**motion blur** on the bat and fast limbs, mitigated primarily by **high-FPS, high-shutter-speed
capture** (120–500+ fps), not by the model alone.

## 1. Licensing summary (the decisive axis)

| Model / Tool | License | Commercial? |
|---|---|---|
| MediaPipe / BlazePose / MediaPipe Hands | Apache-2.0 | ✅ Yes |
| MoveNet | Apache-2.0 | ✅ Yes |
| RTMPose / RTMO / RTMDet / MMPose | Apache-2.0 | ✅ Yes |
| ViTPose / ViTPose++ (code) | Apache-2.0 | ✅ Yes (see weight caveat) |
| MotionBERT (3D lift) | Apache-2.0 | ✅ Yes |
| MHFormer (3D lift) | MIT | ✅ Yes |
| Pose2Sim (3D pipeline) | BSD-3-Clause | ✅ Yes |
| VideoPose3D (Meta, 3D lift) | **CC BY-NC 4.0** | ❌ **Non-commercial only** |
| OpenPose (CMU) | Custom research license | ❌ **$25k/yr AND cannot be used in the field of Sports** |
| YOLOv8-pose / YOLO11-pose (Ultralytics) | **AGPL-3.0** or paid Enterprise | ⚠️ Not without Enterprise license |

**Critical caveats:**
- **OpenPose** grants only non-commercial academic use; commercial license is a **non-refundable
  $25,000/year royalty** and terms state it **cannot be used in the field of Sports** —
  disqualifying. https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/LICENSE
- **Ultralytics YOLO** is AGPL-3.0: any product incorporating it (even as a network service) must
  open-source the entire application, or buy the Enterprise License. https://github.com/ultralytics/ultralytics
- **VideoPose3D is CC BY-NC 4.0** — non-commercial. Prefer MotionBERT or MHFormer.
- **ViTPose weight caveat:** code is Apache-2.0, but verify the specific checkpoint's dataset
  license (COCO-trained weights are generally fine). The HuggingFace `transformers` ViTPose
  integration is the cleanest route.

## 2. Model-by-model assessment

### MediaPipe / BlazePose GHUM (Google) — Apache-2.0
- 33 landmarks with per-landmark 3D (metric-scale via GHUM) + `world_landmarks` in meters.
- Accuracy: heavy ~98.3% / full ~96.6% / lite ~93.8% (Google's own metric). Lower joint-precision
  than ViTPose/RTMPose on COCO AP.
- Real-time on phones/web; detector→tracker pipeline can lag/lose pose during ballistic swing
  phases and heavy motion blur; single-person, front-facing bias.
- Verdict: **best permissive on-device option** with built-in 3D; validate on fast swing footage.
- https://ai.google.dev/edge/mediapipe/solutions/vision/pose_landmarker · https://blog.tensorflow.org/2021/08/3d-pose-detection-with-mediapipe-blazepose-ghum-tfjs.html

### MoveNet Lightning / Thunder (TF) — Apache-2.0
- 2D only, 17 COCO keypoints, >30 FPS on modern phones. TFLite FP16/INT8.
- Verdict: great lightweight permissive edge option for a 2D overlay; insufficient alone for full
  biomechanics. https://blog.tensorflow.org/2021/05/next-generation-pose-detection-with-movenet-and-tensorflowjs.html

### OpenPose (CMU) — legally encumbered, AVOID
- Non-commercial research only; commercial = $25k/yr and **prohibited in Sports**. Superseded on
  accuracy/speed by RTMPose/RTMO. https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/LICENSE

### ViTPose / ViTPose++ — Apache-2.0 (SOTA accuracy)
- SOTA top-down keypoints (COCO, MPII, COCO-WholeBody). Large backbones are heavy / not real-time.
- Verdict: **best-accuracy permissive option for offline** analysis; pair with a detector (RTMDet);
  use the HuggingFace `transformers` route for clean licensing.
- https://github.com/ViTAE-Transformer/ViTPose · https://huggingface.co/docs/transformers/en/model_doc/vitpose

### RTMPose / RTMO / RTMDet (OpenMMLab MMPose) — Apache-2.0 ⭐ recommended core
- RTMPose-m ~75.8% AP on COCO; RTMO (one-stage) 74.8% AP, ~141 FPS on a V100.
- RTMPose-m ~90+ FPS on an i7 CPU, 430+ FPS on a GTX 1660 Ti. Whole-body/feet/hand/face models.
- **Proven in this space:** **Swing Catalyst** built a golf/baseball swing markerless system on
  **RTMPose + RTMDet**; **Pose2Sim** now defaults to RTMPose.
- Verdict: **best all-around permissive core** — accuracy near ViTPose, real-time speed,
  whole-body coverage, proven in baseball/golf.
- https://arxiv.org/abs/2303.07399 · https://arxiv.org/html/2312.07526v2 · https://github.com/open-mmlab/mmpose · https://support.swingcatalyst.com/hc/en-us/articles/18829350333468

### YOLOv8-pose / YOLO11-pose (Ultralytics) — AGPL-3.0
- Very fast single-shot detection+pose; easy to train custom keypoints (e.g., a bat). **License is
  the blocker** for closed-source commercial use. Use Enterprise license or train an equivalent on
  a permissive framework. https://docs.ultralytics.com/models/yolo11

### 3D lifting (monocular 2D → 3D)

| Model | License | H3.6M MPJPE | Notes |
|---|---|---|---|
| **MotionBERT** | Apache-2.0 | ~39.2 mm | Best commercially-usable temporal lifter |
| **MHFormer** | MIT | ~43.0 mm | Multi-hypothesis transformer; permissive fallback |
| VideoPose3D | **CC BY-NC 4.0** | ~46.8 mm | **Non-commercial — do not ship** |

https://github.com/Walter0807/MotionBERT · https://github.com/Vegetebird/MHFormer

### Hand / grip tracking
- **MediaPipe Hands (Apache-2.0):** 21 3D landmarks/hand, real-time. Fast bat-hand motion + blur +
  two overlapping hands is challenging — capture at high FPS.
- **RTMPose whole-body/hand (Apache-2.0):** integrate hand keypoints into the same pipeline.

### Bat / equipment tracking
No off-the-shelf open-source bat model exists — this is **custom object/keypoint detection**:
- **Driveline** trained a **custom 2-keypoint model (bat cap + knob)** on ~thousands of annotated
  frames, ~2.4–2.6 px mean miss. Insights: (1) **keypoint confidence stayed reliable under motion
  blur even when box-detection confidence dropped**; (2) recovered **3D scale from monocular video
  using known physical bat length as a reference**; captured at **120 FPS**.
- **Theia Markerless** (commercial reference): 124+ body points + bat points via multiple
  high-speed cameras.
- **Recommendation:** train your own bat cap/knob keypoint model on a permissive framework
  (RTMDet+RTMPose); use bat length as the monocular scale reference.
- https://www.drivelinebaseball.com/2025/02/bat-tracking-computer-vision-and-the-next-frontier/ · https://www.theiamarkerless.com/blog/markerless-bat-tracking-q-a-for-pro-baseball-performance-staff

## 3. Speed & motion blur — the core physical challenge
Markerless pose degrades on fast swings mainly from **motion blur on the bat and distal limbs**,
not model quality.
- **30 fps consumer footage produces partial-body motion blur during fast actions**, capping
  accuracy. https://dl.acm.org/doi/10.1145/3606038.3616163
- **Primary mitigation = capture, not model:** high frame rate + fast shutter + good lighting.
  Baseball rigs run **120 fps up to 500+ fps** (e.g., Sony IMX287 global-shutter up to 539 fps);
  global shutter avoids rolling-shutter skew.
- **Model-side:** top-down pipeline (detect→crop→high-res pose); per-keypoint confidence +
  interpolate individually blurred joints (Driveline); temporal 3D lifters (MotionBERT) smooth
  across frames; optional deblurring; research frontier: event cameras.
- https://www.theimagingsource.com/en-us/resource/news/2023/08/03/

## 4. Monocular 3D vs multi-camera

**A. Monocular → 2D pose → 3D lifting (MotionBERT/MHFormer).** Cheapest, consumer-friendly, but
inherent depth ambiguity: markerless-vs-marker errors **~72–122 mm in-plane, ~146–249 mm in
depth**, joint-angle errors **~9°** for athletic movements (largest at elbow). Good for
coaching/relative comparisons, not research-grade absolute kinematics. Known-bat-length scale
trick helps recover metric scale.

**B. Multi-camera triangulation → OpenSim (Pose2Sim / OpenCap).** **Pose2Sim (BSD-3)**:
multi-camera 2D (RTMPose default) → triangulation → OpenSim; angle errors **~3–4.3°** — markedly
better. Works with phones/GoPros. **OpenCap**: 2+ smartphones, also outputs kinetics.
- https://github.com/perfanalytics/pose2sim · https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9002957/ · https://link.springer.com/article/10.1007/s12283-024-00460-w

**Guidance:** for a shippable consumer/coach product, single-camera RTMPose/MediaPipe + MotionBERT
is adequate. For accurate 3D joint-angle/biomechanics claims, use **2+ synchronized high-FPS
cameras with Pose2Sim + OpenSim**.

## 5. Recommended commercial/open stack (all permissive)

| Layer | Recommended | License |
|---|---|---|
| Person detection | RTMDet | Apache-2.0 |
| 2D body pose (real-time/edge) | RTMPose (or MediaPipe BlazePose on-device) | Apache-2.0 |
| 2D body pose (max accuracy, offline) | ViTPose/ViTPose++ (COCO weights) | Apache-2.0 |
| Grip / hands | MediaPipe Hands or RTMPose whole-body/hand | Apache-2.0 |
| Bat tracking | Custom cap/knob keypoint model on RTMDet/RTMPose | Apache-2.0 (your weights) |
| 2D→3D lifting (monocular) | MotionBERT (fallback MHFormer) | Apache-2.0 / MIT |
| Full 3D biomechanics (multi-cam) | Pose2Sim + OpenSim | BSD-3 |
| Capture | 120–500+ fps global-shutter, fast shutter, strong lighting | — |

**Avoid in production:** OpenPose (non-commercial + Sports-excluded), VideoPose3D (CC BY-NC),
Ultralytics YOLO weights without Enterprise (AGPL-3.0).

## Key source URLs
- RTMPose https://arxiv.org/abs/2303.07399 · RTMO https://arxiv.org/html/2312.07526v2 · MMPose https://github.com/open-mmlab/mmpose
- ViTPose https://github.com/ViTAE-Transformer/ViTPose
- MediaPipe Pose https://ai.google.dev/edge/mediapipe/solutions/vision/pose_landmarker · Hands https://developers.google.com/edge/mediapipe/solutions/vision/hand_landmarker
- MoveNet https://blog.tensorflow.org/2021/05/next-generation-pose-detection-with-movenet-and-tensorflowjs.html
- OpenPose license https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/LICENSE
- Ultralytics https://github.com/ultralytics/ultralytics
- MotionBERT https://github.com/Walter0807/MotionBERT · MHFormer https://github.com/Vegetebird/MHFormer · VideoPose3D https://github.com/facebookresearch/VideoPose3D
- Pose2Sim https://github.com/perfanalytics/pose2sim
- Swing Catalyst https://support.swingcatalyst.com/hc/en-us/articles/18829350333468
- Driveline bat tracking https://www.drivelinebaseball.com/2025/02/bat-tracking-computer-vision-and-the-next-frontier/
- Theia markerless https://www.theiamarkerless.com/blog/markerless-bat-tracking-q-a-for-pro-baseball-performance-staff
- Motion-blur paper https://dl.acm.org/doi/10.1145/3606038.3616163
- Sports HPE validation https://link.springer.com/article/10.1007/s12283-024-00460-w
