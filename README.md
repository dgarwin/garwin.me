# garwin.me

A plain personal site. No framework, no JavaScript, nothing to compile.

- `index.html` — the whole site (markup plus a small embedded stylesheet)
- `oh-no/index.html` — a period-accurate 1997 version of the same content, served at
  `/oh-no/`. HTML 3.2 and CSS1 only: table layout, `<font>`, `<center>`, body color
  attributes, no images and no JavaScript. Nothing in it postdates 1997, so don't
  "fix" it with flexbox, media queries, or `max-width`.
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
stages the site files into `public/` and publishes that, because the Netlify UI
for this site still has Publish directory = `public` left over from when the
site used Hugo. If you clear that UI setting to `.`, the staging step can be
dropped and the repo root published directly.

The staging copies every top-level file and directory except `netlify.toml`,
`README.md`, and dotfiles, so adding a page, an image, or a subdirectory needs no
config change. It asserts `index.html` landed, so a mistake fails the build instead
of publishing an empty site.

## Editing it

Edit `index.html` directly.
