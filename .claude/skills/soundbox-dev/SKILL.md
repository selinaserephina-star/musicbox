---
name: soundbox-dev
description: Work on the SoundBox app source — its Web Audio engine, sequencer/pattern model, effects chain, save format, and verification workflow. Use when modifying, debugging, extending, or explaining how SoundBox.html works.
---

# Working on SoundBox

SoundBox is **one self-contained file**: `SoundBox.html` (HTML + inline `<style>` + inline `<script>`). No build step, no dependencies, no network calls. It runs by opening in Chrome/Edge — Web MIDI and Web Audio need a secure context, which `file://` and `https://` both provide.

## Ground rules
- Keep it a single file with zero external runtime deps. Match the existing dark UI and terse code style.
- Don't add a framework or build tooling. Edit `SoundBox.html` in place.

## Architecture map
- **Audio graph:** one `AudioContext` `AC`, created on first gesture in `initAudio()`. Sound sources route through `toBus()` (dry + reverb send + delay send) into `master` → `masterFilter` (live master lowpass, for sweeps) → chorus (dry + LFO-modulated delay) → distortion (`WaveShaper`) → `comp` (a limiter) → destination. An `analyser` on `comp` drives the dashboard spectrum.
- **Instruments** live in `PATCHES[]`. A patch = `partials` (each `{ratio,type,gain,detune?}`), ADSR (`atk/dec/sus/rel`), `level`, `cut`, and optional `vibrato`/`breath`/`filterLfo`/`oct`/`q`. `noteOn(note,vel,when)` builds oscillators → biquad filter → VCA (envelope) per note. The Sound menu is grouped by `patchGroup(name)` keyword matching.
- **Drums:** `DRUMS[]` (16 voices), synthesized in `drumHit(i,when,vel)`; `KITS` (Punchy/Chill/Deep) tune the synthesis. **Drum grid cells hold a velocity number 0–1** (or `false`), not just booleans.
- **Sequencer:** `patterns[]` — each `{name,bpm,key,scale,drumGrid,loops}`. `curPat` is the edited one; `drumGrid`/`loops` alias `patterns[curPat]`. `song[]` is the arrangement of `{pat,reps}`. A `loops` layer = `{patchIndex,name,speed,steps}`; `steps` is an array of length `STEPS*bars`, each cell `null` or `[{note,dur}]`. `STEPS` is 16 or 32 (switchable). The `scheduler()` runs on `setInterval` with a lookahead, advancing `currentStep`/`tick`; per-layer `speed` and multi-bar loops key off `lp.steps.length`.
- **Timeline:** `clips[]` (in-memory `AudioBuffer`s) + `timeline[]` placed blocks `{clipId,lane,start,len}`; each clip stores a musical `loopLen`.
- **Effects/dashboard state:** `state.masterCut/masterRes/drive/chorus/delayFb/…`; helpers `setMasterCut/…`, `cutToHz`, `distCurve`.
- **Save/Open:** `.sbx` = JSON of patterns, song, per-part key/scale/bpm, params, `fx`, samples (base64 WAV), and step resolution.

## Verify every change (no browser required)
From the project directory, after editing:
1. **Syntax** — extract the `<script>` body and run `node --check` on it.
2. **Element ids** — every `getElementById('x')` must have a matching `id="x"` in the HTML (grep both, diff).
3. **Mock-DOM boot** — `eval` the script under a stub `document`/`window`/`navigator` to catch load-time errors.
4. **Logic** — for audio/timing/parsing, port the pure function to Node and check outputs (e.g. scheduler step order, scriptlet compile, filter Hz mapping).

Only state that something works after these pass. If a change touches playback, also sanity-check it doesn't leave audio running in a stray tab.
