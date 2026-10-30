# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Mehdi Nobakht's academic homepage, a Hugo site styled after julianprester.com (one column, monospace body, dotted dividers, dark mode). It replaces the older Jekyll site in `../jekyll/`, whose source now matches the live https://mehdi-nobakht.github.io (all its content is carried over here). No theme and no Node/Tailwind: all templates and CSS are local.

## Commands

```sh
hugo server              # http://localhost:1313, live reload
hugo --gc --minify       # build into public/
```

Live at https://mehdi-nobakht.github.io. `origin` is `Mehdi-Nobakht/Mehdi-Nobakht.github.io`; pushing to `main` runs `.github/workflows/hugo.yml`, which builds and deploys to Pages (Pages source: GitHub Actions; the `github-pages` environment only allows `main`). `master` holds the retired Jekyll output as a backup; never deploy from `../jekyll/`. The workflow pins `HUGO_VERSION`; keep it in step with the local version.

## Structure

- `content/_index.md` is the home page bio; `content/teaching.md` is a plain Markdown page. The top nav is `[menus]` in `hugo.toml`, not page front matter.
- Research page: `content/research.md` holds the statement (and an alias so the old `/publications/` URL redirects); the publication list comes from `data/publications.yaml`, keyed by tab (`journal`, `conference`, `report`, `talk`), rendered as tabs by `layouts/research.html`. `site.Title` is bolded wherever it appears in `authors`. Tab switching is in `assets/js/site.js`; with JS off, all panels show with headings.
- Publications may carry an optional `rank` (e.g. `"CORE 2021: B"`), shown as a small badge after the venue. Below the tabs, `layouts/research.html` renders `data/grants.yaml` (`when` + Markdown `text`) as a dated table, skipped while the file is empty.
- Content was last updated from Mehdi's CV (`~/Documents/Personal/me/myResume/2026-08 Aug/Lecturer in Systems/2026-07-31 Application/Docs-CV/`, LaTeX fragments). Publication metadata (author order, DOIs, pages) was checked against Crossref, which overrides the CV where they differ.
- Service page: `content/service.md` + `layouts/service.html` render groups from `data/service.yaml` as dated tables. A menu entry with `params.requiresData = "<data key>"` is hidden by `_partials/header.html` while that data file is empty; Service uses this.
- News on the home page comes from `data/news.yaml` (newest first, `label` instead of `date` when only a semester/year is known, `text` as a `>-` block so colons and quotes are safe; Markdown links allowed). The home page shows the first `params.newsLimit` items (5); the rest sit in a `<details>` "Show all news…" toggle below them that expands in place, as on julianprester.com. Both tables render through `_partials/news-table.html`. There are no individual post pages or RSS.
- Teaching uses Markdown tables; `assets/css/main.css` hides their header row and styles them like the news table (date column on the left).
- Templates use Hugo's 0.146+ layout scheme: `layouts/baseof.html`, `home.html`, `page.html`, `404.html`, partials in `layouts/_partials/`. SVG icons live in `_partials/icon.html`, keyed by name.
- `assets/` is processed by Hugo (CSS/JS minified and fingerprinted). `static/assets/` is copied as-is so PDF links keep their old `/assets/publications/…` and `/assets/talks/…` URLs.
- External links and PDFs open in a new tab: Markdown links go through `layouts/_markup/render-link.html`, template links check `_partials/opens-new-tab.html` (returns a bool; write `target`/`rel` inline, since attributes returned from a partial get sanitised to `ZgotmplZ`). Footer icon links set `target="_blank"` directly.
- Email is split into `emailUser`/`emailDomain` in `hugo.toml` and assembled into `mailto:` by `assets/js/site.js`, so the address never appears whole in the HTML.
- Dark mode: an inline script in `baseof.html` sets `html.dark` before paint (saved choice in `localStorage`, else system preference); colours are CSS variables switched by `:root.dark`.
