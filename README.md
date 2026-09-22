# The Ant and the Melon

A 45-second hand-drawn short with an original score, built in a single file: `index.html`
(canvas + inline CSS/JS, music by [Tone.js](https://tonejs.github.io/) from cdnjs).

Open `index.html` in a browser and tap **Play**. It needs a network connection once to load
Tone.js (cdnjs, with a jsDelivr fallback); without it the film still plays and a message says the
music didn't load.

No sound? Check the volume. On iPhone/iPad the page asks for a "playback" audio session, so
Silent mode shouldn't mute it. If the browser still holds sound back, a **Turn sound on** button
appears and the music comes in at the current moment.

## Controls

| Control | What it does |
| --- | --- |
| Play / Pause | Start or pause the film (the first tap also unlocks audio). |
| Restart | Start again from 0:00. |
| Record | Plays the film from the start and records the canvas + music with `MediaRecorder`. The video file downloads when the film finishes, or when you press Stop. Chrome/Edge/Safari give `.mp4`, Firefox gives `.webm`. Keep the tab in front while recording. |
| Scene bar | Tap a scene (Walk, Tap, Creep, Split, Feast) to jump to it. |
| Keyboard | `Space` play/pause · `R` restart · `1`–`5` jump to a scene. |

## Changing the timing

Everything is driven by the `SCENES` array at the top of the script:

```js
{ id: 'split', label: 'Split', start: 30, end: 38,
  cues: { backAway: 0.08, pop: 0.50, land: 0.625, grab: 0.90 } },  // pop = 34.0s
```

- `start` / `end` are seconds.
- `cues` are moments inside a scene, as a fraction of the scene's length (0 = start, 1 = end).
  Both the animation and the music read them. For example, the melon splits and the pop
  sounds at `split.pop`, and the crack appears on the third tap at `tap.tap3`.
- The music inside each scene is written in beats (`MUSIC_BEATS`) and stretches with the scene,
  so phrases keep landing on scene boundaries.

`CONFIG` holds the rest: boil rate (8 fps), number of hatch passes, the end-fade colour
(`'#f1e6cf'` paper, or `'#000000'` for black), and the recording frame rate and bitrate.

For checking a single moment, `index.html?t=34.1` renders that frame as a still.

## How it's drawn

- **Hatching:** every shape is filled by walking a jittered grid of short, slightly angled
  strokes. Colour comes from a lighting and stripe field (the melon is treated as an
  ellipsoid), with cross-hatching for shadows and cross-contour strokes that follow the
  stripes around the "planet".
- **Line boil:** each background layer is rendered as several independent random hatch passes.
  The film switches between them 8 times a second. The ant, crack, juice and shadows are
  re-randomised live on the same 8 fps clock.
- **The ant:** a rigged body (head, thorax, petiole, gaster) with two-bone IK legs. The tripod
  gait is computed from distance travelled, so feet stay planted on the curved rind and can
  walk backwards. Poses (tap, peer, back away, tumble, sit and nibble) are keyframe tracks
  keyed to the scene cues.
- **Sync:** the music is scheduled on the Tone.js Transport from the same cues, and the
  animation clock is read from the audio clock.
