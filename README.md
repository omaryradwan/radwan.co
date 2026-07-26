# radwan.co

Holding page for **radwan/co** (legal name: Radwan Company), a holding company
for B2B and DTC subbrands.

Distinct from [radwancompany.com](https://radwancompany.com) — that is
radwan&co, the professional services firm — but it deliberately shares that
site's brand system.

## Stack

None. `index.html` is a single self-contained static file with inlined CSS and
JS. There is no build step, no dependencies, and nothing to install. Open the
file in a browser, or serve the directory:

```sh
python3 -m http.server 8000
```

## Deployment

Cloudflare Pages project `radwan-co`, Git-connected to
`omaryradwan/radwan.co`. Pushing to `main` deploys; there is no build step, so
Pages just serves this directory.

- Build command: _(none)_
- Build output directory: `/`
- Production branch: `main`

`_headers` is applied by Pages at the edge; `sitemap.xml` and `robots.txt`
allow full indexing.

`www.radwan.co` redirects to the apex via a zone-level **Redirect Rule**, not a
`_redirects` file — Cloudflare Pages does not support domain-level redirects in
`_redirects` ([docs](https://developers.cloudflare.com/pages/configuration/redirects/)).
The rule is `www.radwan.co/*` → `https://radwan.co/$1`, 301.

## Brand

Inherited from the radwan&co brand sheet, with the separator glyph changed from
`&` to `/`.

| Token      | Value     | Use                                        |
| ---------- | --------- | ------------------------------------------ |
| paper      | `#f4f2ec` | primary surface                            |
| paper-card | `#faf8f3` | elevated surface (the email pill)          |
| ink        | `#0d0d0d` | letters on light surfaces                  |
| muted      | `#6e6759` | meta text — holds WCAG AA on both surfaces |
| accent     | `#0000fb` | the separator, links, focus rings          |

Type: Roboto Mono 500 for the lockup and eyebrows, Newsreader 300 for body.

**Rule 01** — `radwan` and `co` share one color; the `/` is the accent. An
all-one-color lockup is forbidden. **Rule 02** — the lockup sits on white,
paper, medium grey, dark grey, or black, never on a blue surface.

The full lockup is `radwan/co`; the monogram is `r/`.

## Assets

`favicon.svg` and `wordmark.svg` contain outlined Roboto Mono glyphs — **do not
edit the letter paths by hand.** The `/` in each is a drawn parallelogram sized
so its perpendicular stroke matches the letter stems (239 units in wordmark
space, 6.5 in favicon space) at a 22.8° slant.

The rasters are generated from those two SVGs:

```sh
sed 's/ rx="14"//' favicon.svg > /tmp/touch-icon.svg
rsvg-convert -w 180 -h 180 /tmp/touch-icon.svg -o apple-touch-icon.png

rsvg-convert -w 1600 wordmark.svg -b '#f4f2ec' -o wordmark.png

rsvg-convert -w 2000 wordmark.svg -o /tmp/mark.png
magick /tmp/mark.png -trim +repage -resize 720x /tmp/mark720.png
magick -size 1200x630 xc:'#f4f2ec' /tmp/mark720.png -gravity center -composite og.png
magick og.png -depth 8 -strip og.png
```

## Contact address

The email is split across `data-mail-user` / `data-mail-domain` and reassembled
with `U+0040` at copy time, so the served HTML never contains the literal
`user@domain` string. This defeats Cloudflare's email obfuscation rewrite and
naive scrapers without hurting accessibility. Keep it that way if you edit the
markup.
