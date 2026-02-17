# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Jekyll-based academic website using the **al-folio** theme. Personal site for Stijn J. de Graaf, deployed to GitHub Pages (gh-pages branch) from the main branch.

## Build & Development Commands

```bash
# Local development (Docker - recommended)
docker compose up

# Local development (manual)
bundle exec jekyll serve --watch --port=8080

# Production build
bundle exec jekyll build

# Format check
npx prettier . --check

# Format fix
npx prettier . --write

# Deploy (manual, has safety checks)
./bin/deploy
```

Prettier uses `@shopify/prettier-plugin-liquid` with `printWidth: 150` and `trailingComma: "es5"` (see `.prettierrc`).

## Architecture

**Content collections** live in prefixed directories:
- `_pages/` — Static pages (about, cv, publications) with permalink frontmatter
- `_posts/` — Blog posts (`YYYY-MM-DD-title.md` naming convention)
- `_news/` — Homepage announcements
- `_projects/` — Portfolio items
- `_books/` — Book reviews
- `_bibliography/papers.bib` — BibTeX publications (managed by jekyll-scholar)

**Templating & styling:**
- `_layouts/` — Liquid layout templates (`.liquid` extension)
- `_includes/` — Reusable Liquid partials
- `_sass/` — SCSS stylesheets; `_variables.scss` and `_themes.scss` control theming

**Data-driven content** in `_data/`:
- `cv.yml`, `coauthors.yml`, `venues.yml`, `repositories.yml`, `socials.yml`

**Custom Ruby plugins** in `_plugins/` handle cache busting, Google Scholar/Inspire HEP citations, CDN library downloads, and external post fetching.

**Main configuration** is `_config.yml` (~586 lines) — controls site metadata, plugin settings, third-party library CDN URLs with integrity hashes, collection definitions, and feature flags.

## CI/CD

GitHub Actions workflows in `.github/workflows/`:
- `deploy.yml` — Auto-builds and deploys on push to main
- `prettier.yml` — Enforces code formatting
- `broken-links.yml` — Link validation via lychee
- `axe.yml` — Accessibility testing
- `codeql.yml` — Security analysis

## Key Dependencies

- **Ruby/Jekyll**: Gemfile manages 24+ plugins including jekyll-scholar (bibliography), jekyll-imagemagick (responsive images), jekyll-minifier, jekyll-terser
- **npm**: Only used for Prettier formatting
- **Docker**: Dockerfile and docker-compose.yml for containerized development
- **ImageMagick**: Required for responsive image generation (WebP at 480/800/1400px)
