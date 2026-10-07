# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Local Development

This is a plain HTML/CSS/JS static site — no build step, no package manager. Serve it with a local HTTP server (required for correct asset paths and font loading):

```sh
python -m http.server 8000
# then open http://localhost:8000
```

Or with Node: `npx http-server -p 8000`

## Architecture

Single-page site (`index.html`) with four full-viewport sections: Landing, About, Projects, Contact. No framework, no bundler.

**CSS split:**
- `css/style.css` — design tokens (CSS custom properties), base styles, animations, dark-mode (`body.inverted`), and the swipe-transition overlay
- `css/style-responsive.css` — breakpoint overrides only (`≤768px` and `769px–100em`)

**JS (`js/script.js`):**
- Swipe transition: adds `body.is-transitioning` on anchor clicks, scrolls after 600 ms, removes class at 1200 ms
- Dark mode: toggles `body.inverted` via the asterisk button (`#color-invert-btn`)
- Email copy: `navigator.clipboard` on `.email-container` click
- `IntersectionObserver` on `#projects` and `#landing` to toggle `.projects-visible` class (skipped when `body.inverted` is active)

**SVG icons** in `assets/` are rendered via CSS `mask-image` — their visible color comes from `background-color`, not `fill`. To tint an icon, set `background-color` on its element.

**Fonts:** Nohemi Light/Regular/SemiBold loaded as `@font-face` from `fonts/*.woff2`.

**Design tokens (`:root` in style.css):** `--dark-grey`, `--light-grey`, `--pastel-orange`, `--ice-blue`, `--white`, `--pastel-red`, `--status-pulse-green`.

**Known issue:** The `IntersectionObserver` `rootMargin` trick for Firefox is commented out and not working — avoid relying on it.
