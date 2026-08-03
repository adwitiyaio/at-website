# Adwitiya → Intiqo

The single-page notice served at **[www.adwitiya.io](https://www.adwitiya.io/)**, telling
visitors that Adwitiya Technologies is now Intiqo and sending them on to
[intiqo.com](https://www.intiqo.com).

It is a hand-written static page: no framework, no build step, no package manager, no
dependencies to install. Two files and two images. The only JavaScript is a single line
that keeps the copyright year current.

## Files

| Path                       | Purpose                                                      |
| -------------------------- | ------------------------------------------------------------ |
| `index.html`               | The whole page — markup, meta tags, inline SVG icons          |
| `styles.css`               | All styling; design tokens live in `:root` at the top         |
| `assets/intiqo-logo.svg`   | Current brand mark, also used as the favicon                  |
| `assets/adwitiya-logo.png` | Retired brand mark, shown desaturated in the lockup           |
| `CNAME`                    | Pins the custom domain. **Deleting this breaks the domain.**  |

## Working on it locally

No tooling required — open `index.html` in a browser. To exercise it over HTTP instead:

```bash
python3 -m http.server 8000    # then visit http://localhost:8000
```

The page renders fine offline; the only external request is the Google Fonts stylesheet,
and it falls back to system fonts if that fails.

## Deployment

GitHub Pages, configured as **Deploy from a branch** (`main`, `/` root). There is no
Actions workflow and none is needed — Pages watches `main` directly.

**Pushing to `main` publishes the site.** A commit alone does nothing; the push is the
trigger. Builds usually go live in under a minute. HTTPS is enforced, and the certificate
covers both `www.adwitiya.io` and `adwitiya.io`.

To check the most recent build:

```bash
gh api repos/adwitiyaio/at-website/pages/builds/latest --jq '.status, .error.message'
```

## Conventions worth knowing before editing

**Typography.** The page uses Lato, the Adwitiya brand face, loaded from Google Fonts.
Only weights **400 and 700** are requested, and Lato ships no 500 or 600 at all — those
values silently fall back or get synthesised, so stick to 400 and 700. If you need Light,
add `300` to the font URL first.

**Colour.** Every colour is a custom property in `:root` in `styles.css`. The blues are
sampled from the Intiqo logo's own gradient (`#0ba6de` → `#083a66`), so changing the logo
means revisiting those tokens.

**Contrast.** Body and secondary text must clear 4.5:1 against `--paper` (WCAG AA).
`--muted` (`#5f7180`) is already near that floor at roughly 4.6:1 — lighten it and the
small print stops being accessible.

**Layout.** One centred column capped at 660px, with a single breakpoint at 640px where
the logo lockup stacks vertically. The lockup arrow rotates in place there, trading its
width for vertical margin so the SVG does not get squashed.

**Motion.** One shared `rise` keyframe, staggered by animation delay. Everything is
disabled under `prefers-reduced-motion`. Please keep it that way, and avoid adding looping
animation — the page is meant to be calm.

**Images.** The logos are decorative (`alt=""`); the lockup wrapper carries the
`aria-label` that describes the transition, so screen readers hear it once rather than
twice.

## Where to change the common things

| To change              | Edit                                                            |
| ---------------------- | --------------------------------------------------------------- |
| Destination site       | `.cta` href, the `.note` text, and `og:url` in `index.html`      |
| Contact address        | `.footer-contact` — both the `mailto:` href and the link text    |
| Headline / body copy   | The `.hero` section in `index.html`                              |
| Colours, spacing, type | `:root` and the matching block in `styles.css`                   |
| Copyright year         | Nothing — it is set from the current date at runtime             |

## This repository is public

The repo and its full history are visible to anyone. Keep it that way:

- No credentials, API keys, tokens, analytics IDs, or internal-only URLs — not in the
  files and not in commit messages. Anything committed once stays in the history even if
  a later commit removes it.
- `connect@adwitiya.io` is intentionally public; it is rendered on the live page.
- Before adding an image, check what is embedded in it — exported assets can carry author
  names, file paths, or GPS data in their metadata.
