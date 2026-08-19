# ReliefGen

Turn an STL or OBJ model into a greyscale **depth map** (8- or 16-bit PNG) for bas-relief CNC routing and laser engraving — entirely in the browser, no upload, no install. Includes a lit 3D preview that shows how the map will actually carve.

**Live:** `https://<your-username>.github.io/<your-repo>/`

## What it does

An orthographic camera renders the model to a linear depth map: near = light, far = dark (or inverted). The depth is normalised to the model's real surface extent and packed to 24-bit precision before being written out as an 8- or 16-bit PNG, ready to drop into a CAM package as a heightfield.

## Features

- **Import** STL or OBJ by drag-and-drop or file picker.
- **Square, letterboxed viewport** — what sits in the square is exactly what exports.
- **View & transform** — orbit, zoom, fit, view presets, and per-axis move/rotate.
- **Depth window** — backing plane (floor cut) and ceiling window with rolloff to compress geometry past a chosen depth instead of hard-clipping it.
- **Tonal control** — independent Clip highs / Clip lows (percentile range-fill), Output floor / ceiling (compress the result into a physical carve band), gamma, invert, and triangular dither to kill contour banding.
- **Local detail** — base/detail decomposition: flatten the global projection so a jutting feature stops hogging the range, and boost fine local transitions (tooth gaps, sutures) that would otherwise be crushed.
- **Relief detail** — smoothing to de-step flats and an unsharp edge-depth pass to deepen transitions.
- **Live depth preview** — the range/clip, output band, plane, ceiling and gamma update in real time.
- **Lit 3D tab** — freezes the current depth into a carved board and shades it (wood / steel / plain, movable key-light pad, adjustable relief height and base thickness). Fed live from the current depth. Exports the board itself as an STL.
- **Export** — 8- or 16-bit greyscale PNG at your chosen resolution with optional supersampling.

## Usage

1. Open the page and drop in an STL or OBJ.
2. Orbit/transform to frame the face you want to carve inside the square.
3. Tune the depth window, tonal and detail controls (tick **Depth preview** to see them live).
4. Switch to **Lit 3D** to sanity-check how it reads as a carving.
5. Set resolution and bit depth, then **Generate depth map**.

Everything runs client-side; nothing leaves the browser.

## Deploy on GitHub Pages

1. Create a repo and add `index.html`, `README.md`, `LICENSE`, and `.nojekyll`.
2. Push to the `main` branch.
3. Repo **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, folder = `/ (root)`.
4. Wait for the build, then visit `https://<your-username>.github.io/<your-repo>/`.

The `.nojekyll` file tells Pages to serve the files as-is rather than running them through Jekyll.

## Dependencies

Loaded at runtime from cdnjs (no build step, no bundling):

- [three.js](https://threejs.org/) r128 — MIT
- [pako](https://github.com/nodeca/pako) 2.1.0 — MIT

A network connection is required for the CDN scripts. To run fully offline, download those two files and point the `<script>` tags at local copies.

## Browser support

Needs WebGL. The Lit 3D tab opens a second WebGL context; if a browser refuses it, the depth tool is unaffected. Reading back high-resolution render targets is memory-heavy — very large export sizes are capped to the GPU's maximum texture size.

## License

MIT — see [LICENSE](LICENSE).
