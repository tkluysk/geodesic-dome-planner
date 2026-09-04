# 2V Geodesic Dome Pole Planner

A single-file web app for planning the struts of a two-frequency (2ν) icosahedral
geodesic dome — the hub-and-stick form factor sold as a kit.

Type a ground radius, say how much each hub swallows at a strut end, and it gives
you every cut length, the piles to sort, and the poles to buy.

## Two ways to cut the dome

**Equator** — the canonical 2ν dome: 26 hubs, 65 struts, only two strut lengths
(A = 0.61803 R, B = 0.54653 R). Its ten base hubs already land on one level plane,
so it stands flat with no packing and no special parts.

**Shoulder ring** — a shallower, wider dome standing on the ten-point zig-zag line
higher up the sphere. Those hubs alternate in height on the true sphere, so the
five raised ones are dropped onto the floor plane. That re-lengthens the struts
touching them, which the cut list reports as two extra types:

| type | what it is |
| ---- | ---------- |
| A, B | the exact geodesic chords, unchanged above the cut |
| C    | the flattened base-ring chords |
| D    | the sloping struts feeding a dropped hub |

The resulting footprint is a decagon whose plan radius alternates slightly
(0.8507 R / 0.8944 R), because flattening moves hubs vertically only.

## Hub take-off

Every strut is cut short by a constant amount at **both** ends to seat in its hub:

```
cut length = geodesic chord − 2 × take-off
```

Set it once and it applies across the whole cut list.

## Running it

It's one self-contained HTML file — no build, no dependencies.

```
python3 -m http.server 8742
```

then open <http://localhost:8742>. Or just open `index.html` in a browser.

## Geometry notes

The mesh is built by subdividing a pentagon-up icosahedron once and projecting
every vertex to the sphere, then cutting at the chosen ring. Strut counts, hub
counts and dimensions are all measured from that mesh at render time rather than
hard-coded, so the drawing and the cut list can never disagree.
