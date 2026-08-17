# alvarie.github.io

My portfolio, built as a command-line terminal. Live at **https://alvarie.github.io**.

Visitors can type commands (`about`, `experience`, `skills`, …) or click the same
commands from the menu — both paths run the identical handler, so there's only ever
one copy of the content.

## Editing the content

Everything the site says lives in one place: the constants and the `commands` object
near the top of the `<script>` block in `index.html`. To update the site, edit that
object, commit, and push. GitHub Pages redeploys on its own.

The pieces you'll touch most:

| What | Where in `index.html` |
|---|---|
| Roles and bullets | `EXPERIENCE` array |
| Skill buckets | `SKILLS` array |
| Projects | `PROJECTS` array |
| Intro paragraph | `ABOUT_BODY` constant |
| Email / LinkedIn / GitHub | `EMAIL`, `LINKEDIN`, `GITHUB` constants |
| Command list in `help` | `HELP_ROWS` array |

Search the file for `TODO(alex)` — those mark the spots waiting on content.

## Files

```
index.html             the whole site: markup, styles and logic in one file
404.html               terminal-styled not-found page, served by GitHub Pages
resume.pdf             linked by the `resume` command — replace to update
og.png                 1200x630 social preview for LinkedIn/Slack unfurls
favicon.svg / .ico     browser tab icon
apple-touch-icon.png   iOS home-screen icon
robots.txt             allows everything, points at the sitemap
sitemap.xml            one URL; exists so crawlers don't have to guess
assets/                self-hosted JetBrains Mono (no third-party font request)
```

## Sharing a section

Every content command has a URL: `https://alvarie.github.io/#experience` boots
straight into that section (and skips the boot animation, since someone following
a section link came for the section). The address bar tracks whatever command you
last ran, so it's always safe to copy.

`?theme=light` or `?theme=dark` pins the theme and survives a reload. It's only
written once you change the theme yourself — otherwise a copied link would
override the recipient's OS preference.

Which commands get a link is the `LINKABLE` array. `resume` is deliberately not
in it: landing on a URL that immediately opens a PDF trips popup blockers.

## Deploying

The repo must be named exactly `alvarie.github.io` for GitHub to serve it as the
user site at the bare domain.

1. Push to `main`.
2. Settings → Pages → Source: *Deploy from a branch* → `main` / `/ (root)` → Save.
3. Live in about a minute.

## Design notes

Choices that look like omissions but aren't:

- **No framework and no build step.** The site is one interactive component with
  static content. A bundler would add maintenance for zero benefit at this size.
- **No `localStorage` or `sessionStorage`.** Command history is an in-memory array.
  Nothing is stored on a visitor's machine — the URL is the only state the page
  keeps, which is why the theme rides in a query parameter rather than storage.
- **The printable CV is generated, not written twice.** `buildPrintable()` runs the
  same command handlers the terminal uses and drops the result into a hidden block
  that only appears in print, so paper and screen can't drift apart. Without it,
  Cmd+P captured just the one screenful inside the scrolling terminal box.
- **JSON-LD is built from the `EMAIL`/`LINKEDIN`/`GITHUB` constants** for the same
  reason — a changed address can't go stale in a second place.
- **Lookups keyed by visitor input go through `own()`.** A bare `obj[word]` returns
  inherited members, so `cat constructor` used to resolve to `Object` and throw.
- **The font is self-hosted**, so the page never blocks on a third-party font server.
- **Anything a visitor types is HTML-escaped** before it's echoed back.
- **Accessibility:** output is an `aria-live` region so screen readers announce it,
  the prompt is a real `<input>`, links are real `<a>` tags, and the boot animation
  is skipped entirely under `prefers-reduced-motion` (or by pressing any key).

## Running it locally

Any static server works, since there's nothing to build:

```bash
python3 -m http.server 8777
```

Then open http://localhost:8777.
