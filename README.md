# Hand-drawn films

Two films drawn in coloured pencil on the fly, each with an original score and each
built as a single HTML file (canvas + inline CSS/JS, music by [Tone.js](https://tonejs.github.io/) from cdnjs):

- `index.html`: **The Ant and the Melon** (45 seconds)
- `american-revolution.html`: **The American Revolution**, 1763 to 1789 (10 minutes)

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

`american-revolution.html` is a ten-minute film in the same pencil style: the Revolution
from 1763 to 1789 in 31 scenes. Each scene has its own drawing, animation, dated caption
and music. Open it the same way; Play, Restart, Record and `?t=` stills (for example
`american-revolution.html?t=446`) work as above.

| Control | What it does |
| --- | --- |
| ‹ › | Previous / next scene. The date and name of the scene you are in show between the arrows. |
| Timeline | The strip under the arrows has a mark for every scene; tap one to jump there. |
| Record | Records the full ten minutes, picture and music, as in the ant film. The opening scene is drawn in full before recording starts; the rest are drawn ahead while it runs. Keep the tab in front until it finishes. |
| Keyboard | `Space` play/pause · `R` restart · `←` `→` previous / next scene. |

| # | Scene | Starts | What happens |
| --- | --- | --- | --- |
| 1 | 1763 · British North America | 0:00 | A pen draws the coast and the thirteen colonies on an old map; the Proclamation Line runs down the mountains and a British ship sails in. |
| 2 | 1765 · The Stamp Act | 0:22 | Seen from above: the tax stamp comes down on a newspaper, playing cards and a deed, then a huge one; a slogan is scrawled across them and the papers burn. |
| 3 | 1767 · The Townshend Acts | 0:42 | A colonial kitchen: a woman spins at a great wheel, another knits by the window, a crate of taxed British goods sits nailed shut. |
| 4 | 1770 · The Boston Massacre | 1:00 | King Street at night in the snow: snowballs fly, the soldiers fire, the crowd scatters, and the picture becomes Revere's engraving. |
| 5 | 1773 · The Boston Tea Party | 1:22 | By lantern light, colonists heave tea chests over a ship's rail into the harbor. |
| 6 | 1774 · The Intolerable Acts | 1:46 | Boston Harbor closed: warships at anchor, idle merchantmen, the order posted on the wharf and a sentry pacing. |
| 7 | 1774 · The First Continental Congress | 2:02 | Carpenters' Hall: delegates at cloth-covered tables and Patrick Henry on his feet. |
| 8 | 1775 · Paul Revere's Ride | 2:18 | Two lanterns in the Old North Church, then Revere at a gallop down a moonlit road. |
| 9 | 1775 · Lexington | 2:42 | At dawn on the town green, redcoats and militia exchange fire. |
| 10 | 1775 · Concord | 3:04 | At the North Bridge the militia fire back and the regulars fall back toward the town. |
| 11 | 1775 · Bunker Hill | 3:22 | The British climb Breed's Hill three times while Charlestown burns across the water. |
| 12 | 1775 · Washington Takes Command | 3:48 | Under the elm on Cambridge Common, Washington draws his sword before a ragged army. |
| 13 | 1776 · Common Sense | 4:06 | In a print shop the press comes down on COMMON SENSE, and copies fly out in their thousands. |
| 14 | 1776 · The Declaration | 4:24 | A quill writes the Declaration of Independence, ending with John Hancock's signature. |
| 15 | 1776 · Down Comes the King | 4:48 | At sunset on Bowling Green a crowd pulls the gilded statue of George III off its pedestal. |
| 16 | 1776 · Escape from Long Island | 5:04 | Boats ferry the beaten army across the East River by night, hidden by fog. |
| 17 | 1776 · Crossing the Delaware | 5:20 | Washington crosses the icy river in a snowstorm, the rowers pulling to the drum. |
| 18 | 1776 · Trenton | 5:38 | At dawn the Americans charge down King Street and the Hessians throw down their arms. |
| 19 | 1777 · Saratoga | 5:52 | Burgoyne hands his sword to Gates, who gives it back. |
| 20 | 1777–78 · Valley Forge | 6:14 | Log huts in the snow; Baron von Steuben drills a squad until it presents arms as one. |
| 21 | 1778 · The French Alliance | 6:38 | In a gilded Paris salon Franklin signs the alliance and the two flags come together. |
| 22 | 1778 · Monmouth | 6:56 | In fierce heat a gunner collapses by his gun; "Molly Pitcher" drops her water pitcher, takes up his rammer, loads, and the gun fires again. |
| 23 | 1779 · John Paul Jones | 7:14 | By moonlight the Bonhomme Richard and HMS Serapis lie lashed together; the Richard's rigging burns, Jones raises his sword ("I have not yet begun to fight!"), a grenade finds the Serapis's powder, and her ensign comes down. |
| 24 | 1780 · The Swamp Fox | 7:38 | Francis Marion's men glide through a misty cypress swamp by dugout, stop to listen, start a heron, and vanish into the fog. |
| 25 | 1781 · The Siege of Yorktown | 7:54 | At night Washington touches off the first American gun, a French mortar crew works beside it, and shells arc like comets onto the burning town. |
| 26 | 1781 · Surrender at Yorktown | 8:16 | To "Yankee Doodle", British soldiers throw down their muskets between the American and French lines. |
| 27 | 1783 · The Treaty of Paris | 8:40 | By candlelight the closing line of the treaty is written, the four commissioners sign, and each seal is pressed into red wax. |
| 28 | 1783 · Washington Resigns | 8:56 | In the Annapolis senate chamber Washington hands his commission back to Congress, bows, and leaves while the gallery waves. |
| 29 | 1787 · The Constitution | 9:12 | "We the People" is written large, then the Preamble and the signatures, as morning sun crosses the page for Franklin's "rising sun". |
| 30 | 1789 · The First President | 9:32 | On the Federal Hall balcony Washington takes the oath; the crowd in Wall Street cheers, thirteen guns fire and the bells ring. |
| 31 | The End | 9:48 | The flag over the farmland at sunrise, "The United States of America", "The End", and a slow fade. |

The timing lives in the `SCENE_LIST` array at the top of the script: each row is
`[id, label, seconds, cues]`, and the scenes follow one another in that order, so changing a
length moves everything after it. Cues are fractions of their scene, read by both the
animation and the music (for example `jones.quote` for the ribbon and the brass call, or
`inauguration.cheer` for the crowd). `MUSIC_BEATS` sets how many beats of music each scene
has, and `CAPTIONS` holds the caption cards.

Ten minutes of drawing is too much to hold at once, so the film draws as it goes. The first
scene is drawn before the Play button appears; after that, each scene is drawn in the
background a slice per frame, two scenes ahead of where you are, and scenes already passed
are let go. If you jump to a scene that isn't drawn yet, it is finished first, which takes
a second or so. The page loads IM Fell English and Pinyon Script from Google Fonts for the
lettering and falls back to local serif and script faces offline.

## How it's drawn

- **Hatching:** every shape is filled by walking a jittered grid of short, slightly curved
  strokes. Colour comes from a lighting and tone field, with cross-hatching for shadows.
  Stroke direction comes from a flow field chosen for each surface, so marks run the way
  the thing is made: bark up a cypress, rings round a moon or sun, level strokes on still
  water, grain along a floorboard, weave across a wicker basket. Some surfaces are
  stippled or scribbled instead of hatched (baize, felt, foliage, smoke), and night skies
  sit on a dark ground so no paper glints between the strokes.
- **Line boil:** each background layer is rendered as several independent random hatch passes.
  The film switches between them 8 times a second. Figures, flames, smoke and ink are
  re-randomised live on the same 8 fps clock.
- **The ant:** a rigged body (head, thorax, petiole, gaster) with two-bone IK legs. The tripod
  gait is computed from distance travelled, so feet stay planted on the curved rind and can
  walk backwards. Poses (tap, peer, back away, tumble, sit and nibble) are keyframe tracks
  keyed to the scene cues.
- **The people:** in the Revolution film each figure is a rigged profile (hat, head, coat,
  waistcoat, breeches or skirts, gaiters) with two-bone IK arms and legs, in dozens of
  outfits. They walk and run with distance-driven stepping, sit, row, paddle, ride, drill
  with muskets, and handle rammers, swords, quills and flags; each part is shaded with
  strokes that follow it. Horses have their own rig and gaits, and ships, guns, a mortar
  and a dugout are built from the same pencil marks.
- **Sync:** the music is scheduled on the Tone.js Transport from the same cues, and the
  animation clock is read from the audio clock.
