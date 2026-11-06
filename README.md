# Yapanism · Meibutsu Finder (מאתר המייבוצו)

A single-file, self-contained web app for discovering the regional specialties
(*meibutsu* / 名物) of all 47 Japanese prefectures. Hebrew / RTL interface,
bilingual (Hebrew + English) search, 719 curated items across food, sweets,
drinks, crafts and souvenirs.

**No build step. No dependencies. No backend.** Just static HTML/CSS/JS in one
file. The only network call is to Google Fonts, which degrades gracefully to
system fonts offline.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | The app — Hebrew/RTL, primary deliverable. GitHub Pages serves this automatically. |
| `meibutsu-finder-en.html` | English-language version, kept as reference/backup. |
| `photos/` | Card photos, one folder per prefecture (see **Photos** below). |
| `CHANGELOG.md` | What changed and when (Hebrew). |
| `README.md` | This file. |
| `.gitignore` | Standard OS/editor ignores. |

---

## Preview locally

It's a static file, so either:

- **Simplest:** double-click `index.html` to open it in a browser, **or**
- **Via a local server** (recommended, mirrors production):
  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```

---

## Create the Git repo

A repo is already initialized here with one commit. To publish it:

```bash
# 1. create an empty repo on GitHub (no README/…, keep it bare), then:
git remote add origin https://github.com/<user>/<repo>.git
git branch -M main
git push -u origin main
```

If the `.git` folder didn't survive transfer (some unzip tools drop it), just
initialize fresh:

```bash
git init
git add .
git commit -m "Initial commit: Yapanism Meibutsu Finder"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

---

## Deploy to GitHub Pages

1. Push to GitHub (above).
2. Repo → **Settings** → **Pages**.
3. **Source:** *Deploy from a branch* → **Branch:** `main` → **Folder:** `/ (root)` → **Save**.
4. The site goes live at `https://<user>.github.io/<repo>/` within a minute or two.

Because the Hebrew app is named `index.html`, it loads at the root URL with no
extra configuration. The English version is reachable at
`https://<user>.github.io/<repo>/meibutsu-finder-en.html`.

---

## Editing the data

All content lives in the `PREF` array inside `index.html` (see the comment
block at the top of the `<script>`). Each prefecture holds an `items` array of
objects shaped like:

```js
{
  name: "Edomae Sushi",      // romaji / source name (kept Latin)
  jp:   "江戸前寿司",          // Japanese
  cat:  "food",              // one of: food | sweets | drink | craft | souvenir
  he:   "סושי אדומאה",        // Hebrew name
  desc: "…",                 // English description
  heDesc: "…",               // Hebrew description
  where: "…",                // English "where to find"
  heWhere: "…"               // Hebrew "where to find"
}
```

**Editing principle used throughout this project: additive only.** New content
is appended; nothing already in place is removed or overwritten. Please keep to
that when extending the data, and re-check that every item keeps all eight
fields.

---

## Photos

Each photo lives at `photos/<prefecture-id>/<item-slug>.webp` (960×640), with a
smaller `<item-slug>-480.webp` (480×320) next to it; the browser picks the size
that fits the screen. Every photo is pre-cropped to 3:2 around its subject, so
the card frame never crops it again.

An item shows a photo when its data entry has
`img: "photos/<prefecture-id>/<item-slug>.webp"`. Items without `img` show a
plate with the Japanese name in the same frame size, so the grid stays even. If
a photo file is missing, the card falls back to that plate on its own.

155 items currently have photos: Tokyo, Kanagawa, Kyoto, Osaka, Nara, Hiroshima,
Hokkaido, Aomori, Fukuoka and Okinawa. To preview locally with photos, open `index.html` from this
folder (the `photos/` folder must sit next to it).

---

## Map

The clickable map of Japan uses prefecture outlines from
[Natural Earth](https://www.naturalearthdata.com/) (public domain — free for
commercial use, no attribution required), simplified and embedded in `index.html`
as SVG paths. Okinawa is shown as an inset of the main island only.

---

## Technical notes

- **Single self-contained file** — all CSS and JS are inline in `index.html`.
- **Hebrew / RTL** — `<html lang="he" dir="rtl">`, Assistant font for Hebrew,
  Shippori Mincho / Zen Kaku Gothic for Japanese; logical CSS properties
  throughout so bidi text lays out correctly.
- **Dark-mode safe** — a fixed `body::before` underlay + `color-scheme: only
  light` keep the washi-paper background painted inside iOS/webview dark mode.
- **Device-aware & responsive** — a small script tags `<html>` as `is-mobile` /
  `is-desktop` (combining a coarse-pointer test with a width test, re-checked on
  resize/rotate), and breakpoints at 900 / 600 / 380 px adapt the layout for
  tablet and phone with finger-friendly tap targets on touch devices.
- **Bilingual search** — matches prefecture, city/landmark aliases, region and
  Japanese name, in both Hebrew and English.
