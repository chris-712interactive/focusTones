# Exploration — Dichotic / Per-Ear Frequency Player

> **Status:** Feasibility confirmed · shipped as this focusTones app  
> **Origin:** Spun out of forgeFit `prototypes/dichotic-tones/` (2026-07-31)  
> **Scope:** Standalone static Web Audio app — no backend

---

## Verdict

**Yes — this is straightforward to build.** The browser Web Audio API can generate independent oscillators routed to left and right channels, with live frequency control per ear. Headphones (or earbuds) are required for the effect to work; speakers mix both channels into the room.

---

## What “different frequency per ear” means

| Mode | Left ear | Right ear | Perceived effect |
|------|----------|-----------|------------------|
| **Dichotic tones** | Any Hz | Any Hz | Two distinct pitches (independent control) |
| **Binaural beats** | Carrier Hz | Carrier ± Δ Hz | Brain interprets \|L−R\| as a low “beat” (typically 1–30 Hz) |
| **Isochronic / monaural** | Same signal both ears | Same | Pulsing amplitude; no stereo split needed |

This app targets **dichotic tones with independent per-ear frequency** (which also covers binaural-beat use by setting a small Δ).

---

## Technical approach

### Signal graph

```
[Oscillator L] → [Gain L] → ChannelMerger(0) ─┐
                                               ├→ [Master Gain] → destination
[Oscillator R] → [Gain R] → ChannelMerger(1) ─┘
```

- `OscillatorNode.frequency` is an `AudioParam` — set or ramp independently per ear.
- `ChannelMergerNode` assigns left → output channel 0, right → channel 1.
- Soft start/stop uses `linearRampToValueAtTime` on master gain to avoid clicks.

### Core API surface

| Need | API |
|------|-----|
| Tone generation | `OscillatorNode` (sine / square / sawtooth / triangle) |
| Per-ear routing | `ChannelMergerNode` |
| Volume | `GainNode` |
| Smooth frequency changes | `frequency.setTargetAtTime` |
| User gesture gate | `AudioContext.resume()` on first Play click |

---

## Constraints & UX

1. **Headphones required** — stereo speakers collapse the effect.
2. **User gesture to start** — browsers block audio until click/tap.
3. **Safe defaults** — quiet master volume (~18%); warn via copy, not therapy claims.
4. **Frequency range** — prototype allows 20–1500 Hz; binaural carriers often sit ~100–600 Hz.
5. **No medical claims** — treat as a tone tool, not therapy.

---

## What’s next

1. Validate L/R independence with headphones (mute one ear at a time).
2. Optional: PWA install, session timer, favorite L/R pairs in `localStorage`.
3. Optional: native wrap later if background playback matters.
