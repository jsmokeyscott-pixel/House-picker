# 🏡 House Designer

Try siding, trim, roof, garage door, front door, shutter, and paint colors on **any photo of the house**, or on a photo of any room, right in the browser.

| Before | After |
|---|---|
| ![Original photo](docs/before.png) | ![Sage siding, black roof, dark oak garage doors with windows](docs/after.png) |

**Live app:** `https://<your-username>.github.io/house-designer/` (replace with your link once Pages is on)

---

## What it does

- **Any photo:** upload a picture of the house or any room. The new house is preloaded so it works right away.
- **Outside parts:** siding, trim, roof, garage doors, front door, shutters, foundation, and porch/deck. Each part has its own color list (roof shingles, siding colors, deck stains, and so on).
- **Inside parts:** walls, accent wall, cabinets, floor, and trim/ceiling.
- **Custom parts:** add anything else, like gutters or a mailbox.
- **Wood looks** with grain for doors, cabinets, floors, and decks.
- **Garage door windows:** short, long, grid, or arched. They follow the door's angle in the photo.
- **Compare:** save looks and see them side by side. Saves stay on the device.
- **Download** the finished picture as a JPG.
- **Private:** photos never leave the phone. No server, no account, no upload.

![App screenshot](docs/app.png)

---

## How to use it

1. **Add a photo**, or use the one already loaded.
2. Choose **Outside** or **Inside a room**.
3. **Pick a part**, like Siding.
4. **Mark it** on the photo:
   - **Tap to fill:** tap the part, and tap again on any spots it missed.
   - **Brush / Eraser:** touch up the edges.
   - **Mark corners:** tap the 4 corners of a door, window, or shutter in any order. Use this for garage doors if you want windows.
5. **Pick a color.** Change it as many times as you like and the marked area updates.
6. **Save to compare**, then try another combination.

**Tips**
- Photos taken in even daylight work best. Hard shadows break walls into separate fill zones.
- If a fill spills onto the wrong thing, hit **Undo**, slide **Fill reach** down, and tap again.
- Brick, stone, and busy patterns don't fill well. Use the brush on those.

---

## Repo layout

```
house-designer/
├── index.html      # the entire app: HTML, CSS, JS, and default photo inline
├── .nojekyll       # tells GitHub Pages to serve files as-is (no Jekyll build)
├── README.md
└── docs/
    ├── before.png  # README images only, not used by the app
    ├── after.png
    └── app.png
```

There's no build step and no dependencies. The app makes **zero outside network requests**: no external fonts, scripts, or CDNs. Once the page loads, it runs fully offline.

---

## Deploy to GitHub Pages

### Click-through

1. On GitHub, make a new **public** repo called `house-designer`.
2. **Add file → Upload files.** Drag in everything from this folder, including the `docs` folder and `.nojekyll`. Then **Commit changes**.
   - `.nojekyll` is a hidden file. If your file picker hides it, skip it. The app works without it.
3. Go to **Settings → Pages**.
4. Set **Source** to `Deploy from a branch`, set **Branch** to `main` and `/ (root)`, then click **Save**.
5. Wait 1–2 minutes and refresh. Your link shows at the top of the Pages screen.
6. Paste that link into the **Live app** line at the top of this README.

### Command line

```bash
git init house-designer && cd house-designer
# copy index.html, README.md, .nojekyll and docs/ into this folder
git add .
git commit -m "House Designer"
git branch -M main
git remote add origin https://github.com/<your-username>/house-designer.git
git push -u origin main
```

Then turn on Pages (steps 3–4).

### Updating

Replace `index.html`, commit, and push. Pages redeploys on its own in about a minute. If a phone keeps showing the old version, pull to refresh or add `?v=2` to the link.

---

## Speed

- GitHub Pages serves everything through its own global CDN, so the link loads fast anywhere with no extra setup.
- The whole app is one file of about 350 KB, most of which is the default house photo. That's one request and done.
- Uploaded photos are scaled down to 1400px on the long side so tapping and recoloring stay quick on phones.

To make the first load even lighter, swap in a smaller default photo (see below).

---

## Changing the default photo

The default photo is embedded in `index.html` as base64 in the `IMG` constant:

```js
const IMG="data:image/jpeg;base64,....";
```

Make a base64 string from your JPG:

```bash
base64 -w0 photo.jpg        # Linux
base64 -i photo.jpg         # macOS
```

```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("photo.jpg"))   # Windows
```

Paste it in place of the old string. Two things to know:

- The preloaded garage door outline (`MASK` and `DEF_QUADS`) is fit to the current photo. With a new default photo, delete the garage-door preload block in `loadPhoto()`, or just mark the doors by hand with **Mark corners**.
- Keep the photo under about 1 MB for fast loads on cell data.

---

## Where it works

| Opened from | Saved looks persist | Download button |
|---|---|---|
| GitHub Pages or any web host | Yes | Yes |
| The HTML file opened directly on a phone or PC | Yes (most browsers) | Yes |
| Claude app preview or other sandboxed previews | No, resets on close | Hidden (use a screenshot) |

Saves are per device and per browser. Looks saved on one phone don't show up on another.

It works on current Chrome, Edge, Safari, and Firefox, on Android, iPhone, and desktop.

---

## Troubleshooting

- **404 on the Pages link:** the file must be named exactly `index.html` and sit in the repo root, not inside a folder.
- **Old version still showing:** this is browser cache. Pull to refresh or add `?v=3` to the link.
- **Add a photo doesn't open the camera:** give the browser camera and photo permission in the phone's settings.
- **Colors look off from the store sample:** screens vary. Use this to narrow choices, then confirm with a real sample.

---

## Privacy note

If the repo is public, the default house photo inside `index.html` is public too. If you'd rather not share it, make the repo private (Pages on private repos needs a paid GitHub plan) or swap in a different default photo.
