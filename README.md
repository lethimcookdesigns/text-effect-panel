# Text Effect Panel

Ten interactive text effects on one page, each with its own live control panel.
Type your own text, tweak any effect, and download it as a ready-to-use
`index.html` + `style.css` + `script.js` bundle with your settings baked in.

By Let Him Cook Designs

## Effects

| # | Effect | Built with | Based on |
|---|--------|-----------|----------|
| 01 | Pixel Scan | JS · GLSL | [fand — Pixel Scan effect with VFX-JS](https://codepen.io/fand/pen/MWMBdbj) |
| 02 | Bump | JS · GLSL | Original — lens magnify from the Let Him Cook landing page |
| 03 | Spark Text | JS · CSS | [42EG4M1 — Spark Text](https://codepen.io/42EG4M1/pen/QjoYgG) |
| 04 | Smoky Text | JS · CSS | [Bennett Feely — Smoky Text](https://codepen.io/bennettfeely/pen/nZXRyJ) |
| 05 | Entrance Reel | JS · CSS | [Creative Ocean — GSAP Custom Bounce](https://codepen.io/creativeocean/pen/GRMrQpq) and [jh3y — Cascading CSS Text Effects](https://codepen.io/jh3y/pen/gOeGmRN) |
| 06 | Erode | JS · GLSL | [fand — VFX-JS erode](https://codepen.io/fand/pen/xbKBYqz) |
| 07 | Stroke Draw | JS | [l3dlp — GSAP DrawSVG Stroke Animation](https://codepen.io/l3dlp/pen/yLZazzb) |
| 08 | Gooey Marquee | CSS · JS | [Amit Sheen — A Gooey Marquee](https://codepen.io/amit_sheen/pen/wvORNYm) |
| 09 | Jello | JS · CSS | [Pete Barr — Jello Stretchy Variable Font](https://codepen.io/petebarr/pen/LYzBoeg) |
| 10 | Shard Wind | JS · GLSL | [tomncurry — Particle Text Effect](https://codepen.io/tomncurry/pen/wXKReQ) |

Every effect was rebuilt from scratch to run on live text in any font, on one
shared clock (so each can be scrubbed on its timeline), with Hover / Click /
Auto-loop triggers. No libraries — just one HTML file. Public CodePen pens are
MIT-licensed; credit to the original authors above.

## Files

```
index.html     the whole app (HTML, CSS and JS in one file, fonts from Google Fonts)
favicon.svg    tab icon
vercel.json    clean URLs + basic security headers
```

## Run locally

Open `index.html` in a browser. That's it — no build step.

## Deploy (Vercel)

1. Push this folder to a GitHub repo.
2. In Vercel: **Add New → Project → Import** the repo.
3. Framework preset: **Other**. Leave build command and output directory empty.
4. Deploy. Every push to `main` redeploys automatically.

To serve it under your portfolio domain at `/text-effects`, add a rewrite in
the portfolio project's `vercel.json`:

```json
{ "rewrites": [
  { "source": "/text-effects", "destination": "https://YOUR-PROJECT.vercel.app/" }
] }
```

## Editing

- **Change an effect's defaults:** in the effect's panel, set the controls and
  click **Copy values**; each value maps to a `def:` in that effect's `groups`
  list inside `index.html`.
- **Reorder effects:** edit the `EFFECTS` array near the end of the script.
- **Shared text / font / size / color defaults:** the `G` object at the top of the script.
- **Downloads:** each effect's code lives between `//@region` markers; the
  Download button packages only the regions that effect needs.
