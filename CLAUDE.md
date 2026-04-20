# Hyperframes Editor — Workspace Guide

HTML-native video workspace built on [Hyperframes](https://hyperframes.heygen.com). Scaffolded 2026-04-17 from the `product-promo` example. Not a Remotion project — this is the other pipeline.

**This workspace hosts multiple video projects, one folder each, all under `video-projects/`.** The workspace root holds shared tooling (`node_modules/`, `package.json`, `.claude/`, this `CLAUDE.md`, `DESIGN.ais-example.md`, `MOTION_PHILOSOPHY.md`) — never put `index.html`, `assets/`, `compositions/`, or `renders/` directly at the root. Always work from inside a project subfolder.

## MOTION_PHILOSOPHY.md — READ BEFORE BRAINSTORMING

**`MOTION_PHILOSOPHY.md` (at the workspace root) is the canonical motion-graphics aesthetic for this workspace.** It is the deconstructed playbook of the Infinite Global Payments 30s spot — the gold standard Nate has chosen for every motion build going forward.

**You MUST read it before:**
- Brainstorming a new composition, scene, or storyboard
- Proposing a visual direction, palette, or pacing
- Picking transitions, animations, or registry blocks
- Designing any kinetic typography, logo reveal, or product showcase

**How to use it:**
1. **Always read the full file** at the start of any creative session — `Read MOTION_PHILOSOPHY.md` from the workspace root. Don't skim, don't quote from memory. The doc evolves.
2. **Re-read sections 0 (10 Laws) and 4 (pre-flight checklist)** every time, even on quick iterations. They are the discipline.
3. **Apply the defaults**: ~1.5s avg scene length, black canvas + perspective grid + crosshairs + vignette + grain on every scene, chrome-gradient text with halo glow, motion-blurred whip transitions (never hard cuts), ≤5 symbolic colors, hold the outro 4–6s, rule of threes.
4. **Use the recipes**: section 3 has copy-pasteable HyperFrames patterns (composition shell, kinetic-type opener, whip-streak, color-recolor trick, registry block mappings).
5. **Run the pre-flight checklist** before claiming any motion piece is done. The "What Would Infinite Do?" 10-question test in section 5 is a good gut check before showing Nate.

**When NOT to apply it:**
- If a brand brief explicitly demands a different aesthetic (e.g., AIS Shorts use bright/playful Roboto Mono + Montserrat with the AIS palette — see `DESIGN.ais-example.md`). In those cases, keep the *discipline* (one idea per beat, motion in transitions, breathing outros, callbacks) but adapt the *palette and texture* to the brand.

If `MOTION_PHILOSOPHY.md` is missing from the workspace root, stop and ask Nate before brainstorming — it should always be there.

## Skills — USE THESE FIRST

**Always invoke the matching skill before writing or modifying compositions.** Skills encode framework-specific patterns (`window.__timelines` registration, `data-*` attribute semantics, shader-compatible CSS, relative-timing syntax) that are NOT in generic web docs. Skipping them produces broken compositions.

| Skill                    | Command                    | When to use                                                                               |
| ------------------------ | -------------------------- | ----------------------------------------------------------------------------------------- |
| `hyperframes`            | `/hyperframes`             | Authoring/editing compositions, captions, TTS, audio-reactive animation, transitions      |
| `hyperframes-cli`        | `/hyperframes-cli`         | CLI commands: `init`, `add`, `lint`, `preview`, `render`, `transcribe`, `tts`, `doctor`   |
| `gsap`                   | `/gsap`                    | GSAP animation — timelines, easing, stagger, ScrollTrigger, plugins, performance          |
| `hyperframes-registry`   | `/hyperframes-registry`    | Installing catalog blocks/components via `npx hyperframes add <name>`                     |
| `website-to-hyperframes` | `/website-to-hyperframes`  | Turning a URL into a composition (7-step capture-to-video pipeline)                       |

Not present? `npx skills add heygen-com/hyperframes --yes` then reopen this directory.

## Commands

```bash
# Authoring loop
npx hyperframes preview                          # Studio opens in browser with hot reload (port 3002)
npx hyperframes lint                             # static HTML check — always run before rendering
npx hyperframes compositions                     # list comp IDs + resolved durations
npx hyperframes render --quality draft --output renders/draft.mp4   # fast iteration render
npx hyperframes render --quality standard --output renders/final.mp4 # visually lossless 1080p

# Catalog & install
npx hyperframes catalog --type block             # browse 38 blocks
npx hyperframes catalog --type component         # browse 3 components
npx hyperframes add <name>                       # install a catalog item into compositions/

# Media pipeline (baked into CLI — no Whisper CLI needed)
npx hyperframes transcribe <file> --model small.en --json   # word-level timestamps
npx hyperframes tts "text" --voice am_adam --output narration.wav   # on-device Kokoro-82M

# Diagnostics
npx hyperframes doctor                           # env check (Node, FFmpeg, Chrome, Docker)
npx hyperframes info --json                      # project stats
npx hyperframes benchmark                        # find optimal workers/quality
npx hyperframes docs <topic>                     # inline docs: data-attributes, gsap, rendering, examples, troubleshooting, compositions
```

### Render flags worth knowing

- `--quality draft|standard|high` — CRF 28 / 18 / 15 (standard is visually lossless at 1080p)
- `--fps 24|30|60` (default 30)
- `--format mp4|mov|webm` — `mov` = ProRes 4444 with alpha, `webm` = VP9 alpha (Chromium only)
- `--workers <n>` / `--gpu` / `--docker` / `--crf <n>` / `--video-bitrate 10M`
- `--max-concurrent-renders <n>` — when running the producer server

## Workspace Layout

```
Hyperframes Editor/
├── CLAUDE.md, AGENTS.md, DESIGN.ais-example.md         ← workspace docs
├── MOTION_PHILOSOPHY.md                    ← gold-standard motion-graphics aesthetic (READ before brainstorming)
├── package.json, node_modules/              ← workspace tooling
├── .claude/                                  ← skills + plugin config
├── assets/                                   ← shared assets (AIS brand, music)
└── video-projects/                           ← one folder per video
    ├── aisoc-hype/
    ├── aisoc-lesson-5-1/
    ├── may-shorts-19/
    └── clickup-demo/
```

Each project under `video-projects/<name>/` is a self-contained Hyperframes project:

- `index.html` — root composition entry point
- `compositions/` — sub-compositions loaded via `data-composition-src`
  - `compositions/components/` — shared snippets installed by `npx hyperframes add <component>`
- `assets/` — media files for this project (videos, audio, images, SVG, transcripts). Brand assets that multiple projects need (AIS logo, brand-tokens.css) are duplicated per-project, not symlinked — keeps each project portable.
- `renders/` — render outputs for this project (gitignored)
- `hyperframes.json` — CLI config (registry URL, paths — all relative to the project folder)
- `meta.json` — project metadata (id, name, dimensions, fps)
- (optional) `STORYBOARD.md`, `scripts/`, etc. — anything project-specific

### Always run the CLI from inside the project folder

```bash
cd video-projects/may-shorts-19
npx hyperframes lint
npx hyperframes preview
npx hyperframes render --quality standard --output renders/final.mp4
```

The CLI reads `hyperframes.json`/`meta.json` from the current directory and resolves `assets/`, `compositions/`, `renders/` relative to it. Running it from the workspace root will fail or scan the wrong files.

### Adding a new video project

1. `mkdir video-projects/<new-project-slug>` (kebab-case, e.g. `q3-launch-promo`)
2. `cd video-projects/<new-project-slug>`
3. Either `npx hyperframes init` to scaffold, or copy the structure from a sibling project (`cp -r ../may-shorts-19/{hyperframes.json,meta.json} .` then edit `meta.json` for the new id/name/dimensions, and create empty `index.html`, `compositions/`, `assets/`, `renders/`)
4. Pull in any shared brand assets the project needs (e.g. `cp ../../assets/brand-tokens.css ../../assets/AIS\ Logo\ PNG.png assets/`)
5. Build the composition; lint + render from inside this folder

### What lives at the workspace root

- **Motion-graphics philosophy:** `MOTION_PHILOSOPHY.md` (gold-standard aesthetic, deconstructed Infinite Payments spot — read before brainstorming any composition)
- Shared brand source-of-truth: `DESIGN.ais-example.md` (AIS brand spec — kept as a worked example; students should write their own `DESIGN.md` for their brand), root `assets/` (AIS Logo PNG, brand-tokens.css, AIS Background.png) — copy into a project's `assets/` when needed
- Shared raw-recording stash: large source MP4s/MP3s that aren't yet assigned to a project (e.g. raw lesson recordings, license-free music) can sit at root until they're moved into a project's `assets/`
- Tooling: `node_modules/`, `package.json`, `.claude/`, `.gitignore`, `skills-lock.json`

## ARM64 hosts (Grace Hopper / Jetson / Apple-silicon Linux)

**Symptom:** `npx hyperframes render` dies immediately on every worker with
`chrome-headless-shell: Syntax error: word unexpected` / `exec format error`.
**Cause:** Hyperframes caches `chrome-for-testing` under `~/.cache/hyperframes/chrome/chrome-headless-shell/linux_arm-<ver>/chrome-headless-shell-linux64/…` but the downloaded binary is x86_64 ELF — `chrome-for-testing` doesn't publish ARM64 builds, and puppeteer's detection picks the wrong variant. `uname -m` reports `aarch64`; `file` on the cached binary reports `x86-64`. `--docker` also fails because the default image is `linux/amd64` and `qemu-user-static` isn't registered for binfmt.
**Fix (confirmed working 2026-04-19 on this workspace):** Replace the cached binary with a native aarch64 Chromium. Playwright's cache usually has one already — check `~/.cache/ms-playwright/chromium-*/chrome-linux/chrome` with `file`. Back up the broken binary and symlink the native one on top:

```bash
BROKEN=~/.cache/hyperframes/chrome/chrome-headless-shell/linux_arm-131.0.6778.85/chrome-headless-shell-linux64/chrome-headless-shell
mv "$BROKEN" "${BROKEN}.x64.bak"
ln -s ~/.cache/ms-playwright/chromium-1208/chrome-linux/chrome "$BROKEN"
```

`PUPPETEER_EXECUTABLE_PATH` does **not** work — Hyperframes ignores it and always resolves from the cache path above, so the symlink is the actual fix. If Playwright isn't installed, `npx playwright install chromium` grabs the arm64 build; otherwise `sudo snap install chromium` also produces a native binary you can point to. The version number in the cache path may change with `hyperframes` upgrades — re-apply the symlink after each upgrade.

## Render Contract (the must-dos and must-not-dos)

1. Root `<div>` needs `id`, `data-composition-id`, `data-start="0"`, `data-width`, `data-height`.
2. Timed visible elements need `class="clip"` — **except** `<video>` and `<audio>` (adding `class="clip"` to `<video>` breaks it).
3. Every timed element needs `data-start`, `data-duration`, `data-track-index`.
4. `data-start` can reference another clip's id: `data-start="intro"`, `data-start="intro + 2"`, `data-start="intro - 0.5"`. Same-track clips cannot overlap — use different `data-track-index` values.
5. `<video>` must be `muted`; audio belongs in sibling `<audio>` elements for the mixer. `data-has-audio="true"` only when the video's own audio should feed the mix.
6. Every composition registers exactly one GSAP timeline, paused, on `window.__timelines["<data-composition-id>"]`. Key must match `data-composition-id` exactly.
7. Composition duration = `tl.duration()`. If the timeline is shorter than the video, the video truncates. Pad with `tl.set({}, {}, <seconds>)` to extend.
8. Never call `.play()`, `.pause()`, or set `.currentTime` on media. The framework owns playback.
9. Never animate `width`/`height`/`top`/`left` directly on a `<video>` — the browser freezes frames. Wrap in a `<div>` and animate the wrapper.
10. Sub-compositions use `<template>` + `data-composition-src`. Their timelines auto-link to the parent — never do `masterTL.add(child)`.
11. Determinism: no `Date.now()`, no unseeded `Math.random()`, no render-time network fetches. Use seeded PRNGs.

## Authoring Loop

1. **Read `MOTION_PHILOSOPHY.md`** if you haven't this session — it sets the aesthetic baseline (1–2s scenes, chrome type, motion-blur transitions, etc.).
2. Pick the skill → invoke `/hyperframes` (or sibling) before editing.
3. Edit HTML in `index.html` or `compositions/<name>.html`.
4. `npx hyperframes lint` — fix errors, triage warnings.
5. **Localhost Studio preview** — before **any** render (even a draft), start `npx hyperframes preview` in the background and hand Nate the URL. He eyeballs the edit live and iterates; no render cycle until he's seen it. See "Localhost Preview Before Any Render" below.
6. Only after Nate says the live preview looks right: `render --quality draft` for a draft MP4.
7. **Visual verification** (REQUIRED before handoff) — see "Visual Verification" below.
8. **Run the `MOTION_PHILOSOPHY.md` pre-flight checklist** (section 4) before claiming done.
9. Second localhost preview pass on the draft MP4 (via static server on port 8080 for scrubbable playback) — wait for explicit sign-off before the final render.
10. Final: `render --quality standard` (or `high --docker` for archival deterministic output).

## Visual Verification (MANDATORY before delivery)

Lint passing ≠ design working. Never tell Nate a render is done until you have actually looked at the frames. No exceptions. A "successful" render with a cropped face, misaligned text, or a scene landing on the wrong word is a broken render — and lint exit codes will not catch any of that. Nate has explicitly asked for this gate — treat skipping it as a regression.

**Required checks before delivery:**

1. Render a draft: `npx hyperframes render --quality draft --output renders/<name>-draft.mp4`
2. Pull one frame per scene at its hero moment, plus frames at any mid-entrance or transition at risk. Cover the full timeline — don't cherry-pick.
   ```bash
   mkdir -p renders/frames
   for t in <scene1-t> <scene2-t> ...; do
     ffmpeg -y -ss $t -i renders/<name>-draft.mp4 -frames:v 1 -q:v 2 "renders/frames/t${t}.png"
   done
   ```
3. Call `Read` on every PNG so the image actually loads into context. Do NOT just list filenames. Verify:
   - Speaker's face is not cropped in any bottom-half scene
   - Full-screen vs bottom-half face mode is correct for each scene
   - Scene transitions land on the intended word
   - Captions are on-brand (AIS: Montserrat, #37bdf8 accent, #f09025 hot) and readable
   - No text overflow, no unintentional overlap, no blank frames
4. If anything is wrong — fix, re-render, re-verify. Never ship a broken render and let Nate find the bug.
5. Only then run the `standard` quality render and report the path.

**Pre-render (live scrubbing):** Playwright 1.59.1 is installed. For quick contrast/layout checks on a single scene mid-authoring without paying a full render, run `npx hyperframes preview` (opens on localhost:3002) and drive Playwright to screenshot the live state at specific timestamps. Useful when iterating on one scene's visual balance before committing to a draft render.

## Localhost Preview Before Any Render (MANDATORY)

**Every edit pass gets two preview gates**: one on the live Studio **before** any render (so Nate can iterate on cheap edits without waiting for a render), and one on the rendered MP4 **before** the final `--quality standard` bake. Do not run `render --quality draft` OR `render --quality standard` until Nate has eyeballed the relevant state in his browser.

### Gate 1 — Live Studio preview (before any render)

After editing compositions and before any render command:

1. Start Studio in the background:
   ```bash
   cd video-projects/<project-slug>
   npx hyperframes preview    # run_in_background: true
   ```
2. Wait for "Studio running" on http://localhost:3002.
3. Hand Nate the URL + tell him exactly which sub-compositions to scrub (individual comp URLs load fastest — the master composition can stall when it includes WebGL shader blocks under software WebGL fallback). Example: `http://localhost:3002/?comp=v01-kinetic-type`.
4. Wait for Nate's explicit sign-off on the live preview ("looks good, render a draft" / "ship it" / "go ahead"). Silence is not approval.
5. Hot reload means any further edit you make shows up live without a restart.

### Gate 2 — Rendered MP4 preview (before final)

After frame-verification passes on a draft render:

6. Serve `renders/` via `npx serve . -p 8080 -n` (NOT Python's `http.server` — it doesn't support HTTP Range requests, so scrubbing breaks). Hand Nate `http://localhost:8080/<project>-draft.mp4`.
7. Wait for explicit sign-off on the full motion + audio playback.
8. Then run the final `--quality standard` render; report the output path.

Why two gates: the live Studio catches layout/timing/visual bugs on edits you just made — before spending ~2 minutes per render iteration. The rendered-MP4 gate catches pacing, audio sync, and beat-to-beat feel that only reads correctly in real-time playback. Skipping either one turns render cycles into expensive guesses.

**If the master composition stalls in Studio** (software WebGL + multiple shader blocks): route Nate to individual sub-composition URLs instead. They load instantly and isolate the change you're previewing.

The render is the ground truth. "The code looks correct" doesn't clear the bar.

## Asset Prep

Re-encode raw recordings to H.264 MP4 before referencing as `<video src>`:

```bash
ffmpeg -i raw.mov -c:v libx264 -preset medium -crf 20 -c:a aac -b:a 192k -movflags +faststart assets/clip.mp4
```

Keeps `assets/` light and avoids codec issues during capture. Use `npx hyperframes doctor` if a render fails partway.

## Prompting Shorthand (what the `/hyperframes` skill understands)

- **Motion easing:** smooth / snappy / bouncy / springy / dramatic / dreamy
- **Caption energy:** hype / corporate / tutorial / storytelling / social
- **Transition energy:** calm (blur) / medium (push) / high (zoom, glitch)
- **Audio reactivity:** bass→scale, treble→glow, amplitude→opacity, mids→shape. Keep text reactivity at 3–6%; backgrounds can go 10–30%.

Cold-start prompt shape: *"Using /hyperframes, create a 10-second product intro with a fade-in title over a dark background and subtle background music."* Warm-start: *"Summarize this PDF into a 45-second pitch video using /hyperframes."*

## Registry (available via `npx hyperframes add <name>`)

- **Blocks (38):** data viz (`data-chart`, `flowchart`), outros (`logo-outro`), social overlays (`instagram-follow`, `tiktok-follow`, `yt-lower-third`, `x-post`, `reddit-post`, `spotify-card`, `macos-notification`), app/UI (`app-showcase`, `ui-3d-reveal`), shader transitions (`glitch`, `whip-pan`, `cinematic-zoom`, `flash-through-white`, `light-leak`, `ripple-waves`, `chromatic-radial-split`, `cross-warp-morph`, `domain-warp-dissolve`, `gravitational-lens`, `ridged-burn`, `sdf-iris`, `swirl-vortex`, `thermal-distortion`), CSS transition packs (`transitions-3d|blur|cover|destruction|dissolve|distortion|grid|light|mechanical|other|push|radial|scale`)
- **Components (3):** `grain-overlay`, `shimmer-sweep`, `grid-pixelate-wipe`
- Browse: `npx hyperframes catalog --type block --json`

## Documentation — fetch when the skills don't cover enough

The `/hyperframes*` skills encode the common authoring patterns, but the hosted docs are the source of truth for **every block's props, every package's API, and deeper usage examples**. Reach for them when brainstorming scenes, picking a transition or component, or digging into a package you haven't used before. The catalog pages in particular hide a lot of gems — don't guess at a block's props, fetch the page.

**Entry points:**

- **Agent index (fetch first if unsure of a path):** https://hyperframes.heygen.com/llms.txt — the complete sitemap
- **Full site:** https://hyperframes.heygen.com/introduction
- **Inline terminal docs:** `npx hyperframes docs <topic>` — topics: `data-attributes`, `gsap`, `rendering`, `examples`, `troubleshooting`, `compositions`
- **Source repo:** https://github.com/heygen-com/hyperframes

**Known URL patterns (hit directly with WebFetch):**

- **Catalog — Blocks (38):** `https://hyperframes.heygen.com/catalog/blocks/<slug>` — e.g. `instagram-follow`, `flowchart`, `data-chart`, `cinematic-zoom`, `glitch`, `whip-pan`, `sdf-iris`, `light-leak`, `logo-outro`, `app-showcase`, `ui-3d-reveal`, `macos-notification`, plus all the transition packs (`transitions-3d`, `transitions-blur`, …). Full props + examples for each.
- **Catalog — Components (3):** `https://hyperframes.heygen.com/catalog/components/<slug>` — `grain-overlay`, `grid-pixelate-wipe`, `shimmer-sweep`
- **Packages:** `https://hyperframes.heygen.com/packages/<name>` — `cli`, `core`, `engine`, `player`, `producer`, `studio`. Use when scripting beyond HTML (e.g. batch rendering via producer, player runtime hooks).
- **Reference:** `https://hyperframes.heygen.com/reference/html-schema` — authoritative data-attribute + timeline-registration spec
- **Core concepts & guides:** listed in `llms.txt` — covers compositions, deterministic rendering, frame adapters, GSAP animation, prompt guide, website-to-video, troubleshooting, common mistakes. Fetch `llms.txt` to resolve exact paths.

**When to reach for this:** brainstorming a new scene ("what block could sell this beat?"), looking up a block's exact props before wiring it into a composition, deep-diving into a package API, or debugging something the render contract doesn't explain.
