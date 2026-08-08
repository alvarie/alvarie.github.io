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
| Intro paragraph | `commands.about` |
| Email / LinkedIn / GitHub | `EMAIL`, `LINKEDIN`, `GITHUB` constants |
| Command list in `help` | `HELP_ROWS` array |

Search the file for `TODO(alex)` — those mark the spots waiting on content.

## Files

```
index.html              the whole site: markup, styles and logic in one file
resume.pdf             linked by the `resume` command — replace to update
og.png                 1200x630 social preview for LinkedIn/Slack unfurls
favicon.svg / .ico     browser tab icon
apple-touch-icon.png   iOS home-screen icon
assets/                self-hosted JetBrains Mono (no third-party font request)
```

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
- **No `localStorage` or `sessionStorage`.** Command history is an in-memory array
  and the theme resets on reload. Nothing is stored on a visitor's machine.
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
