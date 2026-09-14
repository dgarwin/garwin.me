# garwin.me

A personal site styled like it is 1997. No framework, no JavaScript, no images,
nothing to compile.

- `index.html` — the site. 1997 styling on a modern layout: the look is
  deliberate (black ground, Impact orange, cyan links, yellow header bands,
  outset borders, a starfield of asterisks) but the layout underneath is grid,
  flexbox, and a media query, so it stacks on a phone. Keep the styling; the
  layout is fair game.
- `plain/index.html` — the earlier minimal version of the same content, kept at
  `/plain/` and linked from the top bar.
- `oh-no/index.html` — a stand-in redirect; `/oh-no/` was the homepage's address
  before it was promoted to the root.
- `CNAME` — the custom domain, read by GitHub Pages.
- `.nojekyll` — skip the Jekyll build; these files are served exactly as they
  are committed.

## Viewing it locally

Open `index.html` in a browser, or serve the directory:

```sh
python3 -m http.server
```

Then visit http://localhost:8000.

## Hosting

GitHub Pages serves this repository directly. There is no build and no deploy
pipeline: **pushing to `master` publishes the site.** It costs nothing, and
HTTPS certificates are issued and renewed automatically.

### Settings

Repository Settings → Pages:

- **Source:** Deploy from a branch → `master`, folder `/ (root)`
- **Custom domain:** `garwin.me` (this is what writes `CNAME`)
- **Enforce HTTPS:** on, once the certificate finishes provisioning

The repository has to stay public for Pages on a free plan; private
repositories need GitHub Pro.

### DNS

These records live wherever the domain's DNS is managed. Pages publishes fixed
IP addresses, so the apex domain works with ordinary A records — no nameserver
move, and **every existing MX and TXT record stays exactly as it is, so email
is unaffected.**

| Record | Name | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `dgarwin.github.io` |

AAAA records are optional, for IPv6: `2606:50c0:8000::153`,
`2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.

### Known limits

- **No server-side redirects.** Pages serves files, so `/oh-no/` is a page with
  a meta refresh and a canonical link rather than a real 301.
- **No control over headers or caching.** Fine for a static site; it would
  matter if custom cache or security headers were ever needed.
- Soft limits are 1 GB of content and 100 GB of bandwidth a month, neither of
  which this site will approach.

An S3 and CloudFront setup for the same site — CloudFormation plus a deploy
workflow — is in this repository's history at commit `cedae1a`, if hosting ever
needs to move somewhere with more control.
