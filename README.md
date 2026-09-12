# garwin.me

A plain, single-page personal site. No framework, no JavaScript, nothing to
compile.

- `index.html` — the whole site (markup plus a small embedded stylesheet)
- `david.jpg` — the photo
- `CNAME` — custom domain (vestigial: from GitHub Pages, unused by Netlify)
- `netlify.toml` — Netlify deploy config

## Viewing it locally

Open `index.html` in a browser, or serve the directory:

```sh
python3 -m http.server
```

Then visit http://localhost:8000.

## Deploying

Netlify serves the site. There is nothing to compile, but the build command
copies `index.html`, `david.jpg`, and `CNAME` into `public/` and publishes that,
because the Netlify UI for this site still has Publish directory = `public` left
over from when the site used Hugo. If you clear that UI setting to `.`, the
staging step can be dropped and the repo root published directly.

**If you add a file to the site, add it to the copy list in `netlify.toml`** —
otherwise it won't reach the deploy.

## Editing it

Edit `index.html` directly. To replace the photo, overwrite `david.jpg` with a
square image (the page renders it at 110px, so ~480px square is plenty).
