# Text Effect Panel — handoff

Single-file web app: 10 interactive text effects, each with a live control
panel, per-effect text, timeline scrubber, Copy values and a Download (.zip)
button. No build step, no libraries. Owner: Rob ("Bobby"), Let Him Cook Designs.

## Where things live

| What | Where |
|---|---|
| Source of truth | GitHub `lethimcookdesigns/text-effect-panel` (public), branch `main` |
| Vercel project | `text-effect-panel` → https://text-effect-panel.vercel.app (auto-deploys on push) |
| Public URL | https://lethimcook.design/text-effects — served by a rewrite in repo `lethimcookdesigns/lethimcook.design.site` (`next.config.ts`, private Next.js starter = Vercel project `lethimcook.design.site`, owns the domain) |
| Local copy | `~/Desktop/Lethimcook/text-effect-panel/` on Rob's Mac (may lag the repo) |
| Claude artifact | claude.ai/artifact/KKRfn26shkofsMtsXq1hSk (older mirror; repo is canonical) |
| Vercel team | "Bobby's projects" (`team_CLAp6f7lTLDRpPNcPHHLAhid`, Hobby) |

## Deploy rules (learned the hard way)

- Commit as `lethimcookdesigns <bobby@lethimcook.design>` (repo-local git config).
  Vercel Hobby **blocks** deploys of private repos when the commit author email
  isn't on the GitHub account. Fallback that is known to work:
  `265308637+lethimcookdesigns@users.noreply.github.com`.
- Pushing to `main` of this repo redeploys the panel; nothing to do in the site repo.
- The Vercel MCP connector can't create/list projects for this team — do Vercel UI steps via Rob.
- Keep the page free of relative asset URLs (it's served under `/text-effects`); favicon is an inline data URI.

## File map — `index.html` (~2.8k lines; everything inline)

- `<style>`: page UI + effect CSS. Effect CSS sits between `/*@region NAME*/ … /*@endregion*/`
  (`stage, letters, spark, smoky, reel, gooey, jello, stroke`) — Download copies these.
- `<script>` (IIFE), split by `//@region NAME … //@endregion` (Download copies these too):
  - `core` — `FONTS` (Google Fonts; `gf` = family spec for exports), `G` shared text/font/size/color, `DPR`, `clamp01`, `hexToRgb`, `textOf(row)`, `VERT`
  - `pixel-scan` (01 Pixel Scan, WebGL frag), `bump` (02 Bump, own WebGL lens), `spark` (03), `smoky` (04), `reel` (05 Entrance Reel: Tuna Drop, Cascade, Swapsies, Glitch, Reflective, Bubbling), `erode` (06, WebGL frag), `stroke` (07 Stroke Draw, glyph outline tracing on 2D canvas), `gooey` (08 Gooey Marquee), `jello` (09, drag-to-stretch, Roboto Flex / Anybody axes), `shards` (10 Shard Wind, WebGL triangles), `letters` (shared per-letter DOM helpers + `cssEase`)
  - `const EFFECTS = [...]` — **panel order** (outside regions)
  - `engine` — `rasterText()`, `el()`, `class FxRow` (builds stage + panel, clock/timeline, triggers, hover/click hooks, GL for `frag` effects, headless mode for exports)
  - mount (page only): global bar wiring, `EXPORTS` table (effect id → JS/CSS regions), `buildExport()`, `makeZip()` (store-only zip), `downloadEffect()` (uses `claude.use('downloads')` inside Claude, `<a download>` elsewhere)

## Effect definition shape

```js
const Name = {
  id: 'kebab-id', name: 'Shown Name', langs: ['JS', 'GLSL'],
  kind: 'dom',            // omit for a fullscreen-fragment GL effect (then: frag, uniforms, stateAt, apply)
  groups: [{ label: 'Motion', show: p => …optional, controls: [
    { key, label, type: 'range'|'segment'|'toggle'|'color', min, max, step, def, unit, options, grid } ] }],
  total(p,row), restFrame(p), describe(p), hint?(p),
  // dom kind: setup(row) → row.fx, layout(row), render(row, clock), onParam(row,key), hitRect(row)
  // optional: hoverHold + holdAt/releaseAt/captureRelease, onHoverEnter/Leave, onStageClick, onCycle, alwaysWrap
};
```
Triggers are a `trigger` control with values `hover` / `click` / `loop` (Jello also `drag`).

## Common tasks

- **Change defaults:** Rob sets sliders → clicks Copy values → pastes JSON. Update each matching `def:` inside that effect's region (ignore `effect/text/font/size/color`). Only report keys that changed.
- **Reorder:** edit `EFFECTS`; also renumber the `EFFECT NN —` header comments and the README table.
- **Add an effect:** new `//@region` before the `EFFECTS` line, add to `EFFECTS`, add an `EXPORTS` row (js/css regions), add CSS region if needed, add `gf` to `FONTS` if a new font. Rebuild nothing else.
- **Gotchas:** escape `</script>` inside JS template strings as `<\/script>`; measure DOM from always-visible elements (hidden ones read 0); canvas sizes in device px (`DPR`).

## Verify before pushing

`node --check` the extracted script; open the page in headless Chromium (Playwright is preinstalled in Claude's workspace) and check `.stage-tag` order + zero `pageerror`s; for Downloads, click `.dl-btn` with `acceptDownloads` and open the exported `index.html`. Google Fonts don't load in the sandbox (fallback fonts are expected).

## Credits

Based on CodePen pens by fand, 42EG4M1, Bennett Feely, Creative Ocean, jh3y, l3dlp, Amit Sheen, Pete Barr and tomncurry (credited in README.md). Bump is original (from Rob's landing page).
