# Agarbatti Vertical Dryer — 3D Airflow Model

An interactive, single-file 3D visualization of an ACP (agarbatti/incense stick) vertical thermal drying cabinet. Built with [Three.js](https://threejs.org/) (r128), it renders the full cabinet structure, animates the internal airflow path, and lets you inspect every component, its dimensions, and its role in the drying process.

## Overview

The model represents a **75 × 75 × 100 cm** cabinet with **15 mesh racks** (30 tray positions) that dries agarbatti sticks using a single-pass hot-air circuit:

```
Intake → Blower → Heating Coil → Plenum Plate → Y-Splitter
       → Plenum Walls (L/R) → Rack Zone (15 levels) → Exhaust Chimney → Exhaust Vents
```

A single centrifugal blower draws air in, a resistive coil heats it to ~60°C, a perforated plenum plate homogenizes the flow, and a Y-duct splitter feeds two vertical plenum walls that deliver hot air across all 15 rack levels. Moist air converges on a central perforated chimney and exits through roof vents.

## Getting Started

No build step or install required — it's a single self-contained HTML file.

1. Open `index.html` in a modern desktop browser (Chrome, Edge, Firefox, Safari).
2. An internet connection is needed on first load to fetch Three.js from the CDN (`cdnjs.cloudflare.com`).

## Controls

### Camera
- **Isometric / Front / Top / Side** — jump to preset camera angles.
- **Auto-rotate** — toggle continuous orbit around the cabinet.
- Mouse drag to orbit manually, scroll to zoom (standard orbit controls).

### Structure
- **Cabinet skin** — switch the outer shell between Glass (translucent), Frame (hidden, wireframe-style), and Solid.
- **Show labels** — toggle the callout labels with leader lines pointing at each part.
- **Show dimensions** — toggle on-screen dimension annotations.
- **Pull handle** — extend/retract the trolley-style telescoping handle.

### Door & Rack Loading
- **Front door** — open/close the access door.
- **All racks loaded** toggle, or **Empty ALL / Refill ALL** — animate every rack (door opens, racks slide out in a staggered cascade, sticks swap, racks slide back, door closes).
- **Select rack** + **Empty this rack / Refill this rack** — run the same load/unload animation on a single chosen rack (1–15).

### Airflow Simulation
- **Animate airflow** — toggle the particle-based airflow visualization.
- **Flow speed** — adjust particle speed (0.2× – 3×).
- **Particle density** — adjust the number of airflow particles (10–120).

### Stage Focus
Buttons to jump the camera to, and highlight, each functional stage/part:
Intake/Cooling, Blower, Heating Coil, Plenum Plate, Y-Splitter, Rack Zone, Exhaust Chimney, Exhaust Vents, Trolley Wheels, Pull Handle, Access Door, and the three DS18B20 rack-temperature sensors plus the DHT22 exhaust sensor.

Clicking any part directly in the 3D view selects it the same way and opens an info panel describing that component.

### Part Dimensions (cm)
A scrollable reference table listing the physical dimensions of every modeled component (cabinet shell, power/BMS/battery units, blower, heating coil, plenum plate, ducting, rack trays, chimney, exhaust vents, etc.).

## Modeled Components

| Stage | Components |
|---|---|
| 1 & 2 — Intake / Cooling | Power socket unit, BMS unit, battery block |
| Blower + Heating | Centrifugal blower, single 60°C heating coil |
| 2.5 — Plenum | Perforated plenum baffle plate (8 metered holes) |
| 3 — Distribution | Y-duct splitter, left & right plenum walls, 15 mesh rack levels (30 trays) |
| 4 — Exhaust | Central perforated exhaust chimney, roof exhaust vents |
| Control | ESP32 + driver PCB enclosure |
| Portability | Trolley wheels, retractable pull handle |
| Access | Front hinged door with viewing window |
| Sensors | 3× DS18B20 temperature probes (bottom/mid/top rack), 1× DHT22 humidity/temperature sensor (exhaust) |

## Technical Notes

- **Rendering**: Three.js r128, loaded via CDN, `WebGLRenderer` with sRGB output encoding.
- **Units**: all dimensions are in centimeters and match the original source drawing.
- **Interaction layer**: HTML/SVG overlays (`#label-layer`, `#leader-layer`) draw the callout labels and leader lines on top of the WebGL canvas, kept in sync with 3D positions every frame.
- **Animation loop**: a single `requestAnimationFrame` loop drives blower rotation, coil glow pulsing, camera auto-rotate, airflow particles, door/handle/rack transitions, and label positioning.
- **Responsive layout**: side panels narrow and the dimensions readout hides below 900px viewport width; the camera view is re-centered on resize.

## File Structure

This is a single file with no external dependencies besides the Three.js CDN script:

```
index.html   — HTML structure, CSS, and all JavaScript (scene setup, UI wiring, animation loop)
```

## Browser Requirements

- WebGL-capable browser.
- Desktop use recommended; layout is usable but more cramped on narrow/mobile screens.
