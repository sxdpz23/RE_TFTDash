# Northstar

Personal Android companion app for a **Royal Enfield Himalayan 450** motorcycle,
designed to work with the bike's **Tripper** dash. Single user, Android-only,
targeting a **Nothing Phone 3**. Built for the author's own bike; may be
open-sourced. No user personas, no client/enterprise concerns.

> Independent project, not affiliated with or endorsed by Royal Enfield. "Royal
> Enfield", "Himalayan", and "Tripper" are trademarks of their owners, used here
> only to describe compatible hardware. The dash link is an unofficial
> interoperability feature for hardware the user already owns.

## Primary goal

Low-power **navigation shown on the Tripper dash** (a small **round TFT display**)
without cooking the phone. Northstar **renders the map off-screen and
hardware-encodes H.264**, so the phone screen can stay **OFF** during the ride.
That single architectural choice — keeping the screen off — is the whole point of
the project.

## Dash link

- Talks to the Tripper dash over Wi-Fi using its documented behaviour; the
  open-source **better-dash** project (Apache-2.0) is used as a reference:
  https://github.com/norbertFeron/better-dash
- After a handshake, the dash decodes an **H.264/RTP stream over UDP port 5000**.
  It does not care what produces the video.
- Dash behaviour varies by firmware. The author's dash runs firmware **11.63** —
  the link layer is validated against it before anything else is relied on.
  Treat that as Phase 1, step 1.

## Core user flow

1. Share a destination from **Google Maps** into Northstar.
2. Northstar previews the route, tap **Send to Dash**.
3. While riding, the dash shows the map; the bike's **physical joystick**
   pans/zooms. The phone screen stays off.

## Tech stack

- **Language:** Kotlin (Android, native).
- **Map rendering:** off-screen render → `MediaCodec` hardware H.264 encode →
  `MediaCodec`/RTP to the dash on UDP/5000.
- **Maps/offline:** offline maps preferred for riding; an OSM stack (MapLibre).
  Decide during build — don't over-engineer the map layer up front.
- **Backend:** **Firebase** for email auth + multi-device sync (so installing on a
  second device restores data). Single user, but sync is wanted.
- **Local persistence:** on-device **SQLite** as the source of truth; Firebase
  syncs it.

## Features

1. **Navigation** (primary) — receive shared location, route preview, send to dash,
   joystick pan/zoom, turn-by-turn.
2. **TTS / voice overlay** — toggle per trip: off / chime-only / full. We own the
   TTS layer, so this is just a setting.
3. **Maintenance log** — chain cleaning + lube tracker, service intervals, due
   reminders.
4. **Fuel diary** — fill-ups, mileage/efficiency calculations, cost tracking.
5. **Telemetry / ride history** — distance, duration, map snapshot per ride.
6. **Media controls** — now-playing overlaid onto our own video frame. Note:
   Android restricts answering/ending calls programmatically — calls are
   realistically display + reject + alert only.

## Build phasing

Sequenced so standalone, useful parts land first and the hardware-dependent dash
link is isolated:

1. **Phase 1:** Kotlin link layer validated against firmware 11.63 (stream a static
   test video to the dash). In parallel, the standalone features (maintenance log,
   fuel diary, telemetry) — no dash dependency, usable day one.
2. **Phase 2:** off-screen MapLibre/map → MediaCodec → dash with screen OFF. Proves
   the power fix.
3. **Phase 3:** GPS + offline routing + turn-by-turn rendering.
4. **Phase 4:** polish — TTS, day/night, reconnect handling, settings, media.

## Hard constraints / non-goals

- **Android only.** No iOS.
- **One bike** (Himalayan 450), **one dash target** (Tripper). No generic
  multi-bike / multi-dash abstraction.
- No personas, no branding-as-product, no team/lab infrastructure. Keep it lean.
- The dash-streaming core requires **hardware-in-the-loop validation on the bike** —
  that part can't be verified from code alone.

## Reference docs in this repo

- `@docs/HLD-LLD.md` — full architecture (high- and low-level design).
- `@docs/design/` — UI prototype and screen specs (from Claude Design).
