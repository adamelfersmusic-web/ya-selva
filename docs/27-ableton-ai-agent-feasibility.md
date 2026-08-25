<!-- provenance: idea / exploratory (doc_type: technical-exploration) — pulled from Parachute vault music/ya-selva/ableton-ai-agent-feasibility -->
<!-- status: idea — not built, worth prototyping, not yet prototyped. -->

# Ya Selva — Ableton AI Agent Feasibility

## What this is

An exploration of whether an LLM (Claude) can practically read, reason about, and edit an Ableton Live project — triggered by a real question: could this be useful inside the Ya Selva workflow, specifically for rhythmic/arrangement idea generation during the MIDI-sketch phase.

**Status: idea / exploratory.** Not built. Worth prototyping, not yet prototyped.

## The core technical finding

Ableton shipped an **Extensions SDK** in public beta (June 2026, requires Live 12.4.5+). It is a TypeScript/Node.js toolkit that lets a custom extension read and write a Live Set's structure: tracks, clips, MIDI notes, devices, automation, tempo, scenes, and the currently selected clip/track.

The critical constraint, stated explicitly by Ableton: extensions are **not** for real-time audio or MIDI processing. They run once, when triggered (via right-click), then stop. This is an offline/discrete structural-editing layer, not a live performer or listener.

Max for Live (the older, more mature system) is the opposite: it can observe Live state continuously and react during playback, via the Live Object Model (`live.object`, `live.observer`, `live.path`, or the JS `LiveAPI` object) — but it's a different tool for a different job (real-time/continuous control, not batch structural edits).

**This is not a novel idea.** At least four public projects already wire Claude/LLMs into Ableton via MCP (ahujasid/ableton-mcp — 2.9k★ — being the most mature), all built on older, unofficial bridges (a hacked MIDI Remote Script + TCP socket, or OSC) that predate the Extensions SDK. The Extensions SDK is arguably a *better* fit for Ya Selva's use case specifically because it has native transactional undo, matching the "duplicate, don't destructively edit" requirement, and is officially supported rather than a reverse-engineered hack that can break on Live updates.

## The one fact that reframes everything: MIDI vs. audio

MIDI is symbolic data — a list of note events (pitch, start time, duration, velocity). That's why an LLM can reason over it: it's just structured data, readable the same way you'd read a spreadsheet or a piano roll.

**Audio is not.** A recorded guitar take, a vocal, a field recording — these are waveforms with no discrete "notes" for a model to read or generate. There is no audio perception in this pipeline at all: no frequency analysis, no timbre judgment, no listening. This is a hard, permanent boundary of the architecture, not a maturity gap that improves later.

Practical consequence: **this tool only ever sees the parts of a Ya Selva track that are programmed as MIDI.** The guitar and voice — the actual irreducible core of the project per [`00-master-vision.md`](00-master-vision.md) — are structurally invisible to it. A track built from live guitar and voice would show up as a blank in any arrangement-level analysis.

## What an agent could plausibly do (in ascending order of ambition)

### 1. Clip-level density variation (the v0.1 target)

Select one MIDI clip → "make this 50% sparser, preserve downbeats" → the agent reads the note list, generates a new note list, writes it as a **new duplicate clip** next to the original (transactional, non-destructive). Proves the whole loop: read state → reason → write structured edit → verify.

### 2. Multi-variation generation from one seed

Instead of one "correct" answer, generate 3–5 divergent derivations of a pattern (sparser-same-feel, half-time-feel, different accent placement, kick+hat only). Better fit for how creative variation actually gets used — you curate from a spread of options rather than receive one guess at what you wanted.

### 3. Climax → verse derivation

Take a complex, dense rhythmic idea (the climax) and derive a verse-scale idea from it. Honest limitation: this is not just "make it sparser" — a good verse usually has a different rhythmic *function* (different subdivision feel, cleared space for a vocal phrase, different accent placement), which is real music-theory reasoning at the edge of what's reliable from note data alone. Best framed as fast divergent-option generation for you to pick from, not composition-by-AI.

### 4. Whole-arrangement analysis (the most interesting, most speculative)

If an entire arrangement were programmed as MIDI, the SDK can walk every track/clip across the full timeline — effectively handing the model something close to a full score. That would make possible:
- a real structural map (where verse/chorus/breakdown boundaries actually fall, from density changes, not track names)
- a density/energy curve across the whole song
- register-collision checking (two parts fighting in the same pitch range at the same time — a rough proxy for arrangement clutter, not true frequency masking, which is audio/mix territory)
- contrast auditing ("your pre-chorus and chorus have near-identical rhythmic density, which may be why the drop doesn't land")

This is real and technically sound, but it's in direct tension with how Ya Selva actually gets made: the more "readable" an arrangement is to a model like this, the less it resembles the live-guitar-and-voice-first practice the project is actually built around.

## Fit with the actual workflow

The honest place this fits is **the MIDI-sketch phase, as scaffolding — not as the final material.** MIDI already shows up early: roughing out a beat or bassline idea before the real instrument goes in. That's low-stakes, disposable material — a reasonable place for an agent to work fast (generate variations in seconds instead of hand-editing) before the real performance (guitar, voice) gets layered on top and the MIDI sketch either gets replaced or recedes into the background.

It does **not** fit the live performance, the field-recording layer, or anything involving guitar/voice — those stay entirely outside what this tool can see or touch, by design of the MIDI/audio boundary, not by choice.

## What this is not

- Not AI music generation or audio AI in any general sense.
- Not real-time, not a bandmate, not something that can react while you're playing.
- Not able to "hear" — no frequency/timbre/mix judgment, ever, regardless of how the SDK matures.
- Not proven to produce musically competent output yet — reading MIDI accurately and generating musically good variations are different bars, and the second one is untested.

## Verdict

Worth prototyping the v0.1 loop (one clip, one sparser/downbeat-preserving variation, appearing as a duplicate) to find out whether the note-level output is actually good on real material — before investing further in variation-generation, climax→verse derivation, or arrangement-level analysis. Not premature as a technical matter (the tooling exists and prior art proves the loop works); the open question is purely whether the musical judgment holds up, which can only be answered by trying it.

## Related

- [`00-master-vision.md`](00-master-vision.md) — locked master vision
- [`18-the-field-studio-loop.md`](18-the-field-studio-loop.md) — field mode / studio mode / full-rig mode as one continuous loop
- [`10-one-rig-that-scales.md`](10-one-rig-that-scales.md) — the Ableton Move-first everyday rig
- [`16-ableton-ecosystem.md`](16-ableton-ecosystem.md) — Note → Move → Live/Push ecosystem
