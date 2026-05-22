# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static documentation site for Nineva Studios' Unreal Engine, Unity, and Godot plugins. Content is plain Markdown rendered client-side by Docsify — there is **no build step**. The repo also contains a small Firebase Hosting config and a Jenkins pipeline that deploys to https://docs.ninevastudios.com.

A companion `AGENTS.md` covers the same ground for other agents; keep the two files consistent if either is updated.

## Common commands

- **Local preview**: `docsify serve docs` (install once with `npm i -g docsify-cli`). The site is served as-is from `docs/`.
- **Deploy**: `firebase deploy` against project `nineva-documentation` (see `.firebaserc`). In CI this is invoked by `Jenkinsfile` using `$FIREBASE_TOKEN`; manual deploys generally aren't needed.

There are no tests, no linter, no package.json — don't add them unless explicitly asked.

## Architecture notes (the non-obvious parts)

- **Single-page site, runtime-rendered.** `docs/index.html` is the only HTML shell. It loads Docsify from CDN and configures it inline via `window.$docsify`. Everything else is Markdown files that Docsify fetches and renders client-side. Edits to `.md` files take effect on next page load — no rebuild.
- **Routing is path-based, no `.md` extension.** Internal links use forms like `ue-plugins/admob-unreal` (or `#ue-plugins/admob-unreal` on the home page). Never link with a `.md` suffix — Docsify won't resolve it.
- **Search index is explicit.** `search.paths` in `docs/index.html` is a hand-maintained allowlist. When adding a doc that should be searchable, add its route to that array (otherwise it's reachable but not indexed).
- **Navigation is a single file.** `docs/_navbar.md` is the global navbar/sidebar source. New top-level plugin pages need an entry here, otherwise users can only reach them via direct link or the home page table.
- **Home page table is hand-maintained.** `docs/README.md` contains an HTML `<table>` of plugin tiles with icons from `docs/icons/`. When adding/removing a major plugin, update both `_navbar.md` and this table.
- **Icons vs. screenshots.** Tile icons live in `docs/icons/`; in-page screenshots live in `docs/<engine>-plugins/images/`. Keep new images alongside the docs that reference them.
- **Docsify plugins in use** (loaded via CDN in `index.html`): `docsify-tabs` (for `<!-- tabs:start -->` blocks), `docsify-copy-code`, `prism-csharp` / `prism-gdscript` for syntax highlighting, the built-in search plugin, and `docsify-darklight-theme` with `#8930B1` as the accent.
- **Firebase config is trivial.** `firebase.json` just maps the `docs/` directory as the hosting root — no rewrites or redirects. Don't expect server-side behavior.

## Content conventions

- Use relative links without `.md` for cross-doc references (matches Docsify routing).
- Code samples for Unreal usually use C++ (Prism C# highlighter renders it acceptably); Unity uses C#; Godot uses GDScript — the matching Prism components are already loaded.
- Tabbed sections use docsify-tabs syntax (`<!-- tabs:start -->` / `#### **Tab title**` / `<!-- tabs:end -->`).
