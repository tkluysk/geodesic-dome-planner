# 2V Geodesic Dome Pole Planner

**→ [Open the planner](https://tkluysk.github.io/geodesic-dome-planner/)**

A single-file web app for planning the struts of a two-frequency (2ν) icosahedral
geodesic dome — the hub-and-stick form factor sold as a kit.

Give it the footprint you want and how much each hub swallows at a strut end, and
it works out every cut length, names every individual strut, packs them onto your
actual stock lengths, and shows the result on a model you can turn.

Everything is metric: millimetres throughout.

## The dome

**Equator cut** — the canonical 2ν dome: 26 hubs, 65 struts, two strut lengths
(A = 0.61803 R, B = 0.54653 R). Its ten base hubs already land on one plane, so it
stands flat with no packing and no special parts.

**Shoulder cut** — shallower and wider, standing on the ten-point zig-zag line
higher up the sphere. Those hubs alternate in height on the true sphere, so the
five raised ones are dropped onto the floor plane. That re-lengthens the struts
touching them, which the cut list reports as extra types.

## Shaping it

| control | what it does |
| ------- | ------------ |
| **Short / long diameter** | An oblong footprint. Equal values give a genuinely round dome. |
| **Height** | Vertical stretch, 50–200%, snapping to the natural height at 100%. |
| **Top ring spread** | Pushes the five crown hubs outward, flattening the cap. |
| **Ground ring** | Struts, or a rope — the bottom ring only takes tension. |
| **Move a hub** | Click any hub in the model and nudge it radially or vertically. |

Each of these re-measures the struts it affects rather than distorting them, and
the affected struts get their own type in the cut list:

| type | what it is |
| ---- | ---------- |
| A, B | the exact geodesic chords |
| C, D | flattened base ring, and the struts feeding a dropped hub |
| E, F | the spread crown ring, and the struts feeding it |
| G    | anything touching a hub you moved by hand |

Stretching a dome breaks the chord symmetry, so one type fans out into several
lengths — `A1`, `A2`, … Lengths closer than the merge tolerance are treated as a
single cut, since nobody cuts a 1 mm difference on purpose.

## Naming

Every strut has its own id — `A1-a`, `A1-b` — assigned deterministically down and
around the dome, so the same design always produces the same ids and a printed
list matches the model. Turn labels on in the 3D view to read them off while
building. Hovering a strut, a cut-list row, or a piece on the cutting plan
highlights that length everywhere at once.

## Cutting plan

List up to 20 stock lengths, each with an optional quantity you actually have.
The optimiser packs struts onto poles — first-fit-decreasing with best-fit into
open poles, run once per candidate stock preference, keeping whichever plan buys
the least — and charges saw kerf between pieces. Each pole shows what it becomes,
piece by piece.

## Hub take-off

Every strut is cut short by a constant amount at **both** ends to seat in its hub:

```
cut length = geodesic chord − 2 × take-off
```

## Saving and sharing

Settings persist in the browser. **Download design** writes the whole design as
JSON and **Upload design** reads it back. **Cut list CSV** exports the strut
table, the pole-by-pole cutting plan, and every strut with its id and 3D endpoint
coordinates — enough to rebuild the dome from the file alone.

## Running it locally

One self-contained HTML file — no build, no dependencies.

```
python3 -m http.server 8742
```

then open <http://localhost:8742>. Or just open `index.html` in a browser.

## Geometry notes

The mesh is built by subdividing a pentagon-up icosahedron once and projecting
every vertex to the sphere, then cutting at the chosen ring. Strut counts, hub
counts and dimensions are all measured from that mesh at render time rather than
hard-coded, so the drawing and the cut list can never disagree.

The base is a decagon, not a circle: one axis passes through a vertex and the
other through the middle of a flat, so its extents differ by about 5%. Both axes
are scaled against the circumscribed radius, which is why a round dome stays
round and reports an honest 3400 × 3234 footprint rather than being squashed to
make the numbers look tidy.
