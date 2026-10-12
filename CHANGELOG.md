# Changelog

## v2.0.0 — 2026-10-13「全程活体」首发

- All-scene talking characters: audio-driven lip sync, dual-engine auto decision (H3 ≤5.1s / Wan2.2 S2V 720p for longer), silent-strip + stream verification built in
- Full parameterization: one config drives identity/costume/9 case prompts/narrations/watermark/Blender
- Blender data-gallery deterministic camera moves (headless Cycles, camera-parented fill light, EEVEE-headless avoidance, zoompan fallback)
- Text-direct-out composition cards with proofreading checklist + blank-image guard + auto retrieval
- Five QC gates: text stability / lip motion / hands / A/V alignment / transcription hook
- references/: engine decision tree + 33 recorded pitfalls
