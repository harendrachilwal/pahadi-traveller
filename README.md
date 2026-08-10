# Pahadi SaFar — पहाड़ी सफ़र

A single-page mood piece: dusk in the Uttarakhand hills, with Garhwali and
Kumaoni music playing. No signup, no menus, no scrolling. One screen.

Live at **https://www.pahaditraveller.in**

---

## The whole project is one file

```
index.html    the entire site — HTML, CSS and JavaScript together
CLAUDE.md     the project brief
vercel.json   hosting settings
```

There is nothing to install and nothing to build. Double-click `index.html`
and it opens and works. That's deliberate — it keeps the whole thing
editable by one person without a toolchain.

To see it locally with a proper address bar instead:

```bash
python3 -m http.server 3000
# then open http://localhost:3000
```

---

## Adding or changing a song

This is the part you'll touch most. Open `index.html` in any text editor and
search for **`PLAYLIST`** — it's near the top of the JavaScript, about
two-thirds of the way down the file.

You'll see a list of lines that look like this:

```js
{ id: '4FYT0TIZfnI', title: 'Bedu Pako Baro Masa', artist: 'Mahima Thakur', region: 'kumaoni' },
```

To add a song:

1. Open the song on YouTube.
2. Look at the address bar. It'll read something like
   `youtube.com/watch?v=4FYT0TIZfnI`. The 11 characters after `v=` are the ID.
3. Copy an existing line, paste it below, and swap in your ID, title and artist.

`region` can be `garhwali`, `kumaoni` or `folk`. It shows as the small tag on
the right of each row in the playlist.

To remove a song, delete its line. To reorder, move lines around — the list
plays top to bottom.

**A caution:** don't type an ID from memory or guess one. A made-up
11-character string looks completely normal and simply fails to play. Always
copy it from the address bar.

If a song ever stops working, it was probably taken down or the uploader
turned off embedding. The player notices and skips to the next song on its
own, so a dead link never freezes the music — but you can delete the line to
tidy up.

---

## Using your poster images as the background

The site can use full-screen poster images instead of the drawn hills. Put
JPEGs with these exact names next to `index.html` in the repo:

```
bg-flowers.jpg     bg-lake.jpg     bg-road.jpg
```

Easiest way: on the GitHub page of this repo press **Add file → Upload
files**, drag the images in, rename them to match, and commit. Vercel
redeploys on its own within a minute.

- One image is enough — whichever of the three exist are used.
- With more than one, they slowly crossfade every 16 seconds.
- The drawn hills switch off automatically while photos are present;
  delete the image files and the hills come back.
- Keep each JPEG under ~500 KB if you can (export at around 2000px wide) so
  phones load them fast.

Once photos exist, small round swatches appear in the **top-right corner**:
visitors can pin any single view, go back to the hand-drawn hills, or return
to the slow slideshow. The choice is per-visit (nothing is stored). To
rename what screen readers call each view, search for `PHOTOS` in
`index.html` and edit the `label` text.

The big headline is ordinary text near the bottom of `index.html` — search
for `wordmark-hi`. Current copy: **पहाड़ बुला रहे हैं / Come Home To The
Hills**, with the tagline *“पुरानी सड़कें, पुराने गीत — old roads, old
songs.”*

## Changing how it looks

**Colours.** Search for `COLOR TOKENS` at the very top of the file. Every
colour on the page is defined once in that block, so changing `--sky-glow`
changes the horizon everywhere it appears. Nothing below that block hardcodes
a colour.

**The mountains.** Search for `SCENE`. The big snow mountain is **Nanda
Devi** — her real two-summit profile, the main peak and Nanda Devi East
joined by a high ridge. Unlike everything else she doesn't move: repeat her
to loop the animation and you'd see two of her, and a mountain that far away
barely shifts as you drive anyway. The three nearer ridges are each drawn
twice side by side, which is what lets them slide sideways forever without a
visible seam.

If you change a moving ridge's shape, keep its two halves identical or the
loop will jump. And keep the curves — the `C` letters in the shape data are
what make the peaks look like real mountains rather than a cartoon zigzag.

**The road and the shops.** Search for `SNAKE ROAD` for the hairpin road —
the same path data draws the road and steers the little bus and car along
it, so if you reshape the road the traffic follows automatically. Search for
`ROADSIDE` for the chai shop and the बाल मिठाई (Bal Mithai) shop; the text on
the signboards is ordinary SVG `<text>`, so renaming a shop is just editing
the words.

**How fast things move.** Search for `drift`. Each ridge has its own duration
(30s, 24s, 18s, 14s). Nearer ridges move faster, which is what sells the
feeling of driving forward. Bigger number = slower.

---

## Things worth knowing

**Why the "Begin the drive" screen exists.** Browsers refuse to play sound
until someone clicks something. That button is the click. Remove it and the
site loads silent on phones with no way to start.

**Why YouTube.** Pahadi music is licensed and lives on YouTube. Playing it
through YouTube's official player means plays still count for the artists.

**The "driving" number.** It's atmosphere, not a real count. There's no server
behind this site, so it can't know how many people are here. It drifts
around a plausible number.

---

## Keyboard

| Key | Does |
|---|---|
| `Space` | play / pause |
| `←` `→` | previous / next song |
| `P` | open the playlist |
| `Esc` | close the playlist |

---

## Publishing changes

The site is connected to Vercel. Push to the `main` branch and it goes live
within about a minute:

```bash
git add index.html
git commit -m "Added two songs"
git push
```

In Vercel's project settings the framework preset is **Other**, with the
build command left empty — there's no build step to run.
