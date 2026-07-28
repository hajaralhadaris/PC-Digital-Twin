# PC Digital Twin — Interactive Component Inspector

An interactive 3D digital twin of a desktop PC, built with **Three.js**. Explore a fully modeled computer from the outside in — rotate it, explode it apart CAD-style, and click any component to see its real specifications.

Runs entirely in the browser from a **single HTML file**. No Node.js, no npm, no build tools, no frameworks — just open `index.html`.

---

## Features

- **14 modeled components**: Computer Case, Motherboard, CPU, CPU Cooler, RAM (x2), GPU, SSD (M.2), HDD, PSU, Case Fans, PCIe Slots, SATA Ports, CMOS Battery, Chipset
- **Realistic geometry** — no plain cubes. Fan blades use curved extruded shapes, coolers have cylindrical copper heat pipes and stacked aluminum fins, the CPU has a distinct IHS, the GPU has a shroud/backplate/three fans, etc.
- **Click-to-inspect**: Raycaster-based selection highlights the part and opens an info panel with:
  - Component name
  - Description
  - Function
  - Specifications (model, generation, core count, clock speeds, etc.)
  - Icon
- **Exploded view** — separates every component from the motherboard smoothly, like CAD software
- **Camera controls** — orbit, pan, zoom (OrbitControls), plus a Reset Camera button with smooth easing
- **Auto rotate** toggle for hands-free viewing
- **Floating labels** for every component, projected into screen space and updated every frame
- **RGB lighting animation** — hue-cycling accent lights and LED strips
- **Hide/Show Case** — remove the case to see the internals clearly
- **Wireframe mode** — toggle wireframe rendering across all meshes
- **Professional lighting** — ambient + shadow-casting directional light + point lights, soft PCF shadows, ACES tone mapping
- **Dark engineering-style environment** with a reflective floor and blueprint grid

---

## How to Run

1. Download `index.html`
2. Double-click it (or open it) in any modern browser (Chrome, Edge, Firefox, Safari)
3. That's it — no server, no install, no dependencies to set up

> Requires an internet connection on first load, since Three.js and OrbitControls are loaded from a CDN.

---

## Controls

| Action | How |
|---|---|
| Rotate camera | Left-click + drag |
| Pan camera | Right-click + drag |
| Zoom | Scroll wheel |
| Inspect a component | Left-click on it |
| Reset camera | "Reset Camera" button |
| Auto-rotate | "Auto Rotate" button |
| Show/hide labels | "Toggle Labels" button |
| Explode/assemble | "Exploded View" button |
| Hide/show case | "Hide Case" button |
| Wireframe | "Wireframe" button |
| RGB animation | "RGB Lighting" button |

---

## Tech Stack

- [Three.js](https://threejs.org/) (r128) — 3D rendering, loaded via CDN
- [OrbitControls](https://threejs.org/docs/#examples/en/controls/OrbitControls) — camera interaction, loaded via CDN
- Vanilla JavaScript, HTML, CSS — no framework, no bundler
- Google Fonts: IBM Plex Mono (technical/data text) + Inter (UI text)

---

## Project Structure

Everything lives in one file, `index.html`, organized into clear sections:

```
index.html
├── <style>            → Dark "engineering dashboard" UI, glassmorphism panels
├── <body>             → Top bar, control dock, info panel, label layer
└── <script>
    ├── State & data     → COMPONENT_DATA (specs/descriptions), ICONS, state flags
    ├── buildMaterials() → Shared/reused Three.js materials (PCB green, steel, copper, etc.)
    ├── init()           → Scene, camera, renderer, controls, raycaster setup
    ├── createLighting()
    ├── createFloor()
    ├── createCase()
    ├── createMotherboard()
    ├── createCPU() / createCPUCooler()
    ├── createRAMSticks()
    ├── createGPU()
    ├── createSSD() / createHDD()
    ├── createPSU()
    ├── createCaseFan() / createFanBlades()
    ├── createChipset() / createCMOSBattery()
    ├── createPCIeSlots() / createSATAPorts()
    ├── buildPC()          → Assembles + registers every component
    ├── registerComponent() → Shared registration: scene add, label, raycast target
    ├── createLabel() / updateLabels()
    ├── onClick() / onHover() / selectComponent() → Raycaster interaction
    ├── createUI()          → Wires up all control-dock buttons
    └── animate()           → Main render loop (fans, RGB, explode lerp, camera easing)
```

Each hardware part has its own `createX()` function, and every part is registered through a single shared `registerComponent()` helper — so adding a new component means writing one function and one registration call, without duplicating scene/label/raycast logic.

---

## Notes

- All content and specifications (CPU, GPU, RAM models, etc.) are illustrative examples for educational purposes, based on realistic real-world hardware naming and specs.
- The scene is fully self-contained — component positions for the exploded view are hand-tuned per part rather than computed automatically.
- Browser storage (localStorage/sessionStorage) is intentionally not used; all state lives in memory for the current session.
