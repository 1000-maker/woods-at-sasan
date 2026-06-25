# Woods at Sasan — Trade Partner Brief 2026

A single-page, scroll-driven microsite. Static site, no build step.

## Structure

```
.
├── index.html              # markup, CSS, and the scroll/canvas logic (~80 KB)
├── assets/
│   ├── hero/               # 92 hero scroll-scrub frames (1920×1080 JPEG)
│   ├── ghar/               # Pustak Ghar scroll frames (PNG)
│   └── content/            # section images + logo
├── vercel.json             # cache headers + clean URLs
├── .gitattributes          # treats images as binary
└── .gitignore
```

The page loads images by URL (`window.HERO_FRAMES`, `window.GHAR_FRAMES`,
`window.IMG`, `window.LOGO` in the head). They were previously inlined as
base64; extracting them dropped `index.html` from ~33 MB to ~80 KB and lets the
browser cache and parallel-load each frame.

## Deploy to Vercel

No framework, no build command.

**Option A — Git (recommended)**
1. Push this folder to a GitHub/GitLab/Bitbucket repo.
2. In Vercel: New Project → import the repo.
3. Framework Preset: **Other**. Build Command: *(none)*. Output Directory: `./`.
4. Deploy.

**Option B — CLI**
```bash
npm i -g vercel
vercel          # preview
vercel --prod   # production
```

## Local preview

```bash
python3 -m http.server 8000
# open http://localhost:8000
```
Open `index.html` directly via `file://` will work for layout but image paths
are relative, so prefer a local server.

## Known performance caveat (read before launch)

The hero is 92 full-HD JPEG frames totalling ~20 MB. Even loaded from a CDN,
that is a heavy first impression on mobile. See deployment notes for options
(downscale frames, reduce frame count, or convert to a compressed video/WebP
sequence). This repo ships the assets as-is — the optimisation is a content
decision, not a packaging one.
