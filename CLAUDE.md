# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

Empty Hugo scaffold (`hugo new site`). No theme, no layouts, no content, no git repo yet. `hugo.toml` still holds the default `baseURL`/`title` placeholders. Expect to create most directories on first real work.

Hugo v0.165.0 (extended features not verified — check `hugo version` output for `extended` before relying on SCSS/Sass pipelines).

## Commands

```bash
hugo server -D          # dev server at :1313, includes drafts, live reload
hugo                    # build into public/
hugo --gc --minify      # production build
hugo new content/posts/my-post.md   # new page from archetypes/default.md
hugo mod get -u         # update Hugo Modules (if theme installed as a module)

# WSL2: view from a Windows browser at http://localhost:1313
hugo server -D --bind 0.0.0.0 --baseURL http://localhost:1313/
```

Development runs under WSL2. Plain `hugo server` binds to the WSL loopback only; localhost forwarding usually carries it through to Windows, but `--bind 0.0.0.0` is the dependable form and also exposes the site on the WSL IP (`hostname -I`) for other devices.

No test suite, linter, or package manager config exists. Hugo builds are the only verification step; a broken template fails the build.

## Layout resolution

Hugo picks templates by lookup order, not imports. When a page renders wrong, the cause is almost always which template won, not the template's contents:

- `layouts/` in the project overrides the same path in `themes/<name>/layouts/`.
- Section pages resolve `layouts/<section>/list.html` before `layouts/_default/list.html`; single pages resolve `layouts/<section>/single.html` before `layouts/_default/single.html`.
- `layouts/_default/baseof.html` is the outer shell every other template fills via `{{ block "main" . }}`.

## Directory roles

- `content/` — Markdown; top-level folders become sections and drive URL structure.
- `layouts/`, `themes/` — templates (project wins over theme).
- `assets/` — files processed by Hugo Pipes (`resources.Get`, fingerprinting, SCSS).
- `static/` — copied verbatim to the site root, no processing.
- `data/` — YAML/JSON/TOML read via the `.Site.Data` map.
- `i18n/` — translation tables for `{{ i18n "key" }}`.
- `archetypes/default.md` — front matter template for `hugo new`.
- `public/` — build output; regenerate, never edit. `hugo` does not delete stale files, so remove `public/` before a release build if pages were renamed.
