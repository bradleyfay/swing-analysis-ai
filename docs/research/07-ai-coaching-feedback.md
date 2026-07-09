# 07 — Turning Pose/Biomechanics Data into Coaching Feedback (the "AI Coach" Layer)

**Research question:** How do you turn quantitative pose/biomechanical data from a swing into useful,
actionable coaching feedback? What architecture do credible systems use?

**Compiled:** 2026-07-09 · agent-gathered web research.

---

## Executive summary

The state of the art (2025-2026) has converged on a **hybrid "compute-then-narrate" architecture:**
precise deterministic modules do the biomechanical analysis (phase segmentation, DTW alignment,
statistical deviation scoring), and an LLM is used **only** as the final translation/narration
layer, tightly grounded in the structured facts those modules produce. Every credible system
deliberately keeps the LLM away from raw keypoints to avoid hallucination. The most complete
published blueprint for this exact problem is **SportsGPT** (Dec 2025), whose three-stage
MotionDTW → KISMAM → SportsRAG pipeline maps almost one-to-one onto what's needed. **BioCoach** and
**Talking Tennis** independently arrive at the same "intermediate symbolic biomechanics layer feeds
the LLM" pattern.

## Recommended pipeline (raw pose → coaching advice)

```
[1] Pose keypoints (per frame)      MediaPipe/HSMR → 24–33 landmarks
        ▼
[2] Feature engineering & normalization
        joint ANGLES + angular velocity/accel (NOT raw coords),
        normalized by trunk/bounding-box to remove body-size & camera scale
        ▼
[3] Phase segmentation / event detection
        load, stride, launch, contact, follow-through
        (LightGBM classifier OR rule-based kinematic-event peaks)
        ▼
[4] Reference alignment (DTW)
        align hitter's phased sequence to elite reference keyframes
        in the multi-modal (angle/velocity/accel) feature space
        ▼
[5] Deviation scoring vs. reference distribution
        per-metric z-scores / NCC → structured list of anomalies + magnitudes
        ▼
[6] Fault diagnosis / root-cause mapping
        deviation→fault matrix; rank; pick top-1 (kinematic-sequence logic = cause vs symptom)
        ▼
[7] LLM narration + drill retrieval (RAG-grounded)
        structured facts + retrieved drill knowledge → personalized, prioritized coaching text
        ▼
[8] Hitter-readable feedback (one fix, phase-specific, with a drill)
```

**Critical design principle across every source: separate quantitative processing from language
generation.** Steps 2–6 are deterministic and auditable; step 7 is the only generative step, fed
facts, not pixels.

## Topic 1 — Approaches to generating feedback from metrics
Three paradigms, and winning systems **layer all three:**
- **(a) Rule-based / expert-threshold** — metrics vs. biomechanical thresholds → fault labels.
  Transparent, injury-relevant, but brittle/robotic.
- **(b) Comparison-to-ideal-template** — map to an elite reference, report deviations (backbone of
  steps 4–5).
- **(c) LLM-based generation** — convert structured deviations into fluent, personalized, drill-
  oriented prose. **Universal finding: never feed the LLM raw pose.** Inject "structured
  biomechanical constraints directly into the prompt" (SportsGPT) / an "intermediate symbolic layer
  of biomechanical facts the model conditions upon" (BioCoach).
- Key systems: SportsGPT, BioCoach, Talking Tennis, AgentCoach (CHI 2026), ViSTAR.
- https://arxiv.org/html/2512.14121v1 · https://vilab-group.com/project/biocoach/ · https://arxiv.org/pdf/2510.03921 · https://dl.acm.org/doi/10.1145/3772318.3791652

