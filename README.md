# Hand-drawn shorts

Two short films drawn in coloured pencil on the fly, each with an original score and each
built as a single HTML file (canvas + inline CSS/JS, music by [Tone.js](https://tonejs.github.io/) from cdnjs):

- `index.html`: **The Ant and the Melon** (45 seconds)
- `american-revolution.html`: **The American Revolution**, 1765 to 1783 (80 seconds)

## The Ant and the Melon

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

## The American Revolution

`american-revolution.html` is a second film in the same pencil style: an 80-second history
of the Revolution from 1765 to 1783, in seven scenes with a fife-and-drum score. Open it the
same way. Play, Restart, Record and `?t=` stills work as above; the scene bar is labelled by
year, and the keys `1`–`7` jump to a scene.

| Scene | Seconds | What happens |
| --- | --- | --- |
| 1765 · The Stamp Act | 0–11 | A pen draws the map of the thirteen colonies. The names pop on in time with the tune, then a tax stamp slams down. |
| 1773 · Boston | 11–22 | By lantern light, colonists heave tea chests over a ship's rail into the harbor. |
| 1775 · Lexington & Concord | 22–33 | Redcoats march in, halt, make ready, present and fire. Militia behind a stone wall fire back. |
| July 1776 · Philadelphia | 33–45 | A quill writes the Declaration of Independence, ending with John Hancock's signature. |
| December 1776 · Delaware River | 45–57 | Washington crosses the icy river at night, with the rowers pulling to the beat of a drum. |
| 1781 · Yorktown | 57–69 | To "Yankee Doodle", British soldiers throw down their muskets between the American and French lines. |
| 1783 · Paris | 69–80 | Peace: the new flag over the farmland, then a slow fade. |

Each scene has a dated caption card. The timing lives in the same kind of `SCENES` array at
the top of the script, with cues such as `lexington.fire` for the volley and `colonies.stamp`
for the stamp. The music is written in beats per scene (`MUSIC_BEATS`), and every sound
effect is placed from those same cues.

The first scene is drawn before the Play button appears. The other scenes finish drawing in
the background, a slice per frame. Press **Record** and anything still unfinished is completed
before recording starts. The page loads IM Fell English and Pinyon Script from Google Fonts for the
lettering and falls back to local serif and script faces offline.

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
- **The soldiers:** in the Revolution film each figure is a rigged profile (hat, head, coat,
  waistcoat, breeches, gaiters, musket) with two-bone IK arms and legs. It walks with the same
  distance-driven stepping as the ant, and the musket drills (shoulder, make ready, present,
  fire, throw down) are poses blended on the scene cues. Parts are filled with flat colour,
  then pencil-texture tiles and cross-hatched shadows, so they match the hatched backgrounds.
- **Sync:** the music is scheduled on the Tone.js Transport from the same cues, and the
  animation clock is read from the audio clock.
