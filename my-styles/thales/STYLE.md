# Thales — Style Prompt

> A vertical, voice-led explainer where **one line icon per phrase** is born, performs one small action and **turns into the next icon**, on a ground that flips between near-black and off-white, with a **word-by-word caption** in the middle of the frame and **one accent keyword** per idea. Everything is organic: springs with a little overshoot, shapes that inherit each other across cuts, rings and counters that keep breathing during holds.
> Reference (grammar only): a creator explainer reel (78 s, 9:16) analysed frame by frame at 30 fps, `reference-sheet.jpg` and `reference-morph-30fps.jpg`. Learn its grammar; never copy its icons, wording, layout of any specific card, or the creator's handle.

## 1. Essence, and what it is not

The film is a chain of **cards**, one per caption chunk (1–4 spoken words, ~1.5–2 s each). Each card = **one centred line icon doing one verb** + **the caption under it** + at most **one accent keyword**. The cut between cards is never a plain cut or crossfade: the outgoing card **hands an object to the next one** (morph), or **opens a door into it** (circle wipe, zoom-through, arc, shutter).

What makes a frame read as this style at a glance:
1. **One icon, centre stage, lots of air.** Flat, monoline or solid-fill pictograms (clapper, calendar, play button, coin, magnifier, toggle, waveform, filmstrip, eye, gear, lock, speech bubble), at most 30 % of the frame width, surrounded by empty ground.
2. **Shape continuity.** A line becomes a pill becomes a play button; a dot becomes a rounded square becomes a coin; a "+" becomes a framed photo. The eye never loses its anchor.
3. **Caption as a living line.** Small, semibold, centred, words lighting up one by one with the voice; one keyword gets an accent marker bar and, often, a huge condensed echo above it.
4. **Two-ground rhythm.** Near-black and off-white alternate every 1–3 cards, always through a transition that starts from a point on screen.
5. **Nothing is ever still.** During a hold something always ticks: a timecode rolls, rings ripple out, a waveform jitters, a cursor glides, a ghost glyph drifts.

Mood: editorial, calm-confident, "product launch for a person". Precise but warm, the opposite of stiff.

Not Swiss motion (it bounces, and shapes morph), not a keynote (no gradients, no glow, no device renders), not kinetic lyric typography (the caption stays small and still; only the keyword grows), not a UI screencast (real UI appears only as a short framed insert), not an infographic (numbers are props, not charts).

## 2. Materials & rendering

- **Canvas2D / SVG paths, 1080×1920, 30 fps.** Flat fills, round caps and joins. **Light film grain** (~3–4 %) and a very slight vignette added at mux time only.
- **Icon model.** Built from primitives that can interpolate into each other: line, capsule (pill), circle, rounded rect, triangle, arc. Two stroke weights per film: **icon stroke** (≈ 10–12 px at 1080 wide) and **hairline** (2 px) for rings, brackets, tick rulers and guides. Fills are solid ink or solid ground; at most one accent fill per card.
- **Every icon is a morphable spec**: `{shape, x, y, w, h, r(corner), rot, stroke, fill, glyph}`. A morph tweens these numbers; glyph details (play triangle, R$ letters, clapper stripes) scale in after the body settles.
- **Furniture (secondary layer, hairline, ~40–60 % opacity):** corner brackets (viewfinder), tick ruler under icons, mono labels (`CENA 01 · TAKE 1`, `TER · NOV 2026`, `BPM 132`, `1 CLIQUE = 00:00:15`), small pin labels on a leader line, progress bars with a dot head, dotted grids.
- **Ghost glyph:** a giant outline character or word (a "?", a word in display type) behind the icon at **6–10 % contrast**, scale 1.6–2.5× the icon, drifting slowly.
- **Real-media inserts** (screenshots, stills, video frames) only inside a **rounded frame card** with a hairline border, desaturated or B&W unless the brief says otherwise, max ~70 % width, tilted −3° to +3°.
- **Composition (Instagram-safe):** icon centre at **~40 % of frame height**; caption baseline at **~58–62 %**; display echo word between them; nothing important above 12 % or below 78 % (feed UI zones).

