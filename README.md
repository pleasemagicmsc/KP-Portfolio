# Portfolio

Static site. No build step, no framework. Vercel serves it straight from the repo.

```
index.html      the site
works.json      the work manifest — the site reads this
Assets/         posters and self-hosted video
admin.html      DEV ONLY — the studio. Delete before launch.
```

## Running it locally

`fetch` is blocked on `file://` URLs, so opening `index.html` by double-clicking will show
a "manifest didn't load" message. Serve the folder instead:

```bash
npx serve .
# or
python3 -m http.server 8000
```

## Adding work

1. Open `admin.html` through the same local server.
2. Drop in video or image files, or paste a YouTube / Vimeo URL.
3. Name each piece, tick its categories, drag rows to set the order.
4. **Download renamed media** — files come back named after the title, plus an
   auto-captured poster frame for each video.
5. Move those into `Assets/`.
6. **Export works.json**, replace the one at the repo root.
7. Commit, push, done.

Work stays in the browser's IndexedDB between sessions, so you can close the tab
mid-batch without losing anything. Nothing leaves your machine until you export.

## Where video should live

| Size | Put it |
|---|---|
| Under ~20 MB, short loops | `Assets/`, self-hosted |
| Anything longer | YouTube or Vimeo, unlisted, added as a link |

Git repos handle large binaries badly and Vercel's bandwidth is metered. Full edits and
reels belong on a video host; the site embeds them and plays them in the lightbox either way.

## Categories

Defined twice, on purpose, so neither file depends on the other:

- `CATEGORIES` at the top of the `<script>` in `index.html`
- `CATEGORIES` at the top of the `<script>` in `admin.html`

Edit both when adding or renaming one. The `id` is what lands in `works.json`; the `label`
is what shows on the pill.

## Manifest shape

```json
{
  "id": "w_a1b2c3",
  "title": "Nightshift",
  "kind": "Short Film",
  "year": "2025",
  "categories": ["video", "shortfilm"],
  "type": "youtube",
  "src": "dQw4w9WgXcQ",
  "poster": "Assets/nightshift-poster.jpg",
  "order": 3
}
```

`type` is one of `youtube`, `vimeo`, `video`, `image`. For the hosted types, `src` is the
bare video ID. YouTube items fall back to YouTube's own thumbnail if no poster is exported.
`order` controls position; the site sorts on it.

You can hand-edit `works.json` at any point — the studio is a convenience, not a dependency.

## Stripping the studio

When the site is done:

```bash
rm admin.html
```

That's it. `index.html` never imports it. Until then it's reachable on the deployed URL by
anyone who guesses the path — it can't modify the site, but rename it to something
unguessable or keep it out of the repo if that bothers you.
