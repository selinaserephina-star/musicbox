---
name: soundbox-arrange
description: Translate music-production intent (mood, arrangement, sound design) into SoundBox features. Use when someone wants help composing, arranging, or shaping sounds in SoundBox — picking scales/chords, building sections/bridges, filter and delay moves, or making a part feel a certain way.
---

# Arranging & sound design in SoundBox

Turn what the musician wants ("lift into the chorus", "chill it", "too busy") into concrete SoundBox moves.

## Key, chords, mood
Use the **Chords & Scales helper** (pick Key + Scale). Moods: Major = uplifting, Minor = dark/emotional, Dorian = jazzy-cool, Lydian = dreamy, Phrygian = exotic/tense, Mixolydian = bluesy, Harmonic Minor = cinematic. It shows the in-key chords (click to hear), **borrowed/color chords** (♭VII ♭VI ♭III iv — spice that stays consonant), and **related keys** (up/down a 5th, relative) for modulation. Progressions can be dropped straight in as a chord loop. In-key notes glow on the on-screen piano.

## Build the structure
Each **Part** holds a beat + loop layers and its **own key, scale and tempo**. Build a verse, then **🌉 Bridge** to spawn a contrasting section auto-modulated to a related key with lighter drums. Line parts up in the **Song builder** (e.g. `A×8  B×8  Bridge×4  B×8`). Sections can each be a different key/feel.

## Make it move
- **Live Dashboard:** close the master **Filter** in a breakdown and open it up into the drop — the spectrum and movement graph react live. Shape the **Delay** (mix / time / feedback) for spacious echoes in a bridge.
- **Effects:** **Drive** for grit, **Chorus** for width/thickness.
- **Per-layer speed** (¼×/½×/2×): a slow pad under fast drums, or a double-time topline.
- **Timing** 1/16 ↔ 1/32 for finer placement; **loop length** 1–8 bars so phrases breathe.

## Intent → move
- **"Too busy / needs space":** remove notes, **Mute** layers per section, use a longer loop length, or make a stripped **Bridge**.
- **"Lift into the chorus":** modulate up a 5th (related key), open the filter, add a second **clap on beat 4 of the last bar** to flag the change.
- **"Chill it":** lower BPM, **Chill** drum kit, soft pads at ½×, jazzier chords, a jazzy `ii–V–I` progression.
- **"Go up an octave / more distance between notes":** shift the keyboard octave (Z/X) before recording; write fewer, longer notes; widen a bridge with a longer loop.
- **Quick starting point:** ✨ Suggest beat / bassline / melody generate in the current key — then edit.

Pair this with **soundbox-scriptlet** to write any of these as text, and **soundbox-dev** to change the app itself.
