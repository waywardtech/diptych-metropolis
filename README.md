# Diptych Metropolis

A virtual city grown from a painting.

This project translates an abstract mixed-media diptych into an explorable WebXR
world: the paintings' vertical bands become painted skyscraper facades, their
dotted textures become drifting particle clouds, and their looping white
"spaghetti" gestures become skywriting drones that trace long, slowly fading
condensation trails across the sky. You stand at a street corner in a snowy
futuristic metropolis on an icy winter day, with traffic gliding past on
reflective streets and a cloud deck that the tallest towers pierce clean
through. The two source paintings hang as giant murals on the corner towers.

Tap or click anywhere and the scene plays out: your point of view turns toward
a glass elevator car waiting at the corner, walks along the pavement to it, the
doors chime and slide open, you step aboard, turn to face the street, and ride
45 seconds up the outside of a 640-meter tower — floors streaming past the
glass, the street noise falling away below, wind rising — up through the cloud
deck and out into a sunset that only exists above the clouds, arriving at an
observation deck where the drones' contrails loop at eye level. Tap again to
ride back down and watch the city rise to meet you.

Everything is procedural and self-contained in a single HTML file: the facades
are painted with canvas brush strokes at load time, and the entire soundscape —
street rumble, lobby hum, elevator drone with speed-tracked cable whine,
floor-pass chimes, door slides, high-altitude wind — is synthesized live with
the Web Audio API. No asset files, no build step, no external requests
(the A-Frame library is embedded).

## Running it

It needs to be served over HTTP(S) — opening the file directly with `file://`
won't work in most browsers.

**Easiest: GitHub Pages.** Enable Pages on this repo (see below) and open the
URL on any device. Pages provides a trusted HTTPS certificate, which is exactly
what iOS requires before it will grant motion-sensor access for the stereo
headset mode.

**Local:** `python3 -m http.server` in the repo folder, then visit
`http://localhost:8000`. Localhost counts as a secure context, so sensors work
here too.

## Controls

| Platform | Look | Move | Ride |
|----------|------|------|------|
| Desktop | mouse drag | WASD | click anywhere (or walk to the elevator) |
| Phone | one-finger drag | — | tap anywhere |
| Cardboard-style viewer | gyroscope | — | tap before inserting phone |
| WebXR headset (e.g. Quest) | head tracking | controller | trigger |

The **3D SBS** button (bottom-right, or the `S` key on desktop) toggles a
side-by-side stereo mode with gyroscope head tracking for phone-in-headset
viewing — implemented in-page because browsers have dropped native Cardboard
support. On iOS it will ask for motion-sensor permission; allow it. A screen
wake lock keeps the phone from dimming while in the headset.

On a WebXR-capable headset browser, the standard A-Frame VR button provides
true stereo with full tracking.

## The paintings

The murals load from `left.jpg` and `right.jpg` in the repo root. Those files
are not required: if they're absent, the scene paints procedural stand-in
murals in the same visual language. To hang your own diptych, drop two photos
in with those names (crop to the canvas edges for best results). Only commit
images you hold the rights to; the MIT license below covers the code, not any
photographs you add.

## Technical notes

- Built on [A-Frame](https://aframe.io) 1.5.0 (MIT), embedded inline so the
  page is a single dependency-free file (~1.4 MB).
- ~380 towers share a pool of 14 canvas-painted facade textures (with
  per-building UV scaling for variety), keeping GPU texture memory around
  15 MB — small enough for mobile Safari's strict per-tab limits.
- The cloud deck is placed by rejection sampling against every tower's
  footprint, so clouds wrap around the buildings that pierce them and drift
  without ever intersecting one.
- Drone flight paths are collision-checked at build time and climb over any
  tower in their way; their contrails hold for 30 seconds, then fade over 15.
- Sky, fog, and sun color are driven continuously by your altitude, so the
  ascent grades from icy daylight into sunset and the descent plays it in
  reverse.
- On phones, render resolution is capped and particle counts halved; the
  audio is voiced to remain audible on small speakers.

## License

Code is released under the [MIT License](LICENSE). The embedded A-Frame
library is © its contributors, also MIT.
