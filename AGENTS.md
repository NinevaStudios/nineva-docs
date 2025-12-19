# AGENTS.md

## Project overview
- Nineva Studios documentation site built with Docsify.
- Content is Markdown under `docs/` and published as a static site.
- Hosting is configured via Firebase (`firebase.json`) with `docs/` as the public root.

## Key entry points
- Site shell and Docsify config: `docs/index.html`
- Home page content: `docs/README.md`
- Navigation: `docs/_navbar.md`
- Custom styling: `docs/custom.css`
- Static 404 page: `docs/404.html`

## Content structure
- Unreal Engine docs: `docs/ue-plugins/`
- Unity docs: `docs/unity-plugins/`
- Godot docs: `docs/godot-plugins/`
- Shared assets: `docs/icons/` and `docs/**/images/`

## Docsify configuration notes
- Theme: `docsify-darklight-theme` with custom accent color.
- Plugins: search, tabs, copy-code, Prism language support (C#, GDScript).
- Navbar is enabled via `loadNavbar: true` and uses `docs/_navbar.md`.
- Search paths are explicitly listed in `docs/index.html`.

## Common tasks
- Add a new doc page: create a Markdown file under `docs/**/` and link it from `docs/_navbar.md` (and optionally the home page).
- Update the landing page: edit `docs/README.md`.
- Update styles: edit `docs/custom.css` and, if needed, adjust theme settings in `docs/index.html`.
- Update search: add new routes to the `search.paths` array in `docs/index.html`.

## Local preview
- Docsify can be served locally from `docs/` (for example via `docsify serve docs`).
- No build step is required; the site is static and rendered client-side.

## Content conventions
- Use relative links without `.md` extensions for Docsify routing (for example `ue-plugins/admob-unreal`).
- Keep images inside the relevant `images/` folder next to the docs they illustrate.
