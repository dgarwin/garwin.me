# garwin.me

A plain, single-page personal site. No build step, no framework, no JavaScript.

- `index.html` — the whole site (markup plus a small embedded stylesheet)
- `david.jpg` — the photo
- `CNAME` — custom domain for GitHub Pages
- `netlify.toml` — tells Netlify to publish the repo root as-is

## Viewing it locally

Open `index.html` in a browser, or serve the directory:

```sh
python3 -m http.server
```

Then visit http://localhost:8000.

## Editing it

Edit `index.html` directly. To replace the photo, overwrite `david.jpg` with a
square image (the page renders it at 110px, so ~480px square is plenty).
