# SHIPCAT

A small arcade game in a single HTML file. An orange cat hops between checkboxes and ticks them before their outlines burn out. Chain ticks for a multiplier, grab gold boxes for bonus points, and survive as long as you can.

**Community:** [Join the X community](https://x.com/i/communities/1974725158540574780)  
**CA:** 4vFXRCwsWZZm3md2VYJY5iA3FawDiGWArMkxcsrV3wmM

## Play

Open `index.html` in any modern browser, or host it anywhere that serves static files (see [Deploy](#deploy)).

- **Tap or click a box** and the cat hops over to tick it.
- You can queue up to two hops ahead while the cat is in the air.
- Each box has an outline that burns down. If it runs out before the cat arrives, the box is missed.
- Three misses end the run.

### Controls

| Input | Action |
| --- | --- |
| Tap / click a box | Send the cat to it |
| Space or Enter | Start, or play again from the end screen |
| P or Esc | Pause and resume |
| M | Mute and unmute |

The game also pauses on its own when the tab is hidden or loses focus.

### Scoring

- A normal box is worth 1 point. A gold box is worth 3.
- Ticking within 2.2 seconds of the previous tick keeps your combo alive.
- The multiplier is `1 + floor(combo / 5)`, capped at ×5.
- Gold boxes start appearing after about 8 seconds of play.

### Difficulty

Difficulty rises with time survived:

| Value | Formula |
| --- | --- |
| Time between spawns | `max(0.5, 1.1 - 0.009 * t)` seconds |
| Life of a box | `max(1.6, 4.4 - 0.028 * t)` seconds |
| Boxes on screen at once | `min(8, 4 + floor(t / 15))` |

## Tech

- One file: HTML, CSS and vanilla JavaScript. No framework, no build step, no dependencies.
- Rendering is a single `<canvas>`, scaled for device pixel ratio and resized with the window.
- The cat is an inline SVG drawn to the canvas as a sprite.
- Sound is synthesised with the Web Audio API, so there are no audio files.
- Fonts (Dela Gothic One and Familjen Grotesk) load from Google Fonts, with system fallbacks if they are unavailable.
- Best score and the mute setting are saved in `localStorage` under `tickcat.best` and `tickcat.mute`. Scores stay on the player's device.

## Run locally

Open the file directly, or serve the folder:

```bash
npx serve .
# or
python3 -m http.server 8080
```

## Deploy

Any static host works. Upload `index.html` as the site root.

**Vercel**

```bash
npm i -g vercel
vercel --prod
```

Choose the "Other" framework preset, leave the build command empty and set the output directory to `.`.

**Netlify**: drag the folder onto <https://app.netlify.com/drop>.

**GitHub Pages**: push the file to a repository, then set Pages to serve from the main branch in the repository settings.

## Customise

Everything lives in `index.html`.

- **Name:** change the `<title>`, the two `og:` meta tags, the `<h1>` on the title screen and the canvas `aria-label`.
- **Colours:** edit the tokens at the top of the `<style>` block (`--bg`, `--ink`, `--orange`, `--orange-deep`, `--dim`). The canvas reads the same tokens, so one edit restyles the whole game.
- **Difficulty and scoring:** edit `difficulty()`, `multOf()` and `COMBO_WINDOW` in the script.
- **Lives:** change the starting value of `lives` in `startGame()`, and the number of icons drawn in `hud()`.
- **Contract address:** the title screen shows a "CA coming soon" chip. Replace the `<span class="chip ca">` in the `.links` block with the real address (and a link to the explorer) once it is live.
- **Share preview:** add an `og:image` meta tag pointing to a PNG of the banner so links preview with art on X and Telegram.

## Files

```
index.html   the whole game
README.md    this file
```

## Roadmap ideas

- Shared leaderboard (small serverless function with a Redis store).
- More box types, such as moving boxes or freeze boxes.
- Daily seeded run so everyone plays the same layout.

## Credits

Game, cat and logo artwork made with Claude.