## Topic 2 — Comparing against "good" reference swings
**Dynamic Time Warping (DTW)** is the consensus alignment method — aligns sequences that differ in
tempo so equivalent events line up (Euclidean distance can't). Three refinements:
1. **Align in a feature space of joint angles + angular velocity + acceleration, not raw coords.**
   SportsGPT's **MotionDTW** is two-stage: coarse FastDTW subsequence-match to locate the swing,
   then fine keyframe alignment; **weights joints by biomechanical relevance.**
2. **Statistical deviation from a reference *distribution*, not a single template** — per-metric
   **z-score** against good swings; ideally personalize to the athlete's own stable high-quality range.
3. **NCC (Normalized Cross-Correlation)** as a complementary similarity score (>0.85 = good match),
   keypoints normalized in a fixed bounding box.
- https://arxiv.org/html/2512.14121v2 · https://pmc.ncbi.nlm.nih.gov/articles/PMC12749503/ · https://arxiv.org/pdf/2604.01130

## Topic 3 — LLMs translating structured biomechanics → drill-based coaching
**SportsGPT is the reference implementation.** Three stages:
- **MotionDTW** → keyframe extraction.
- **KISMAM** (Knowledge-based Interpretable Sports Motion Assessment Model) → compares keyframes to
  elite references, computes per-metric deviations, **aggregates through a mapping matrix to
  identify root causes**, outputs a **ranked list of problems with biomechanical rationale.**
- **SportsRAG** → a **RAG layer** (Qwen3-8B over a 6B-token KB of textbooks + 50k expert QA pairs)
  that converts semantic labels ("knee valgus") into metric queries, retrieves domain knowledge,
  concatenates it with the KISMAM diagnosis as an **augmented prompt**, and generates specific
  prescriptions rather than generic advice.

The **RAG-grounded drill library is the key to actionable, non-hallucinated drills.** For baseball,
build a KB of hitting drills (tee work, high-tee, walk-through, connection ball, etc.) keyed to
fault labels; retrieve the drill matched to the diagnosed root cause; let the LLM personalize.

**BioCoach** confirms the pattern (prepends a structured motion-quality analysis to LLaMA-2-7B,
fuses body-measurement context via cross-attention; built a biomechanics-aware LLM-judge metric).
**Talking Tennis** grounds the LLM in per-stroke numeric features chosen from biomechanics literature.

**For a build with Claude:** a structured JSON payload (phase-by-phase metrics + deviations + ranked
faults + retrieved drill candidates) → Claude, with a system prompt encoding coaching principles and
"cite only the provided facts."

## Topic 4 — What makes feedback actionable
- **Diagnose the disease, not the symptom.** Weak grounders/getting jammed are *symptoms*; fixing
  the visible symptom is the common coaching mistake. Fault-mapping must trace symptoms to a root fault.
- **The kinematic sequence is the root-cause engine.** Power flows pelvis → torso → lead arm →
  hand/bat. **If the torso rotates before the hips, energy is lost** — a measurable *cause* upstream
  of many symptoms. Measure peak angular velocity + timing/order of each segment, and hip-shoulder
  separation.
- **Fix one thing at a time** — pick the highest-leverage upstream fault (maps to a ranked top-N,
  surface top-1).
- **Prescribe a drill/constraint, not just a correction** — favor constraint-based drills that force
  the adjustment.
- https://www.coachqbaseball.ca/post/every-hitter-leaves-clues · https://www.drivelinebaseball.com/2016/10/coaching-hitting-mechanics/ · https://rocklandpeakperformance.com/understanding-the-kinematic-sequence/

## Topic 5 — AI form-feedback in fitness/PT
Common architecture: **MediaPipe/OpenCV pose → joint-angle features → compare to ideal → rep
counting via peak detection → immediate feedback.** Most transferable is the **PT real-time
action-scoring system** (Nature Sci Reports 2025): 33 landmarks → joint angles → **DTW + NCC** vs. a
clinician reference; feedback is **specific and comparative** ("right shoulder extending 20° more
than demo"); composite Action Score blends similarity, rep accuracy, ROM/velocity closeness. This
"specific joint + magnitude + vs. reference" structure is exactly the intermediate representation to
hand the LLM.
- https://www.nature.com/articles/s41598-025-29062-7 · https://pmc.ncbi.nlm.nih.gov/articles/PMC12749503/ · https://github.com/yakupzengin/fitness-trainer-pose-estimation

## Topic 6 — Automatic swing phase segmentation
Two complementary methods:
- **(a) ML classifier (LightGBM).** Baseball *pitching* phase study (App. Sci. 2025): 12 MediaPipe
  landmarks → 33 kinematic features normalized by trunk → 5 phases at **99.7% accuracy**, 6-8s/video.
  https://www.mdpi.com/2076-3417/15/22/12155
- **(b) Rule-based kinematic-event detection.** Phase boundaries = characteristic events as extrema.
  For hitting: **stance/load** (weight to back foot), **stride** (front foot off → plant), **launch**
  (rotation start / peak force), **acceleration**, **contact** (bat-hand velocity peak), **follow-
  through**. Detect from front-foot lift/plant (ankle vertical), COM weight-shift, first pelvis-
  rotation frame, hand/bat peak velocity. Phase segmentation is a **prerequisite for phase-specific
  feedback.**

## Concrete recommendations
1. **Adopt the SportsGPT three-stage skeleton** (align → assess → narrate).
2. **Joint angles + angular velocity/accel, normalized by trunk length**; never feed the LLM raw keypoints.
3. **Segment phases first** (rule-based for interpretability; add LightGBM later), then per-phase DTW
   against a library of "good" reference swings.
4. **Score deviations as z-scores** against a distribution of good swings, personalized over time.
5. **Encode a deviation→fault mapping; use kinematic-sequence logic for root cause;** rank; surface
   top-1 per session.
6. **Build a RAG drill library keyed to fault labels** so the LLM retrieves/personalizes a real drill.
7. **Prompt the LLM with structured JSON** + a coaching-principles system prompt (one fix,
   phase-specific, cause-not-symptom, cite only provided facts).
8. **Evaluate feedback with a biomechanics-aware LLM-judge** (BioCoach) plus human coach review.

**Most valuable next reads:** SportsGPT — https://arxiv.org/html/2512.14121v2 ; BioCoach —
https://arxiv.org/html/2603.26938v1
