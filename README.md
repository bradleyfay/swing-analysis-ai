<h1 align="center">Sandlot</h1>

<p align="center">
  <strong>Pro-grade baseball swing analysis that runs on a phone, costs nothing, and belongs to everybody's kid.</strong>
</p>

<p align="center">
  Free forever · Open source · Runs on the device · No subscription
</p>

---

> **Sandlot is a working name.** The sandlot is where kids played baseball for free — no fees,
> no coaches, no gatekeepers. That's the whole idea.

The biomechanics that shape million-dollar prospects shouldn't be locked behind
million-dollar incomes. Sandlot turns a single video of a baseball swing into the kind of
feedback that used to require a lab — and gives it away.

**Read the full vision → [`VISION.md`](VISION.md)** (or open the rendered
[`docs/vision.html`](docs/vision.html) in a browser).

## Why this exists

Youth sports is being quietly monetized into two tiers: families who can spend put their
kids in front of motion-capture rigs, wearable sensors, and subscription "AI coaches" —
everyone else guesses. The maddening part is that the *technology* is no longer what's
expensive. Open-source pose models are free and modern phones can run them. What's expensive
is the monetization wrapped around them.

Sandlot removes the wrapper.

| What swing analysis costs today | Price |
| --- | --- |
| Motion-capture lab session | $10,000+ |
| Bat sensor + platform (per athlete) | ~$150 |
| Subscription "AI swing" apps | ~$80/mo |
| **Sandlot** | **$0** |

## How it works

A pipeline that watches a swing the way a good coach does, then says the one thing that matters:

1. **Sees the body** — an open pose model tracks the joints through every frame of the swing.
2. **Finds the moments** — automatically segments the swing into phases (load → stride → launch → contact → finish).
3. **Measures the sequence** — checks the order power flows through the body (hips → torso → hands) against a library of strong, healthy swings.
4. **Finds the cause, not the symptom** — traces a fault upstream and picks the single highest-leverage fix.
5. **Coaches in plain words** — one clear cue and one drill, grounded strictly in what was measured.

## Design principles

Each is a technical decision that makes the promise structurally true — not a policy that can be reversed later.

- **On-device.** No server means no per-swing cost and nothing to shut down when the grant runs out.
- **Private by construction.** Minors' video never has to leave the phone to be analyzed.
- **Open source, permanently.** Public code, permissive license, forkable by anyone.
- **Honest about accuracy.** Body mechanics from one camera are trustworthy; true bat speed needs more, and we'll say so.
- **Core never paywalled.** Record a swing, get real feedback — free for everyone, always.

## Status

**Pre-code / vision stage.** This repository currently holds the founding vision. The first
milestone is deliberately small and honest: a single video in, a skeleton and a
swing-sequence read out. Feedback generation comes next.

### Direction of the build (from research, subject to change)

- **Pose estimation:** permissively-licensed, on-device — e.g. [RTMPose](https://github.com/open-mmlab/mmpose)/MediaPipe (Apache-2.0). Avoiding license-encumbered options (OpenPose's terms exclude sports use; Ultralytics YOLO is AGPL).
- **3D lift (later):** MotionBERT (Apache-2.0) for monocular 2D→3D.
- **Reference data / validation:** [Driveline OpenBiomechanics](https://github.com/drivelineresearch/openbiomechanics) (CC BY-NC-SA — available to a non-commercial project like this one) and [BaseballCV](https://github.com/BaseballCV/BaseballCV).
- **Feedback layer:** deterministic rule engine first (free, on-device), with an optional small/local LLM for natural-language polish — kept off the critical path so nothing essential ever requires a backend.

## Contributing

If you coach, code, run a rec league, or work at a library branch — there's a piece of this
with your name on it. Contribution guidelines will land alongside the first code. In the
meantime, issues and ideas are welcome.

## License

[BSD 3-Clause](LICENSE) © 2026 Bradley Fay
