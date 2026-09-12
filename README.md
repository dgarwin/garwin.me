# garwin.me

A personal site styled like it is 1997. No framework, no JavaScript, no images,
nothing to compile.

- `index.html` — the site. 1997 styling on a modern layout: the look is deliberate
  (black ground, Impact orange, cyan links, yellow header bands, outset borders, a
  starfield of asterisks) but the layout underneath is grid, flexbox, and a media
  query, so it stacks on a phone. No images and no JavaScript. Keep the styling; the
  layout is fair game.
- `plain/index.html` — the earlier minimal version of the same content, kept at
  `/plain/` and linked from the footer.
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
copies `index.html`, `CNAME`, and the `oh-no/` directory into `public/` and publishes
that,
because the Netlify UI for this site still has Publish directory = `public` left
over from when the site used Hugo. If you clear that UI setting to `.`, the
staging step can be dropped and the repo root published directly.

**If you add a file to the site, add it to the copy list in `netlify.toml`** —
otherwise it won't reach the deploy.

## Editing it

Edit `index.html` directly.
