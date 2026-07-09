# Sandlot

**Pro-grade swing analysis that runs on a phone, costs nothing, and belongs to everybody's kid.**

> *Sandlot is a working name.* The sandlot is where kids played baseball for free — no
> fees, no coaches, no gatekeepers. That's the whole idea.

Free forever · Open source · Runs on the device · No subscription.

---

## The problem: talent is evenly distributed, access isn't

Genetics don't check your family's tax bracket. But the tools that turn raw talent into
real development increasingly do.

Youth sports is being quietly monetized into two tiers. Families who can spend put their
kids in front of motion-capture rigs, wearable sensors, and subscription "AI coaches."
Everyone else guesses. Over time that gap compounds — not because one pool of kids is more
gifted, but because one pool got measured and coached and the other didn't. The end state
is a shallower, poorer game for all of us: a competition pool filtered by income instead of
ability. That's not just unfair. It's a worse sport.

The maddening part: **the technology is no longer what's expensive.** Open-source pose
models are free. Modern phones can run them. What's expensive is the monetization wrapped
around them.

| What swing analysis costs today | Price |
| --- | --- |
| Motion-capture lab session (marker suits, camera rig, a technician) | $10,000+ |
| Bat sensor + platform (per athlete) | ~$150 |
| Subscription "AI swing" apps (recurring, per season) | ~$80/mo |
| **Sandlot** (open source · on-device · no account) | **$0** |

---

## What we won't compromise

These aren't marketing values. Each one is a technical decision that makes the promise
structurally true — not a policy that can be quietly reversed later.

1. **It runs on the device.** Analysis happens on the phone or laptop you already own. No
   server means no per-swing cost, no bill that grows as more kids use it, and nothing to
   shut down when the grant runs out.
2. **Kids' video stays on kids' phones.** Because nothing has to leave the device to be
   analyzed, minors' footage never has to be uploaded, harvested, or sold. On-device isn't
   only cheaper — it's the privacy model children deserve.
3. **Open source, permanently.** The code is public and the license keeps it public. Anyone
   can inspect it, fork it, translate it, or run it after we're gone.
4. **Honest about what it can measure.** Body mechanics from a single camera are
   trustworthy. True bat speed needs more. We show our accuracy, cite our ground truth, and
   never dress an estimate up as a lab reading.
5. **The core is never behind a paywall.** Record a swing, get real feedback — that path
   stays free for everyone, always. If anything is ever paid, it funds the free tier and
   sits strictly on top of it.

---

## The access model: one camera, a whole town

The last real barrier isn't software — it's a good enough lens. So we don't put one in
every home. We put one in the commons.

**1 camera → ∞ swings.**

A single high-frame-rate camera — the one piece of hardware that meaningfully lifts quality
— is a one-time purchase, not a per-child expense. Picture it living at the public library,
checked out like a book: a family borrows the camera for the weekend, films at the cage or
in the backyard, and Sandlot does the rest on their own phone for free. The library buys
one; the community shares it forever.

That's the whole thesis of accessibility in one image. The expensive part becomes a shared
public good, the free part runs on the phone in your pocket, and the price of entry for a
kid drops to a library card.

---

## What it does

Under the hood it's a pipeline. To the kid, it's a coach who never gets tired, never
charges, and always explains why.

1. **Sees the body.** An open pose model finds the joints in every frame and tracks them
   through the swing — the same skeleton the pros pay a lab to capture.
2. **Finds the moments.** It automatically splits the swing into its phases — load, stride,
   launch, contact, finish — so feedback can point at exactly *when* something goes wrong.
3. **Measures the sequence.** It checks the order power flows through the body — hips, then
   torso, then hands — and how the swing stacks up against a library of strong, healthy
   swings.
4. **Finds the cause, not the symptom.** Weak contact is a symptom. Rotating the torso
   before the hips is a cause. Sandlot traces the fault upstream and picks the single
   highest-leverage fix.
5. **Coaches in plain words.** The finding becomes one clear cue and one drill — grounded
   strictly in what was measured, phrased for a twelve-year-old, not a biomechanist.

---

## The swing, in phases

Power is built in order, from the ground up. Sandlot reads each phase against what a strong,
healthy swing looks like — and localizes the fault to the exact moment it happens.

| # | Phase | What Sandlot watches |
| --- | --- | --- |
| 01 | Stance | Balance, posture, and a stable spine angle — the platform everything is built on. |
| 02 | Load | Weight gathers back, hands set, head stays quiet. Energy is stored, not leaked. |
| 03 | Stride | A short, controlled step toward the pitch. Timing to foot-plant and a still head. |
| 04 | Launch | Hip–shoulder separation peaks and the hips fire first — the signature of real power. |
| 05 | Contact | The chain arrives in order against a braced front leg. Where the sequence pays off. |
| 06 | Finish | Full rotation, balanced. A clean follow-through is evidence the swing was efficient. |

---

## The horizon: start with the swing, build a commons

Baseball is the first swing, not the last — the same skeleton and sequence logic extends to
softball, then to any sport where a body moves through a pattern. Beyond the app, the real
prize is an open, community-owned library of drills and reference movement that improves
every time someone contributes to it.

And here's the quiet advantage of doing this in the open, for free: the best public
biomechanics datasets are licensed for exactly this — non-commercial, community,
youth-development use. A predatory for-profit legally can't build on them the way we can.
The mission isn't a constraint on the project. It's the moat.

---

## What Sandlot will never be

- **A subscription.** The core loop — record a swing, get real feedback — is free for every
  kid, every season, forever.
- **A harvester of children's data.** Analysis runs on-device precisely so a minor's video
  never has to be uploaded or sold to anyone.
- **A gatekeeper.** No academy affiliation, no coach's login, no gear requirement stands
  between a kid and their own swing.
- **Snake oil.** We measure what a camera can honestly measure, show the accuracy, and say
  so when it's an estimate.

---

## The invitation

If you coach, if you code, if you run a rec league or a library branch — there's a piece of
this with your name on it. The technology is finally cheap enough. The only thing missing is
the will to give it away.

Every kid with a swing and a phone deserves to know why it works. Let's make that the
default, not the privilege.

**Status:** vision, pre-code. First milestone is small and honest — a single video in, a
skeleton and a swing-sequence read out. Feedback comes next. Nothing here requires a server,
a sensor, or a subscription to be true.

---

*A rendered version of this vision lives at [`docs/vision.html`](docs/vision.html).*
