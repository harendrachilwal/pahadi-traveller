# CLAUDE.md — Pahadi Drive (पहाड़ी ड्राइव)

This file gives you full context for this project. Read it before making any changes.

---

## What we're building

An atmospheric single-page website that recreates the feeling of driving through the hills of Uttarakhand — misty deodar forests, distant snow peaks, small villages at dusk — while Garhwali and Kumaoni music plays.

It is a *mood piece*, not an app. Someone opens it when they miss home. There is no signup, no navigation, no scrolling. One screen, one job: make a person in a city feel like they're on a road above Kausani.

**Reference for the interaction model:** [saloon.wtf](https://saloon.wtf) — hidden YouTube player, custom controls, ambient scene.

---

## Hard constraints — do not violate these

The person building this is a **complete beginner** to web development. These constraints exist to keep the project understandable and editable by them.

1. **One file. `index.html`.** HTML, CSS, and JS all live inside it. CSS in a `<style>` block in the head, JS in a `<script>` block before `</body>`.
2. **No frameworks.** No React, no Next.js, no Vue, no Svelte, no Tailwind, no jQuery. Vanilla only.
3. **No build step.** No npm, no package.json, no bundler, no transpiler. Double-clicking `index.html` must open a fully working site.
4. **No local dependencies.** Fonts and the YouTube API load from CDNs. Nothing is installed.
5. **No browser storage.** No `localStorage`, no `sessionStorage`, no cookies. Keep state in plain JS variables.
6. **No backend.** No server, no database, no API keys, no environment variables.

If you think a change requires breaking one of these, **stop and ask first.** Don't silently introduce a framework.

---

## Project structure

```
pahadi-drive/
├── index.html       ← everything lives here
├── README.md        ← setup + how to add songs
├── bg-flowers.jpg   ← optional full-screen background posters —
├── bg-lake.jpg      ←   if present they replace the drawn scene
└── bg-road.jpg      ←   (see BACKGROUND PHOTOS in the script)
```

That's it. Don't create `src/`, `components/`, `styles/`, or `assets/` directories.

---

## Design direction

**Mood (updated Aug 2026, owner's direction):** a vintage Indian travel
poster — "Discover Uttarakhand" lithograph style. Aged cream paper, litho
green hills, white peaks with blue shadow, navy and terracotta ink, film
grain, a paper-margin frame. The owner supplied a reference poster image and
chose this over the original dusk direction; note that this deliberately
overrides the earlier rule against "warm cream backgrounds with terracotta
accents". The original dusk palette survives in git history (commit
`acec8e5` and earlier) if the site ever returns to night.

**Original mood (superseded):** dusk in the high Himalaya, amber into deep
teal, modeled on a Chopta/Munsiyari dusk.

### Color tokens

Defined as CSS custom properties on `:root`. Use these — don't invent new colors.

```css
--sky-deep:       #0a1620;   /* top of sky, near-midnight teal */
--sky-mid:        #12283a;
--sky-warm:       #3d4b4d;
--sky-glow:       #d68b52;   /* horizon amber */
--sky-glow-soft:  #e8b078;
--horizon:        #4a3a2a;
--snow:           #f0e4cf;   /* sunlit snow faces */
--pine-far:       #253944;
--pine-mid:       #17262f;
--pine-near:      #0a1418;   /* foreground pine silhouettes */
--ivory:          #eadfcb;   /* primary text */
--ivory-dim:      rgba(234, 223, 203, 0.68);
--ivory-faint:    rgba(234, 223, 203, 0.38);
--amber:          #e6a566;   /* active/current track highlight */
--glass:          rgba(10, 18, 24, 0.42);
--glass-strong:   rgba(10, 18, 24, 0.62);
--border:         rgba(234, 223, 203, 0.13);
--border-strong:  rgba(234, 223, 203, 0.22);
```

**Retro extension (added later):** the roadside shops and vehicles needed a
few colours a silhouette can't provide. They live in a clearly-marked block
after the tokens above — `--retro-bus`, `--retro-bus-trim`, `--retro-roof`,
`--sign-board`, `--sign-paint`, `--window-lit`, `--dust`. The original dusk
tokens are unchanged; use the retro set only for the roadside cluster and
traffic, never for sky or ridges.

### Typography

Four faces, loaded from Google Fonts in one `<link>`:

| Role | Font | Used for |
|---|---|---|
| Devanagari display | **Tiro Devanagari Hindi** | पहाड़ी ड्राइव wordmark |
| Display / serif | **Fraunces** (italic, light) | English wordmark, tagline, track titles |
| UI | **Instrument Sans** | buttons, labels, artist names |
| Data / mono | **JetBrains Mono** | clock, online counter, timestamps, track numbers |

The English wordmark is uppercase with wide letter-spacing (`0.32em`). Track titles use Fraunces at ~15–17px. Don't swap these for system fonts.

### Layout

A CSS Grid with three rows filling `100dvh`:

```
┌─────────────────────────────────────┐
│  [7:42 PM]            [23 driving]  │  ← topbar (auto)
│                                     │
│           पहाड़ी ड्राइव               │  ← center block (1fr, self-center)
│          PAHADI · DRIVE             │
│      "Music from the hills…"        │
│         ( ) ( ) ( )                 │     spotify / ytm / playlist icons
│                                     │
│  ┌───────────────────────────────┐  │
│  │ NOW PLAYING                ⤨  │  │  ← player (auto, self-end)
│  │ Bedu Pako Baro Masa           │  │
│  │ Traditional                   │  │
│  │ 0:42 ──●─────────────── 4:18  │  │
│  │        ⏮   ▶   ⏭              │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

Page-level `overflow: hidden` on `html, body` — this site never scrolls.

### The scene (signature element)

This is where the boldness is spent. Everything else stays quiet.

Four bottom-anchored SVG ridge layers, each in a `.ridge` div positioned by `bottom:` percentage, plus a road and three fog bands:

| Layer | `bottom` | What it is |
|---|---|---|
| `.ridge-snow` | 24% | Far snow-capped peaks, gradient fill, sunlit highlight faces |
| `.ridge-far` | 18% | Hazy blue-grey second ridge line |
| `.fog-2`, `.fog-1` | 36%, 26% | Drifting mist bands |
| `.ridge-mid` | 9% | Forested hills with sparse pine tips on the crest |
| `.fog-3` | 14% | Low valley fog |
| `.road-layer` | 4% | S-curve road receding into distance, warm edge light |
| `.ridge-near` | 0 | Foreground pine silhouettes + tiny warm village lights |
| `.vignette` | — | Radial darkening at edges |

**Critical detail:** mountain paths must use **Bézier curves (`C` commands), not straight-line sawtooth (`L` commands)**. Symmetric zigzag peaks read as clip-art. Real Himalayan silhouettes have wide asymmetric bases and uneven spacing.

**Parallax:** each ridge has its own `translateX` keyframe animation on a different duration (30s / 24s / 18s / 14s). Nearer layers move further. This reads as slow forward motion, not a slideshow.

**Fog:** three large blurred (`filter: blur(38px)`) gradient divs with `mix-blend-mode: screen`, drifting sideways on deliberately non-matching timelines (75s, 110s, 140s) so the loop is never visible.

---

## Music system

### How it works

Uses the official **[YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference)**.

- The iframe is mounted in `<div id="yt-player">`, positioned off-screen at `top: -9999px; left: -9999px` with `width/height: 1px`.
- **Don't use `display: none`** — some browsers refuse to play audio in a fully hidden iframe. Off-screen positioning is the reliable trick.
- All controls are custom HTML/CSS. `playerVars` sets `controls: 0`, `modestbranding: 1`, `playsinline: 1`, `rel: 0`, `iv_load_policy: 3`.

### Why YouTube instead of hosted MP3s

Pahadi music is licensed and lives on YouTube. Embedding through the official API respects the artists' monetization and YouTube's terms, and gives access to thousands of songs at no cost. **Never** suggest ripping audio, using `youtube-dl`, or proxying the stream.

### Key API surface

```js
window.onYouTubeIframeAPIReady = function() { … }   // global callback, must be on window
new YT.Player('yt-player', { videoId, playerVars, events })
player.loadVideoById(id)    // load + play
player.cueVideoById(id)     // load, don't play
player.playVideo() / pauseVideo() / seekTo(sec, true)
player.getCurrentTime() / getDuration()
```

Events wired: `onReady`, `onStateChange`, `onError`.

`onStateChange` compares against `YT.PlayerState.PLAYING / PAUSED / ENDED`. On `ENDED`, advance to the next track.

### Error handling — important

YouTube error codes `2, 5, 100, 101, 150` mean the video is removed, private, or embed-disabled. `onError` must **auto-advance to the next track** so one dead ID never freezes the queue. Guard with a counter so it can't infinite-loop through an entirely broken playlist.

### Autoplay policy

Browsers block audio until the user interacts. That's what the intro overlay ("Begin the drive" button) is for — the click satisfies the gesture requirement and triggers the first `playVideo()`. Don't remove the intro; without it, mobile is silent on load.

---

## The playlist

A plain array near the top of the `<script>` block. This is the **only thing the owner will regularly edit**, so keep it at the top, well-commented, and simple.

```js
const PLAYLIST = [
  { id: 'YOUTUBE_VIDEO_ID', title: 'Song name', artist: 'Artist', region: 'kumaoni' },
  // …
];
```

- `id` — the 11-character string after `v=` in a YouTube URL
- `region` — `'garhwali' | 'kumaoni' | 'folk'`, rendered as a small pill tag in the drawer

### Songs the project targets

Currently seeded with placeholder IDs (`PLACEHLDR01`…) that must be replaced with real video IDs.

**Traditional & classics:** Bedu Pako Baro Masa · Chaita Ki Chaitwali · Ghughuti Na Basa · Chhopati · Jhumelo · Chhaila Bihari
**Narendra Singh Negi:** Nauchami Narena · Sulpa Ki Saaj · Bol Chitthi · Geet Ganga · Rangeeli Bwari
**Modern pahadi:** Dhana (Priyanka Meher) · Gajra (Sanjay Bhandari) · Pahadan (Inder Arya) · Kaidi Ghar Chha (Pappu Karki) · Aakhar & Chhaam Ghungharu (Pritam Bhartwan) · Twilo Ma (Meena Rana) · Aiga Kausani (Lalit Mohan Joshi)
**Devotional & road-trip:** Aayo Re Kanhaiya · Bhagwati Ma · Meri Munsyari · Ghar Aaja Pardesi

⚠️ **Never invent YouTube video IDs.** A fabricated 11-character string looks plausible and silently 404s. If you can't verify an ID, leave the placeholder and tell the owner to fill it in.

---

## Features

**Player:** play/pause, previous, next, shuffle, seekable progress bar (click + drag + touch), current/total time, track title and artist.

Previous-button convention: if more than 3 seconds into a track, restart it; otherwise go to the actual previous track. (Standard music-player behavior.)

**Playlist drawer:** slides up from the bottom, glass background, numbered rows with title/artist/region tag. Current track's title is `--amber`. Backdrop click closes; swipe-down on mobile closes.

**Clock:** 12-hour format with a smaller AM/PM. Updates every 15 seconds.

**Online counter:** a *simulated* random walk — there's no backend, so it can't be real. Starts at 18–38, drifts back toward 26, clamped to 11–46, updates every ~4.2s. Keep it plausible; don't make it jump wildly.

**Keyboard:** `Space` play/pause · `←/→` prev/next · `P` playlist · `Esc` close drawer.

---

## Quality floor

Meet these without announcing them:

- **Responsive.** Mobile is the primary target — most people open this on a phone. Breakpoint at 520px shrinks padding, hides the tagline, shrinks the play button. Test at 390×844.
- **Safe areas.** Use `env(safe-area-inset-*)` for notched phones, and `100dvh` (not `100vh`) so mobile browser chrome doesn't clip the player.
- **Reduced motion.** A `@media (prefers-reduced-motion: reduce)` block kills all parallax, fog drift, and twinkle.
- **Keyboard focus.** Visible `:focus-visible` outline in `--amber` on every interactive element.
- **ARIA.** Player region labeled, progress bar is a `role="slider"` with `aria-valuenow`, drawer toggles `aria-hidden`.
- **Escaped HTML.** Track titles go through an `escapeHTML()` helper before `innerHTML`.

---

## SEO / sharing

`<title>`, `<meta name="description">`, `theme-color`, and Open Graph tags (`og:title`, `og:description`, `og:image`, `og:type`) plus `twitter:card`. An `og.png` in the project root is referenced but optional.

---

## Deployment

Static site, no build. Vercel is the target:

```bash
npx vercel
```

Leave all build settings empty. Or drag the folder onto [vercel.com/new](https://vercel.com/new) — that path is better for a beginner since it needs no terminal.

Domain ideas: `pahadidrive.com`, `devbhoomidrive.com`, `chalopahad.com`, `pahadmein.com`.

---

## Working with the owner

They're new to web development. When you make changes:

- **Explain in plain language.** Say "the block that draws the mountains," not "the fourth SVG path node."
- **Point to landmarks.** "Search for `PLAYLIST`" beats "line 847."
- **Don't refactor for elegance.** Extracting things into modules or adding abstraction makes this *harder* for them to maintain, even when it's technically cleaner. Verbose and obvious wins.
- **Comment generously** in the JS, especially around the playlist and anything they'd plausibly want to tweak.
- **Flag tradeoffs honestly.** If something can't work (real online counts without a backend, verified video IDs), say so rather than faking it.

---

## Cultural note

Uttarakhand's music tradition runs deep. Narendra Singh Negi, Pritam Bhartwan, Chandra Singh Rahi, and Meena Rana carry the weight of what pahadi identity sounds like; Priyanka Meher, Sanjay Bhandari, Inder Arya, Pappu Karki, and Lalit Mohan Joshi have kept it alive for a generation raised in the plains.

Treat the names, spellings, and Devanagari with care. Get transliterations right. This isn't decorative theming — it's someone's home.
