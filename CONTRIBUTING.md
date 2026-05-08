# Contributing to OrbitGuard AI

Thanks for considering a contribution. This project is small and single-file by design — keep changes focused and reversible.

## Ways to contribute

- **Bug reports** — open an issue with steps, expected vs. actual, browser/OS, and a console log if relevant.
- **Feature requests** — open an issue describing the use case before writing code. Drive-by features without a discussion are likely to be closed.
- **Pull requests** — small, single-purpose, with a clear "what & why" in the description.

## Development setup

No build step. Clone and serve `index.html` from any static server:

```bash
git clone https://github.com/astroraul/orbitguard.git
cd orbitguard
python3 -m http.server 8000
# open http://localhost:8000
```

## Conventions

- **Single file**: keep all UI code in `index.html`. Split only when the file would otherwise stop being readable in one editor window.
- **No new dependencies via package manager**. CDN imports are fine if pinned to a specific version.
- **Vanilla JS**. No frameworks, no transpilers.
- **CSS**: keep selectors short (the existing code uses 2–3 letter class names — match that style). New rules go near related rules, not at the bottom.
- **Commits**: imperative mood, ≤72 char subject. Reference the issue if there is one (`fixes #12`).

## Pull request checklist

Before opening a PR:

- [ ] Tested in the latest Chrome and Firefox.
- [ ] No regressions in the live demo flow (globe loads, TLEs fetch, inspector opens on click).
- [ ] No new console errors.
- [ ] README/SCREENSHOTS updated if user-visible behavior changed.

## Reporting security issues

Do **not** open a public issue for security problems. See [`SECURITY.md`](SECURITY.md).

## Code of conduct

Participation is governed by the [`Code of Conduct`](CODE_OF_CONDUCT.md).
