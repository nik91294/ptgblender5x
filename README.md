# Pidgeon Tool Bag (PTB) — Blender 5.x Compatibility Fork

This is a community-maintained compatibility fork of **Pidgeon Tool Bag**, updated to work on **Blender 5.0+ / 5.1**. It is **not the official release** and is **not affiliated with or endorsed by the original creators** — please support their original work using the links below.

## Credit

All original design, features, and code in this add-on were created by:

- **Kevin Lorengel** — [BlenderMarket](https://blendermarket.com/creators/kevin-lorengel) · [YouTube](https://www.youtube.com/channel/UCgLo3l_ZzNZ2BCQMYXLiIOg)
- **Crafto Hohenvels**
- **Pidgeon Tools** — [Discord](https://discord.gg/cnFdGQP) · [Instagram](https://www.instagram.com/pidgeontools/) · [Twitter/X](https://twitter.com/PidgeonTools)

If you use and enjoy this tool, please consider supporting the original creators directly through the links above.

## What is Pidgeon Tool Bag?

A suite of Blender add-ons bundled together:

- **Super Advanced Camera** — extra camera color-grading and image-effect tools built on the compositor (bokeh, vignette, film grain, halftone, chromatic aberration, and more).
- **Super Image Denoiser** — an enhanced multi-pass and temporal (animation) denoising workflow built on top of Cycles' denoiser.
- **Super Project Manager** — project/folder-structure setup and management.
- **Super Fast Render** — Cycles benchmarking and render-setting optimization tools.
- **Super Res Render** — tiled high-resolution rendering.
- **Super Render Farm** — basic network render farm management (in development).

## About this fork

Blender 5.0 made sweeping changes to the compositor: many node properties were converted into input sockets, several nodes were renamed or restructured (sometimes with sockets reordered), a few nodes were removed outright, and core scene/compositor APIs (`Scene.node_tree`, the dedicated Composite node) were replaced. This fork updates every part of the add-on suite that touched those APIs so it runs on Blender 5.0.1 / 5.1 instead of crashing on load or silently producing wrong results.

Notable fixes in this fork:
- `Scene.node_tree` → `Scene.compositing_node_group`, and the removed Composite node → Group Output.
- Multiple compositor nodes (`Gamma`, `Math`, `Map Range`, `Color Ramp`, `MixRGB`, `Value`) replaced with their current Shader-node equivalents.
- Numerous node property → input-socket conversions across Denoise, Color Balance, Curves, Hue Correct, Invert, Filter, Dilate/Erode, Scale, Lens Distortion, Set Alpha, Glare, Switch, Bokeh Image, Vector Blur, and Bilateral Blur — including cases where socket *order* changed and enum values changed casing (e.g. `"ACCURATE"` → `"Accurate"`).
- The Halftone effect's dot-screen pattern, previously built on the removed legacy Texture-datablock node, rebuilt using current shader nodes.
- The File Output node's rewritten multi-slot API.
- A handful of unrelated logic bugs found along the way (a render-progress loop that never terminated correctly, a settings-collection clearing bug, an unregistered-property crash, and a couple of others).

This fork was adapted with AI assistance (Claude), cross-checked against Blender's own release notes and verified interactively against a live Blender 5.1 session. It has **not** been exhaustively tested across every feature — if you find something broken, please open an issue (or better, a pull request).

## Installation

1. Download this repository as a ZIP (or clone it).
2. In Blender: `Edit > Preferences > Add-ons > Install from Disk`, and select the ZIP (make sure `__init__.py` sits at the root of the folder you zip, not nested one level down).
3. Enable "Pidgeon Tool Bag (PTB)" in the Add-ons list.
4. Some features (texture optimization, benchmarking plots) use optional Python dependencies (`opencv-contrib-python`, `numpy`, `matplotlib`). Use the "Check Dependencies" / "Install Dependencies" buttons in the add-on preferences to install them. This runs `pip` on the main thread, so Blender will appear unresponsive (not crashed) while it downloads — this can take a few minutes.

## Known limitations

- `CompositorNodeVecBlur`'s min/max speed clamping and curved-interpolation options were removed in Blender 5.x with no replacement socket; that specific behavior is currently unavailable in Super Image Denoiser's temporal alignment.
- Only the code paths that have actually been exercised in Blender 5.1 are confirmed working end-to-end; other paths have been checked statically and against the live API but not necessarily run.
- Super Render Farm's networking features are still a work in progress (inherited from the original project).

## License

GNU General Public License v3.0 (see `LICENSE`), same as the original project.
