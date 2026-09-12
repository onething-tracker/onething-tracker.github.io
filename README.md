# onething-tracker.github.io

Source for the OneThing Tracker site, built with [Hugo](https://gohugo.io/) and the
[PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

Live at <https://onethingtracker.app>.

## Setup

The theme is a git submodule, so clone with it:

```bash
git clone --recurse-submodules https://github.com/onething-tracker/onething-tracker.github.io.git
```

Already cloned without it:

```bash
git submodule update --init --recursive
```

Requires Hugo v0.165.0 or later.

## Local development

```bash
hugo server -D          # dev server at :1313, includes drafts, live reload
hugo --gc --minify      # production build into public/
```

Under WSL2, bind explicitly so a Windows browser can reach it:

```bash
hugo server -D --bind 0.0.0.0 --baseURL http://localhost:1313/
```

## Adding a post

```bash
hugo new content/posts/my-post.md
```

Front matter comes from `archetypes/default.md`. Posts with `draft: true` are
excluded from production builds; drop the flag or set it to `false` to publish.

## Deployment

Every push to `main` triggers `.github/workflows/hugo.yml`, which builds the site
and publishes it to GitHub Pages. No manual step. The Pages source is set to
"GitHub Actions" — do not switch it back to branch-based publishing, which would
serve an empty site because `public/` is intentionally not committed.

## Configuration

Site-wide settings live in `hugo.toml`: `baseURL`, `title`, the nav menu, and the
PaperMod options under `[params]`.
