# 05 — Baseball Swing Biomechanics & Open Reference Datasets

**Research question:** Which swing metrics matter for hitting analysis, which are measurable from
body-pose keypoints (vs. needing sensors), and what open datasets can serve as reference/ground truth?

**Compiled:** 2026-07-09 · agent-gathered web research.

---

## 1. The kinematic sequence & hip-shoulder separation ("X-Factor")

**Core concept.** Power flows proximal-to-distal: **ground → pelvis → torso/trunk → lead arm →
hands → bat**. Each segment accelerates, peaks in angular velocity, then *decelerates* to transfer
energy to the next. An efficient swing shows peak angular velocities **in order** (pelvis → torso →
arm → hand/bat), each reaching a *higher* peak than the prior — a "speed gain" up the chain.
([RPP](https://rocklandpeakperformance.com/understanding-the-kinematic-sequence/), [Athletes Untapped](https://athletesuntapped.com/blog/the-science-of-the-swing-decoding-biomechanical-analysis-in-baseball/))

**X-Factor / hip-shoulder separation.** As the pelvis rotates toward the pitcher, the torso stays
back, creating angular separation that loads the trunk (stretch-shortening cycle), storing energy
that amplifies bat speed. ([Driveline](https://www.drivelinebaseball.com/2019/10/hitting-concepts-visualizing-the-why-behind-hip-hinge-hip-torso-separation-and-maintaining-spine-angle/))

**Quantitative benchmarks** ([RPP](https://rocklandpeakperformance.com/pitching-biomechanics-hip-shoulder-separation-closing-the-gap/), [Inside the Lines](https://www.insidethelinessports.com/post/kinematic-sequence-explained-in-under-3-minutes-why-your-swing-isn-39-t-working)):
- Elite peak separation typically **~30°–55°** (scales with torso length).
- Peak separation occurs when the torso angular-velocity curve *crosses* the pelvis curve.
- At contact, elite hitters have **closed the gap to ~0–5°** — separation is a mid-swing loading
  state, not an end state.
- **Common flaws:** torso firing before/with the pelvis ("upper-half hitter"); insufficient
  separation; separation closing too early. ([RPP flaws](https://rocklandpeakperformance.com/3-most-common-kinematic-sequence-flaws-in-baseball-swings/))

## 2. Coaching-relevant swing metrics

**Blast Motion** (bat-knob IMU) — the de-facto coaching vocabulary
([Blast](https://blastmotion.com/blog/baseball-swing-metrics-explained/), [RPP review](https://rocklandpeakperformance.com/a-review-of-blast-motion-baseball-and-its-swing-quality-metrics/)):

| Metric | Definition | Benchmark |
|---|---|---|
| Bat Speed | Barrel velocity at impact | Pro 66-78 mph; HS 60-70; MS 46-62 |
| Hand Speed | Knob velocity through swing | High rel. to bat speed = efficient whip |
| Time to Contact | Downswing start → impact | Shorter = better coverage |
| Attack Angle | Vertical bat-path angle at contact | +5° to +20° |
| On-Plane Efficiency | % of swing barrel is on plane | 70%+; MLB avg 68.6% |
| Early Connection | Lead-arm-to-torso angle at rotation start | ~90° (80-105°) |
| Connection at Impact | Body-to-barrel angle at contact | swing "unity" |
| Rotational Acceleration | How fast bat accelerates into plane | Higher = explosive |
| Vertical Bat Angle | Bat tilt vs. horizontal at contact | pairs with attack angle |

**MLB Statcast Bat Tracking** (Hawk-Eye markerless, 12 cameras, 5 @ 300 fps) — camera-derived, no
bat sensor: **bat speed, swing length, fast-swing rate, squared-up rate, blast rate, attack angle,
swing path tilt.** Hawk-Eye "tracks not only player center of mass but full limb orientation
(including the bat)" — a **production markerless system already extracting swing kinematics from
video**. ([MLB.com](https://www.mlb.com/news/what-you-need-to-know-about-statcast-bat-tracking), [Savant](https://baseballsavant.mlb.com/leaderboard/bat-tracking/swing-path-attack-angle), [Everchem](https://everchem.com/mlb-tech-update/))

**Body-mechanics checkpoints coaches care about:** load/stride mechanics, weight transfer/COM
shift, **head movement/stability**, **front-leg block/extension**, **posture/spine angle**, hand
path, hip-hinge. ([Hitting Vault](https://thehittingvault.com/how-to-swing-a-baseball-bat/), [RPP 15-step](https://rocklandpeakperformance.com/15-steps-to-analyzing-baseball-swing-mechanics/), [Catapult](https://www.catapult.com/blog/baseball-swing-analysis))

## 3. Measurability from pose keypoints — the core mapping

Markerless context: multi-camera markerless (Theia3D) resolves joint centers to **~1–3 cm** and
angles to a few degrees; pitching validation reported pelvis/trunk **r²≈0.92, RMSE ~6°**. Accuracy
is **best in the sagittal plane, worst for transverse-plane rotations**, worst at the elbow.
Single-camera 2D pose is far less reliable for absolute 3D angles/rotational velocities.
([NMU/ISBS](https://commons.nmu.edu/isbs/vol42/iss1/238/), [PITCHAI](https://sportrxiv.org/index.php/server/preprint/view/101))

| Metric | Video/pose derivable? | Notes |
|---|---|---|
| **Kinematic sequence** (ordering & timing) | ✅ **Yes (strong fit)** | Relative timing/ordering, tolerant of absolute error. Best match. Needs 240-300 fps ideally. |
| **Hip-shoulder separation / X-factor** | ✅ **Yes** | Transverse rotation is markerless's *weakest* plane — trends/timing usable, absolute degrees noisier. |
| **Peak pelvis/torso angular velocity** | ✅ **Yes** | Differentiate orientation; needs high fps + smoothing. |
| **Head movement/stability** | ✅ **Yes (easy)** | Track head/nose keypoint — very robust. |
| **Front-leg block / lead-knee extension** | ✅ **Yes** | Sagittal plane = high accuracy. |
| **Posture/spine angle** | ✅ **Yes** | Trunk tilt from shoulder-hip vector; reliable. |
| **Weight transfer / COM shift** | ⚠️ **Proxy only** | COM kinematics estimable; true GRF/weight needs **force plates**. |
| **Stride length/load/timing** | ✅ **Yes** | Foot/ankle displacement + event timing. |
| **Hand path / hand speed** | ⚠️ **Partial** | Wrist trajectory gives path + approx speed; noisier than a bat IMU. |
| **Time to contact** | ⚠️ **Partial** | Needs reliable swing-start + contact detection; high fps + bat/ball detection. |
| **Bat speed (true, sweet spot)** | ❌ **Needs bat tracking** | Requires tracking the **bat**. Blast IMU or Hawk-Eye/bat-detection CV. |
| **Attack angle / swing plane / VBA / OPE** | ❌ **Needs bat tracking** | Defined by the *bat's* barrel path, not joints. |
| **Rotational acceleration (Blast)** | ❌ **Sensor-specific** | Body-pose analog is trunk angular accel, not the same metric. |
| **Exit velocity / batted-ball outcome** | ❌ **External** | Ball-tracking (radar/HitTrax/Hawk-Eye). |

**Bottom line:** body-pose keypoints strongly support the *body* side (kinematic sequence,
separation, posture, head stability, leg block, weight-transfer proxies, phase timing). The *bat*
side (true bat speed, attack angle, swing plane) needs an on-bat sensor **or a separate
bat-detection/tracking CV pipeline**. **Treat "body mechanics" and "bat metrics" as two distinct
measurement problems.**

## 4. Established research & the OpenBiomechanics Project

**ASMI (American Sports Medicine Institute).** Gold-standard biomechanics repository — but
**overwhelmingly pitching-focused**, and data is not openly downloadable. Useful for
methodology/norms, not open hitting data. ([Wikipedia](https://en.wikipedia.org/wiki/American_Sports_Medicine_Institute), [asmi.org](https://asmi.org/))

**Driveline OpenBiomechanics Project (OBP) — the key open dataset.**
([openbiomechanics.org](https://www.openbiomechanics.org/), [GitHub](https://github.com/drivelineresearch/openbiomechanics), [announcement](https://www.drivelinebaseball.com/2022/12/openbiomechanics-project/))
- **Scope:** ~**98–99 hitters** and ~**100 pitchers**. Largest high-fidelity open-source elite
  dataset; predominantly collegiate/elite amateur (**not** MLB).
- **Repository modules:** `baseball_hitting/`, `baseball_pitching/`, `high_performance/`
  (force-plate + physical assessment), `computer_vision/` (markerless/2D pose examples), `additional_resources/`.
- **Hitting collection methodology:**
  - Marker-based optical mocap, **marker-derived signals at 1,080 Hz**.
  - **16-marker body set** + **10 markers on the bat**.
  - **Four force plates** in the batter's box at **360 Hz** → true **ground reaction forces /
    weight transfer**.
  - **HitTrax** integration for pitch/batted-ball data (exit velo).
- **Defined swing events (POI):** first move, foot contact (10% BW), foot plant (100% BW), max
  hip-shoulder separation, contact.
- **Hitting POI variables** (from `data_dictionary.csv`): `pelvis_angle_*`, `torso_angle_*`,
  `pelvis_angular_velocity_seq_max_x`, `torso_angular_velocity_seq_max_x`, `upper_arm_speed_mag_seq_max_x`,
  `hand_speed_mag_max_x`, `x_factor_fm_*`, `x_factor_fp_*`, `bat_speed_mph_max_x`,
  `bat_speed_mph_contact_x`, `blast_bat_speed_mph_x`, `attack_angle_contact_x`,
  `bat_torso_angle_connection_x`, `exit_velo_mph_x`, `lead_knee_launchpos_x`, `max_cog_velo_x`, etc.
- **File structure:** `POI.csv` + full-signal `force_plate`/`joint_angles`/`joint_velos`/`landmarks`
  CSVs, `hittrax.csv`, `metadata.csv`. Joined via `session_swing` + `time`. Raw **C3D files &
  full-signal archives on GitHub Releases** (a 2026 restructure shrank the git repo from ~2.2 GB to
  ~25 MB; POI CSVs stay in-repo; `scripts/download_data.sh` fetches archives).
- **License: Code = MIT; Data & docs = CC BY-NC-SA 4.0. Non-commercial only. Explicit exclusion:
  employees/contractors of professional sports organizations and financial firms barred without a
  paid commercial license.** (A non-commercial open project is squarely within the allowed use.)

**Usability:** Very high for the *body* side — synchronized marker joint angles/velocities,
landmarks, force plates, bat kinematics, Blast cross-reference, outcomes, defined events on ~98
elite hitters. Ideal for (a) normative "good swing" ranges and (b) **ground truth to validate a
markerless pipeline**. Limits: not MLB-level talent; transverse-plane rotations carry the most
measurement sensitivity; CC BY-NC-SA + pro-org exclusion.

**OBP Computer Vision module:** ships "markerless/2D pose examples, calibration, pose tutorials"
plus **synchronized video samples** aligned to the mocap — a starter kit for relating video
keypoints to gold-standard marker data.

## 5. Swing phases & good-vs-bad checkpoints

**[P]** = well-measured by body pose · **[B]** = needs bat tracking · **[F]** = needs force plate.
([Hitting Vault](https://thehittingvault.com/how-to-swing-a-baseball-bat/), [RPP 15-step](https://rocklandpeakperformance.com/15-steps-to-analyzing-baseball-swing-mechanics/), [Wikipedia](https://en.wikipedia.org/wiki/Hitting_mechanics))

1. **Stance** — balance, posture/spine angle **[P]**.
2. **Load** — rear-hip load, hand set, no drift; head still **[P]**; weight distribution **[F]**.
3. **Stride** — stride length **[P]**, timing to foot plant **[P]**, minimal head movement **[P]**.
4. **Launch / foot plant → rotation start** — **hip-shoulder separation peaks (~30-55°)** and
   pelvis fires first. Separation magnitude/timing **[P, transverse caveat]**, sequence order
   **[P]**, spine angle **[P]**.
5. **Contact** — separation closed to ~0-5°; front leg braces/extends; barrel on-plane. Front-leg
   block **[P]+[F]**; bat speed **[B]**; attack angle/VBA/OPE **[B]**; connection ~90° **[P]**.
6. **Extension** — arms extend, hands finish high **[P]**.
7. **Follow-through** — full rotation, balanced finish **[P]**.

**Efficient signature:** correct-order, increasing peak velocities; adequate well-timed separation
peaking near foot plant, closing by contact; stable head; firm front-leg block; maintained spine
angle. **Inefficient:** torso leading pelvis; early/late/insufficient separation; head drift;
collapsing front leg (energy leak); lost posture. ([RPP flaws](https://rocklandpeakperformance.com/3-most-common-kinematic-sequence-flaws-in-baseball-swings/))

## Key takeaways
1. **Split the problem in two** — body mechanics (pose) vs. bat metrics (bat detection/IMU).
2. **Pose's strongest outputs:** kinematic-sequence ordering/timing, peak segment angular
   velocities, head stability, front-leg block, spine/posture, stride/weight-shift timing.
3. **Handle separation carefully** — highest-value power metric but transverse plane (weakest axis).
4. **True bat speed, attack angle, swing plane, OPE need bat tracking.**
5. **OpenBiomechanics is the reference dataset** — normative ranges + pipeline ground truth. Watch
   the CC BY-NC-SA license + pro-org exclusion.
6. **ASMI** = authoritative but pitching-centric and closed.

## Primary sources
- OpenBiomechanics: https://www.openbiomechanics.org/ · https://github.com/drivelineresearch/openbiomechanics · https://www.drivelinebaseball.com/2022/12/openbiomechanics-project/
- Metrics: https://blastmotion.com/blog/baseball-swing-metrics-explained/ · https://www.mlb.com/news/what-you-need-to-know-about-statcast-bat-tracking · https://baseballsavant.mlb.com/leaderboard/bat-tracking/swing-path-attack-angle
- Kinematic sequence: https://rocklandpeakperformance.com/understanding-the-kinematic-sequence/ · https://rocklandpeakperformance.com/3-most-common-kinematic-sequence-flaws-in-baseball-swings/
- Markerless validation: https://commons.nmu.edu/isbs/vol42/iss1/238/ · https://sportrxiv.org/index.php/server/preprint/view/101
- Phases: https://thehittingvault.com/how-to-swing-a-baseball-bat/ · https://rocklandpeakperformance.com/15-steps-to-analyzing-baseball-swing-mechanics/
- ASMI: https://asmi.org/