## 3. Colour logic

- **Four tokens only:** `GROUND_DARK` (near-black, e.g. #0E0E0E), `GROUND_LIGHT` (warm off-white, e.g. #F0EEEA), `INK` (the opposite of the current ground) and **one** `ACCENT`. Colours are swappable: take ground/ink/accent from the user's brand (Marcos: his "Marcos Saura" design system) at film start; the logic never changes.
- `ACCENT` is used for exactly three things: the keyword marker bar, the display echo word, and one state change (a toggle on, a bookmark fill, an arrow, a strike). Never for grounds, never for body icons at rest.
- On the dark ground icons are light-ink; on the light ground, dark-ink. Furniture uses INK at 40–60 %. Caption words not yet spoken don't exist yet; spoken words are 100 % INK.
- B&W for any photographic insert keeps the four-token logic intact.

## 4. Type & subtitles

- **Caption:** a neo-grotesk sans (Inter / Inter Tight / the brand sans), semibold, tight tracking (−1 %), **~44–48 px at 1080 wide**, centred, one line, 1–4 words. Always the narration verbatim, chunked on breath/phrase.
- **Display echo:** an ultra-condensed heavy sans (Anton / Bebas Neue / brand display), uppercase, **5–8× caption size**, ACCENT, sitting just above the caption. Used for the keyword of an idea (≈ once every 3–5 cards), never two on screen.
- **Mono furniture:** a monospace (JetBrains Mono / Space Mono), uppercase, 18–22 px, tracking +12 %.
- **Rule from the narration:** every word on screen is spoken (caption, echo, and any title or label that matters). Mono furniture is decoration and may stay unspoken only if it is a number/ID, never a phrase.
- Text never overflows its container (bubbles, buttons, cards). Bubbles, buttons and screens are never empty: dots, an icon or a word.

## 5. Motion quality

The signature. All values at 30 fps; "f" = frames.

**Global feel.** Springs, not linear. Entrances overshoot ~8–12 % and settle; exits accelerate out (ease-in). Default spring: stiffness ≈ 260, damping ≈ 20, mass 1 (≈ critically under-damped, one visible overshoot, settled in ~10 f). Default ease-out: `cubic-bezier(0.22, 1, 0.36, 1)`; ease-in for exits: `cubic-bezier(0.64, 0, 0.78, 0)`.

**Icon pop-in (the basic entrance).** scale 0 → 1.10 in **3 f**, 1.10 → 0.97 → 1.00 over the next **6–8 f**; rotation enters at ±4–8° and settles to 0° (or to a resting tilt of ±2° for cards/frames); opacity 0 → 1 in the first 2 f. Inner glyph details (play triangle, numbers, stripes) pop **3–4 f after** the body.

**Morph bridge (preferred transition between cards of the same ground).** The previous card's main shape tweens its spec into the next card's shape over **6–10 f** with ease-in-out, keeping its centre: line (hold) → thickens and shortens to a pill (6 f) → pill to circle (3 f) → glyph scales in (3 f). Colour and stroke can tween too. Everything else of the old card exits first (fade + 4 % scale-down, 4 f).

**Circle wipe (ground flip).** A circle of the next ground grows from a **focal point on screen** (the icon just used, a coin, a cursor tip, a "+") to cover the frame in **3 f** (ease-in, radius ∝ t²). The next card's first element is already growing at that point as the wipe completes.

**Zoom-through (ground flip into dark).** The camera scales into a solid-ink area of the current icon (a play button, an eye's pupil, a mono word) ×1 → ×20 in **6 f**, ease-in expo, with sub-frame motion blur on the last 3 f. When the ink fills the frame it *is* the new dark ground; the next element is born as a 6 px dot in the centre and grows (rounded square → circle → icon).

**Arc rise.** A giant circle of the next ground (diameter ~2.5× frame width) rises from below; its curved edge sweeps up through the frame in **8–10 f**. Use when the narration pivots ("Agora…", "Até agora…").

**Shutter split.** Two thick horizontal ink bars meet at centre and pull apart (or a single bar line splits) revealing the next ground, **6 f**.

**Collapse to dot.** All elements shrink toward the centre into one dot (6 f, ease-in), the dot holds 2 f, then the next card grows out of it.

**Caption.** Per word, on its spoken onset: opacity 0 → 1 and y +6 px → 0 in **4 f** (ease-out), tiny blur 2 px → 0. Words already spoken stay at 100 %. Chunk exit: whole line fades to 30 % in 3 f, then gone; 2–4 f of empty caption zone before the next chunk. The caption never moves sideways or scales.

**Keyword marker.** On the keyword's onset an ACCENT bar (rounded 6 px, padding 6/10 px) **wipes left → right behind the word in 4 f**; the word switches to dark ink on it. Exit: bar fades to ~35 % (a pale stain) in 3 f, then leaves with the line.

**Display echo build.** The big condensed word assembles **letter by letter, 1 f stagger**; each letter enters squashed (scaleX 0.3 or scaleY 0.4) with a short trailing dash and springs to full width with overshoot. Whole word settles in ~10–12 f. Exit: scale up 1 → 1.4 while fading in 5 f, or becomes the door of a zoom-through.

**Rings (accent pulse).** On a click, a keyword or a beat: 2–3 hairline rings from the icon's radius to 2.5× radius, opacity 0.5 → 0, **15 f** each, staggered 3 f, ease-out.

**Cursor.** Arrow cursor glides in on a slight arc (ease-in-out, 10–14 f), pauses 3 f, clicks: cursor scale 1 → 0.85 → 1 (4 f) + rings + the target changes state (toggle slides 5 f with spring, button fills ACCENT, bookmark fills from bottom).

**Hold life (never static > 2 s).** While a card holds, at least one of: a mono timecode/counter rolling every frame; a waveform's bars jittering (random heights, 2-frame steps); a ring breathing (scale 1 → 1.04, 30 f period); the ghost glyph drifting (+3 % scale over the card); a slow tilt ±1°.

**Groups & particles (use 1–2 per film section):**
- *Grid fill:* cells pop in row by row (2 f stagger), then inner dots pop (1 f stagger).
- *Gravity drop:* elements fall out of their cells with gravity (g ≈ 2.5 px/f²), small random x drift and spin, staggered 1 f, off-screen in ~18 f.
- *Toss and snap:* cards fly in from below with random rotation (±25°) and land scattered (spring), then snap into a straight row on a timeline (2 f stagger) when the narration says "order".
- *Radial reveal:* a dot field appears from the centre outward (delay ∝ distance, ~12 f total); dots can then morph into tiny tiles.
- *Burst:* many small pills/labels spawn around a central button (radial, 1 f stagger, drifting outward 4 % over the hold).
- *Line draw:* paths draw with dash-offset in 10–12 f (ease-out); a dashed accent line can draw over a solid ink one for contrast.
- *Coin flip:* rotateY faked with scaleX = cos(θ), 12 f per half turn.
- *Strike:* two ACCENT lines draw diagonally across a card (4 f each), the card tilts 6° and drops 20 px.
- *Cage / lock:* vertical bars drop from the top edge (1 f stagger, spring), a lock icon drops with a bounce.

## 6. Camera grammar

- **Locked, centred frame** for every card; the composition is the movement. No pans, no handheld.
- The only camera moves are **zoom-throughs** (into an ink area) and a rare **push** (scale 1 → 1.08 over a card) on an important line.
- Frames and screenshots may sit at a small tilt; the camera itself never rotates.

## 7. Sound palette

- **Voice first**, informative and calm (Marcos: ElevenLabs v4, voice "The Witcher"). Narration silences trimmed tight; the visual pace follows the voice, not a fixed beat.
- **One short, satisfying SFX per event** (pop, morph, wipe, click, toggle, ring, letter build, drop), subtle chiptune/UI-tick character, **well under the voice** (≈ −18 to −24 dB relative), never cartoonish. Generated with ElevenLabs.
- Wipes and zoom-throughs get a short whoosh (≤ 8 f); grid fills and letter builds get a soft rapid tick run; gravity drops get tiny clicks.
- Music optional and only if asked; if used, a quiet minimal beat ducked under the voice.
- Mix: −14 LUFS, ≤ −1 dBTP.

## 8. Native moves

- **Object relay.** The icon of one idea becomes the icon of the next (line → play button; coin → question mark). *Fits:* cause → effect chains; "before / after".
- **Counter under the icon.** A mono timecode or number rolls while the voice speaks a duration, price or quantity, then lands. *Fits:* time saved, cost, followers.
- **Keyword slam.** Caption keyword marked in ACCENT + giant condensed echo built letter by letter. *Fits:* the one word the viewer should remember per idea.
- **Click to change.** Cursor arrives, clicks, a toggle/button/bookmark changes state with rings. *Fits:* calls to action, "one click", settings.
- **Scatter to order.** Pieces tossed messy, then snapped into a row on a timeline. *Fits:* editing, organising, "someone has to decide".
- **Grid count.** A grid of cells fills to show a quantity, then items drop out to show loss. *Fits:* projects per month → per year.
- **Framed proof.** A real screenshot/still slides in as a tilted rounded card with a mono caption label. *Fits:* product launches, news, examples.
- **Viewfinder focus.** Corner brackets snap in around an icon to say "look at this". *Fits:* details, detection, "the eye of the director".

## 9. Pitfalls of the medium

- Stiff motion: linear tweens or no overshoot kill the style → every entrance uses the spring, every exit the ease-in.
- Two things moving for attention at once → one focus per card; furniture moves only in hold-life mode (small, slow).
- Plain crossfades between cards → always a morph or a point-origin transition.
- Caption drifting or bouncing → the caption is calm; only the keyword marker and the echo are energetic.
- Accent overuse → one accent role per card at most.
- Text overflow in bubbles/buttons, or empty containers → measure text and size containers to it; never leave them blank.
- Captions near top/bottom → keep the 58–62 % caption band (Instagram safe area).
- Morphs between unlike primitives looking messy → morph through a capsule or circle as the intermediate shape.
- Gravity drops crossing the caption → render particles in their own layer masked above the caption band.

## 10. Engine

To be built in the demo (`demo/`): `engine.js` with the four colour tokens, a `Shape` spec + `morph(a, b, t)`, `spring()` and bezier easings, `popIn`, `circleWipe(origin)`, `zoomThrough(target)`, `arcRise`, `shutter`, `collapseToDot`, `caption(words, timings)` with per-word onsets from the TTS alignment, `keywordMarker`, `echoBuild`, `rings`, `cursor`, `counter`, `waveform`, particle helpers; `film.js` with the card list (from the script + word timings) and its SFX events; `mix.py` to place SFX on the same events under the voice. Technique base: Pictogram Motion (pictograms, masked type); borrows Swiss Motion (grid furniture, single signal colour).

## 11. Variation space

You decide the topic, the icons, which transitions, the keyword list, the furniture labels and the palette (from the brand).

- Structures: **a question and its answer** (hook question, problem cards, a pivot "Agora…", solution cards, CTA); **a list** ("3 erros…", each item opened by a mono `#1 / #2` label); **a timeline** (a date or counter recurring as the spine).
- Openings: **a dot that becomes everything** (first frame is one dot that grows into the first icon); **the counter already running** (a timecode rolling under the first icon); **the keyword first** (the display echo slams in before the caption).
- Endings: **CTA button** (cursor clicks a "Seguir"/"Salvar" button, it fills ACCENT, rings); **collapse to dot** (everything returns to one dot, then black); **the first icon returns** (the opening icon comes back with a new state).
