# Cozy Commute — Project Handoff / Memory

> **Status:** Concept locked. This is the source-of-truth doc for the idea so far
> and the place to pick up if context is lost. It now lives in its permanent home,
> the `cozy-commute` repo. (It was originally drafted in `club-runner` because that
> was the only repo this session could push to before this repo existed.)

_Last updated: 2026-06-30_

---

## One-line pitch

A cozy, real-time game where you book a seat, ride a **train or a plane** that
moves over actual minutes, share the window with a stranger who joined from
another browser, and log the **miles** you travel — unlocking nicer ways to ride.

## The feeling we're going for

Slow-TV calm + Sky-style quiet co-presence + Flighty's miles ritual — built
deliberately as the **anti–_Up in the Air_**: accumulation that pulls people
**together**, not apart.

---

## Where the idea came from (conversation origin)

The concept was assembled live in conversation. Key reference points the user
raised, and what we took from each:

- **Flighty (flight-tracking app)** — the user loves its **miles/Passport stats**
  and the idea that logging miles earns **upgrades** (better cars, more
  refreshments, status). Also relevant: Flighty's beautiful **boarding-pass /
  ticket-as-object** treatment, its **live dot-on-a-map** real-time presence, and
  its **Live Activity** lock-screen countdown.
- **_Up in the Air_ (2009 film)** — George Clooney's Ryan Bingham chasing 10M
  frequent-flyer miles. We took the **ritual, status, and recognition** of
  accumulation — and consciously **rejected its isolation**. The film is a
  cautionary tale: the number is huge, the man is empty. Our design thesis is to
  make the miles point back toward the **stranger in the seat across from you**.
- **Trains AND planes** — the user wants both modes. This is a *strength*: one
  unified miles progression, multiple ways to ride. Future modes possible
  (ferries, night buses, etc.). The shared throughline is **the window seat**.

---

## Prior-art research (done 2026-06-30, web search)

**Conclusion: the specific concept does not appear to exist.** Adjacent things
cluster into three buckets, none of which is this idea:

1. **Train/transit _management/building_ games** — *Station to Station* (cozy,
   voxel, wholesome, but you build networks), *Rail Nation*, *Mini Metro*. You're
   a god, not a passenger.
2. **_Driving/operating_ sims** — *SimRail*, *Train Sim World*, *Densha de Go*,
   *RailroadsOnline*, *Microsoft Flight Simulator*. Real-time, sometimes
   multiplayer, but it's a **job**, not relaxation.
3. **Real-time passenger _watching_ but not a game** — Norway's **Slow TV**
   (the 7-hour *Bergensbanen* train ride was a cultural hit), lo-fi "train window"
   YouTube loops. Passive, solo, no presence.

Closest *feeling* in an actual game: **Sky: Children of the Light** (cozy ambient
co-presence with strangers) and **Animal Crossing** (real-time clock). Neither is
a train/plane, neither is a 5-minute browser drop-in.

**White space = passenger (not operator) + ride (not build) + real-time shared
presence with a stranger.** Validation signal: Slow TV proved millions will watch
a real-time journey do nothing. We add the one thing broadcast couldn't — someone
in the seat across from you.

---

## Design pillars

1. **The ride is the point.** No goals, no fail state. It's an "anti-game"
   (cf. *Mountain*). Cozy ≠ empty, though — see "alive window."
2. **Real time is a feature, not a cost.** A journey takes its real duration. You
   "catch the 3:15." Scarcity/appointment is part of the charm.
3. **Shared presence is the magic.** Quietly sharing a window with a stranger is
   the core emotional hook.
4. **Progress by showing up, not grinding.** Miles accrue in real time only —
   intrinsically un-grindable, un-bottable, and it rewards exactly the behavior
   the game is about: being present.
5. **Patient, additive progression.** Nothing decays, nothing expires, you never
   lose status. No streaks, no FOMO, no daily-login anxiety. The flex is "I've
   logged 4,000 shared miles," not "ride today or lose your streak."

---

## Core loop

Book a seat (one tap — **not** an airline checkout; ticket is a delightful
*object*, not a form) → board a **shared departure** → ride in real time, window
alive, maybe a stranger across from you → arrive → **end-of-journey stats card**
(miles this trip, lifetime miles, new tier, who rode with you — shareable).

