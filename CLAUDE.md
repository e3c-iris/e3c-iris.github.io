# IRIS landing page

Static GitHub Pages site for IRIS, an event sensing project. `index.html` is the whole site; logos are `iris-logo*.svg` / `.png`.

## Rule: the animation is frozen

Do not change the home-page animation in `index.html` (the inline script and the `#field` / `#spark` canvases) unless the owner explicitly asks. It was finalized on 2026-10-15. Other edits (text, layout, meta, logos) are fine, but leave the animation's code and behavior untouched; if a change would affect it, ask first.

## How the animation works

Concept: the iris of an eye + the Japanese iris flower (*Iris ensata*) + the purple palette + spikes in a spiking neural network. Everything is plain canvas JS/CSS inline in `index.html`, no libraries.

- **`#field` canvas** (behind the content): radial fibers of leaky integrate-and-fire neurons around the logo `iris-logo-transparent.png`.
  - Each fiber starts just outside the nearest dot of the logo's eye (`innerR`), skipping the IRIS lettering (`TEXT_BOX`). Dots shrink and pack tighter toward the logo; outer dots and fibers fade out, with no outer border line.
  - Spikes travel **inward**, from the outer rim to the logo. Volleys start at the rim every 3–6 s; pointer movement injects current near the cursor (an event-camera nod).
- **Eye ↔ flower morph**: the outline morphs from a circle (eye) to a Japanese iris over `MORPH_PERIOD = 40` s and back. The flower is three broad ruffled falls (at 90°, 210° and 330°) with three small standards between them, defined as disks in `PETALS`. It opens only partway (`FLOWER_STRENGTH = 0.7`; 1 is the full shape). Spikes at the base of the falls glow gold.
- **`#spark` canvas** (above the logo, `pointer-events: none`): spikes jump from the fibers onto the logo's own dots and hop inward (`logoNext`) to the pupil, which flashes larger.
  - `LOGO` holds the 127 circle coordinates copied from `iris-logo-transparent.svg` (320-unit viewBox). `PUPIL = 100` is the dot at (160, 160).
- **Palette**: `#b394df`, `#c5a6eb`, `#f5efff` on `#171321`.
- **Reduced motion**: with `prefers-reduced-motion`, a single still frame is drawn.
- **Removed on request**: a spike raster strip at the bottom, the ring around the eye, and the petal outline.

## If the logo changes

`LOGO`, `PUPIL` and `TEXT_BOX` must be re-derived from the new SVG; otherwise spikes land in the wrong places.

## Testing renders

Headless Chrome barely advances `requestAnimationFrame`, and an early resize resets the network state. To check a given moment, render a scratch copy that runs `frame(1/60)` up to a fixed time inside a `setTimeout`, then screenshot it.
