# skill-promo-video

Digital-human promo video pipeline: turn any skill/product into a「digital-human narrated + real-case showcase」short video (16:9, 60–120s), where **every on-screen character talks** with audio-driven lip sync.

数字人推广视频一键生成：为任意 skill/产品制作「数字人全程口播解说 + 真实案例展示」风格推广短视频——所有场景人物开口说话（音频驱动口型同步），人物服饰与案例提示词全参数化。

## What v2 does（v2 实质能力）

- **All-scene talking characters** — audio-driven lip sync with automatic dual-engine decision: narration ≤5.1s → H3 (ref2va, 8-step INT8); longer → Wan2.2 S2V at native 720p. Generated clips are stripped to silent video (`-an`) and narration is re-injected in compositing for exact A/V alignment.
- **Full parameterization** — one JSON config drives identity anchor, costume (换装), scene background, 9 case prompts (shown verbatim on the composition cards), chart commands, narrations, subtitles, watermark and Blender settings.
- **Blender deterministic camera moves** — headless Cycles「data gallery」: opening dolly-in / closing pull-out with the real chart PNGs as light-box panels; camera-parented fill light; EEVEE headless avoided by design (black-frame risk). Falls back to still-frame + zoompan if the shot fails acceptance.
- **Text-direct-out composition cards** — prompt cards are generated with text rendered natively (no PIL pasting), each prompt ends with a line-by-line proofreading checklist; retrieval includes a blank-image guard.
- **Five QC gates built in** — text stability (first/last frame diff on the text region), lip motion (inter-frame diff, texture-robust), hands (3× crops for finger-by-finger counting), A/V alignment, and a hook for any FunASR-style transcription gate.
- **Battle-tested references** — engine decision tree (`references/digital-human.md`) and 33 recorded pitfalls (`references/pitfalls.md`), distilled from production reworks.

## Structure

```
SKILL.md                     # entry: pipeline, boundaries, safety statement
scripts/
  gen_scene_cards.py         # composition cards / talking first frames (PE text direct out)
  talking_segment.py         # dual-engine talking segments (H3-TALK / S2V / H3-I2V) + preflight
  blender_scene.py           # data-gallery camera moves (headless Cycles)
  style_transfer.py          # optional restyle (Qwen-Image-Edit-2511 Plus)
  tts_batch.py               # batch TTS + mtime/size verification (OOM fake-OK guard)
  make_watermark.py          # signature watermark methodology
  compose_3step.py           # 3-step ffmpeg compositing (video → audio → mux)
  qc_gates.py                # text / lips / hands / av / asr gates
references/
  pipeline.md                # 12-step production SOP
  digital-human.md           # engine decision tree + GPU serialization discipline
  pitfalls.md                # 33 pitfalls (P0 process / P1 content / P2 technical)
  config-template.json       # one-file parameterization template
```

## Requirements（边界）

Local GPU (24G class) with locally deployed inference services: a Qwen-Image-family T2I/edit service (default `127.0.0.1:8190`), an H3 video service (`:8189`), a Wan2.2-S2V-capable service (`:8188`), a local TTS, Blender 4.x+, ffmpeg. Models are obtained from their official release pages — **nothing is bundled**. Without GPU/services the pipeline degrades to a static slides + narration video (all other steps unchanged). See `SKILL.md` §边界与限制.

## Safety & behavior

Everything runs locally: the only network calls are HTTP to user-configured loopback inference services; no telemetry, no background jobs, no dependency installation, nothing leaves the machine. Use only character reference images you are authorized to use.

## Install

SkillHub 搜索 `skill-promo-video`，或 `clawhub install skill-promo-video`。

docsor.cn · © docsor1212
