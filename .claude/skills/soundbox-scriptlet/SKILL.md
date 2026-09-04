---
name: soundbox-scriptlet
description: Write SoundBox scriptlet notation — compact text that compiles to loops in the SoundBox music app. Use whenever someone describes a musical idea (arp, bassline, melody, chord loop, drum pattern) that should become a pasteable scriptlet, or asks about SoundBox scriptlet syntax. Produce valid scriptlet text the user pastes into SoundBox's "Scriptlet" box.
---

# SoundBox Scriptlet notation

SoundBox (a browser music studio) has a **Scriptlet** panel: paste text, click **Compile**, and it becomes a loop. A **melody** scriptlet becomes a loop layer on the currently-selected instrument; a **drum** scriptlet writes the beat grid. The text is the source of truth — the piano roll / grid is just a view of it.

**Your job:** turn a musical description into correct scriptlet text. Output the scriptlet in a fenced code block, plus one line telling the user which Sound (and BPM/key) to pick. Nothing else.

## Time model
Beats; 4 beats = 1 bar (4/4). Loops round up to whole bars. Sub-beat timing comes from fractional durations — there is no forced step grid.

## Melody — one line of space-separated tokens
| Token | Meaning |
|---|---|
| `C4` `A#3` `Gb2` | absolute note: name + optional `#`/`b` + octave. `C4` = middle C (MIDI 60). |
| `+7` `-5` | move in **semitones** from the previous note (a perfect fifth = `+7`). Chaining builds runs/arps. |
| `:N` | duration in **beats** (default 1). `A3:2` holds 2 beats; `:0.5` = eighth. |
| `~` | legato — tie this note into the next. |
| `_` | rest (`_:2` = 2-beat rest). |
| `[ … ]*4` | repeat a group 4 times; nestable. |

**Worked example** — "arp C up in fifths ×4, then A–G–A with A held 2 beats and a legato A→G, whole 4+1 unit ×3, on a bass synth":
```
[ [C4 +7 +7]*4  A3~ G3 A3:2 ]*3
```
That is C→G→D (each a fifth up) four times, then the A–G–A phrase; ×3 → 12 bars.

## Drums — one rule per line, `name: rule`
Drum names: `kick snare hat openhat clap tom rim perc cowbell shaker ride crash hitom lotom conga snap`.

| Rule piece | Meaning |
|---|---|
| `on 2 4` | hit on those beats of the bar (1-indexed). |
| `every 1/16` | a hit every sixteenth. Also `1/8` `1/4` `beat` `bar`, or a number of beats (`every 0.25`, `every 4`). |
| `@ 70` | velocity 0–100. |
| `@ lfo(1 bar, 30-100, sine)` | velocity rides an LFO: period in bars, range lo-hi, shape `sine`/`tri`/`saw`/`square`. |

**Worked example** — four-on-the-floor with swelling hats and a backbeat:
```
kick: every 1
clap: on 2 4
hat:  every 1/16 @ lfo(1 bar, 30-100)
```

## Guidance for good output
- A melody plays on the **current Sound** — always say which to pick (bass synth for basslines, a lead/keys for toplines).
- Stay in one key; resolve phrases toward the root; mind the octave (bass ≈ C2–C3, lead ≈ C4–C5).
- The scriptlet box auto-detects: a line like `kick: …` makes the whole thing a drum scriptlet; otherwise it's melody.
- `name@lastbar:` conditional variants parse but are **not applied yet** — don't use them unless asked.
- If the key/scale isn't given, state the one you assumed so the user can correct it.
