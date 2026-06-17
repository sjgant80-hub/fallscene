# FallScene

**Sovereign 3D scene builder · single HTML file · prime 1453 · v1.0.0**

The Dimension wedge of **FallStudio** (phase 3). Replaces Adobe Dimension for basic 3D scene composition: drop primitives, push lights around, render a frame, export GLB. Runs from `file://`. No account, no cloud, no telemetry.

---

## For people who just want to make a scene

1. Open `index.html`.
2. Click a primitive from the left sidebar (Cube, Sphere, Cylinder, Cone, Torus, Plane).
3. Click an object in the canvas to select it · drag the gizmo to translate / rotate / scale.
4. Edit position, rotation, scale, material, and geometry params in the right panel.
5. Add point and directional lights from the lighting panel.
6. Pick a background mode (solid, gradient, or studio).
7. `file → export PNG` for a 2× render · `export GLB` for the model · `export JSON` to reload later.

**Ω autopilot** (Ctrl+K) gives you starter templates (`product render`, `logo plinth`, `hero shot`) and a natural-language scene parser ("red cube and blue sphere") which works as keyword routing at T0 and as an LLM call at T3.

Everything you build autosaves to your browser's IndexedDB. Close the tab and reopen — your scene is still there.

### Keyboard

- `Ctrl+K` — Ω palette
- `T` / `R` / `S` — translate / rotate / scale mode
- `Delete` / `Backspace` — remove selected object
- `Esc` — close palette / modal

### Tiers

- **T0** (default) — everything in the brief works offline, including templates and keyword scene parsing
- **T2** — local Ollama at `127.0.0.1:11434` is auto-detected
- **T3** — paste a key (Anthropic, Gemini, OpenAI, OpenRouter) in settings; the Ω palette will route descriptions like "a crimson torus next to a gold sphere" through the LLM into structured scene JSON

---

## For developers

Vanilla JS. Inline CSS. One HTML file, ~75KB. Three.js is the **only** external dependency, loaded via importmap from unpkg — see below.

### The Three.js CDN dependency · why it's exempt

The shared build doctrine says no CDN dependencies. **FallScene is the declared exception** — Three.js is a 600KB+ engine you cannot meaningfully ship inline in a single HTML file without destroying the size budget for every other tool in the FallStudio suite. The same precedent was set by the konomi-cube visualization. The importmap pins exact versions:

```html
<script type="importmap">
{"imports":{
  "three":"https://unpkg.com/three@0.160.0/build/three.module.js",
  "three/addons/":"https://unpkg.com/three@0.160.0/examples/jsm/"
}}
</script>
```

When unpkg is unreachable, the canvas stays blank but every other shell feature (file menu, settings, palette HTML) still mounts. Mirror to your own CDN by changing those two URLs.

### Architecture

- **WebGLRenderer** with `preserveDrawingBuffer:true` for PNG export at 2× pixel ratio
- **OrbitControls** + **TransformControls** from `three/addons`
- **GLTFExporter** / **GLTFLoader** for round-trip GLB
- Scene tree, properties panel, lighting panel are all rebuilt from `state` arrays on change — no virtual DOM, no framework
- IndexedDB (`fallscene` db, `kv` store) holds the autosave snapshot and settings keys
- Cascade T0/T2/T3 router is the canonical estate shape

### Estate integration

- `KONOMI` sovereign shim (inert, baked)
- `BroadcastChannel('fall-signal')` for inter-tool hello/ping/pong
- `postMessage` API: `{target:'fallscene',action:'ping|add|export-png|state',kind,opts}`
- PWA manifest baked via `data:` URL
- Prime: **1453**

### File deliverables

```
index.html   the tool
README.md    this file
LICENSE      MIT
.nojekyll    GH Pages legacy deploy marker
```

### Build / deploy

No build step. Open `index.html`. To ship to GitHub Pages: push these four files and set Pages to legacy / `main` / root. The `.nojekyll` ensures Pages serves the importmap untouched.

---

**Seal:** ◊·κ=1 · MIT · sovereign single HTML · part of the FallStudio plan
