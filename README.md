# focusTones

Independent left/right ear frequency player (dichotic tones / binaural deltas) built with the Web Audio API.

Spun out of [forgeFit](https://github.com/chris-712interactive/forgeFit) as a standalone static app.

## Run locally

Open `index.html` in a desktop browser, or serve the folder:

```bash
npx --yes serve .
```

Then open the printed URL with **stereo headphones**.

## Features

- Independent left / right oscillators (20–1500 Hz)
- Live |L−R| beat-difference readout
- Per-ear mute + master volume
- Waveform select (sine, triangle, square, sawtooth)
- Presets: unison, binaural Δ4 / Δ10, wide dichotic
- Soft start/stop gain ramps

## Notes

- Headphones (or earbuds) are required — speakers mix both channels.
- Browsers require a user gesture before audio starts (tap Play).
- Not a medical device; keep volume low.

## Files

| Path | Role |
|------|------|
| `index.html` | App shell |
| `app.js` | `DichoticToneEngine` + UI wiring |
| `styles.css` | UI |
| `docs/exploration.md` | Feasibility notes from the forgeFit spin-out |