---

## Key systems

### The "alive window" (anti-boredom)
Real time with a static view is boring, not cozy. The window must gently change:
parallax scenery, **day → dusk → night** lighting, weather, tunnels (train),
clouds/altitude (plane), the occasional tea trolley. For planes, the
Flighty-style **live dot on a map** is a natural second view.

### Shared departures (anti-ghost-town)
Empty multiplayer feels lonely. Pool players onto a **few shared scheduled
departures** rather than infinite private instances, so carriages/cabins feel
populated. Backfill with **NPC passengers** when human count is low.

### Miles & progression (trains + planes, one currency)
- Miles earned **only by riding in real time** (anti-cheat is intrinsic).
- Upgrades are **cozy and horizontal, NOT power-creep** — nicer ways to *sit*,
  not advantages:
  - **Better cars/cabins:** coach → first class → panoramic dome car → sleeper →
    Orient-Express tier (train); economy → premium → a quiet window suite (plane).
    Each upgrade is really a **better window** (wider view, glass roof, warmer
    light) — which doubles as content the engine already wants.
  - **Refreshments:** tea trolley → dining car → a little pour/eat menu (ambient
    interaction, not a mechanic).
  - **Status tiers** (frequent-flyer style): Coach → Silver → Pullman → Gold, with
    *soft* perks (earlier boarding, reserved window seat, choice of route).
- **Social unlocks are the priority** (the anti–*Up in the Air* hook): a
  **private compartment you can earn and invite a friend into.** Cozy parts gate
  behind *togetherness*. The proudest line on the stats card is **"127 miles
  shared with strangers,"** not the raw total.

### Stats card (borrowed from Flighty)
End-of-journey shareable summary. Cozy games live and die on screenshots.

---

## Risks to design against

- **Real time + nothing to do = boring.** Mitigation: the alive window.
- **Booking becomes friction.** Mitigation: one-tap board; ticket is flavor, not a form.
- **Empty multiplayer = lonely.** Mitigation: shared departures + NPCs.
- **Progression becomes a treadmill / FOMO.** Mitigation: purely additive,
  nothing decays/expires, no streaks.
- **Miles become isolating (the Bingham trap).** Mitigation: gate the best
  rewards behind shared rides; foreground "shared miles."

---

## Tech direction (proposed, not final)

**Next.js + Supabase** (same stack the user knows from club-runner):
- **Supabase Realtime Presence** → live carriage/cabin occupancy, who's aboard.
- **Postgres** → journeys, schedules, lifetime miles, unlocks, status tiers.
- **Server-authoritative clock** → a journey is just `departs_at + duration`.
  Every client computes position as `(now − departs_at) / duration`. No syncing of
  "train position" needed — clients stay in lockstep because they share a clock.
  Miles = real elapsed time aboard, computed/validated server-side.
- If native is wanted later, **iOS Live Activities** (Flighty-style lock-screen
  countdown: "Carriage 3 · arriving in 6 min · 2 others aboard") are reachable
  (club-runner uses Capacitor; same approach could apply here).

> Stack is the working default. Confirm before committing to it.

---

## Naming

Repo / working name: **Cozy Commute**. Alternatives floated during ideation:
*Window Seat*, `wayfare`. (The window-seat motif is still the core image even
under the Cozy Commute name.)

---

## Status & logistics

- This is a **new, dedicated repo** (`jryan340/cozy-commute`), separate from
  club-runner (a fitness/club app mid-rename to "Sweep"). The user created it.
- This handoff doc is committed here as foundational context.
- The original draft also exists in `club-runner` on branch
  `claude/cozy-train-game-idea-llhno5` (historical; this copy supersedes it).

---

## Next steps (pick up here if context is lost)

1. Confirm the stack (Next.js + Supabase default).
2. Scaffold the project.
3. First buildable slice: a **single shared departure**, a server-authoritative
   clock driving a simple alive window, Realtime Presence showing who's aboard,
   and miles ticking up. Everything else (upgrades, planes, stats card, native
   Live Activities) layers on after that vertical slice works.
