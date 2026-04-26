# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Personal academic homepage for Sangjune Park (PhD candidate, UNIST 3D Vision and Robotics Lab), deployed via GitHub Pages at https://jjuke.github.io. It is a single-page Jekyll site — there are no blog posts or collections.

## Local development

```bash
gem install bundler:2.3.3
bundle install
bundle exec jekyll serve   # http://localhost:4000
```

Ruby 2.7.2 is pinned in `.ruby-version`. Jekyll `~> 3.4` is pinned for GitHub Pages compatibility — do not bump major versions casually.

There is no test suite, linter, or build script beyond `jekyll serve` / `jekyll build`. The generated `_site/` is committed (it ships the deployed output).

## How content edits actually work

Almost all visible page content lives in `_config.yml`, not in markdown or HTML. The site is essentially a single template (`_layouts/default.html`) that loops over arrays in `_config.yml`. Typical edits:

- **Add/edit a publication** → append an entry under `pub_3dvision:` in `_config.yml`. Each entry needs `background_url`, `name`, `project_page_url`, `paper_url`, `arxiv_url`, and `animation` (e.g. `fadeInLeft`). The image referenced by `background_url` goes in `pub_materials/`.
- **Update CV link** → change `about_box_button.link` (PDF lives in `pdf/`).
- **Update career timeline** → edit `career_list:` array.
- **Update bio** → `about_content`.
- **Update profile photo** → replace `images/me.JPG` (path set by `profile_image`).
- **Counters under "Honor"** → `hon_counters` (papers/honors counts).

`index.md` is a near-empty stub that just selects `layout: default`. Don't put content there — it won't render.

After editing `_config.yml`, restart `jekyll serve` (config changes are not hot-reloaded).

## Layout & sections

`_layouts/default.html` renders, in order: sidebar (profile + nav + social) → About → (commented-out Tech Skills) → Publications → Honors → Career. Section navigation uses `data-section` / `data-nav-section` attributes wired up in `js/main.js`. Single-page scroll, no routing.

CSS pipeline: `sass/style.scss` compiles to `css/style.css` (committed). Theme colors are CSS variables injected from `_config.yml` (`primary`, `color_1..6`, `color_red`, `color_navy`) into a `<style>` block in the layout — change palette in `_config.yml`, not in CSS.

## Files to ignore / leave alone

- `_config_old.yml`, `_layouts/default_old.html` — backups from the previous theme; keep around but don't edit.
- `cal.py` — unrelated personal expense calculator, not part of the site build.
- `prepros-6.config` — config for the Prepros app (used historically to compile Sass); not required if you compile via Jekyll.
- `_site/` — generated output, but it is checked in. Regenerate with `bundle exec jekyll build` before committing content changes so the deployed output stays in sync.
- `.gitignore` only excludes `.DS_Store`.

## Conventions worth respecting

- Inline styles are heavy in `_layouts/default.html` (e.g. the CV button block). When tweaking visual elements, match the existing inline-style approach rather than refactoring into the SCSS — the layout mixes both.
- Large blocks of HTML and `_config.yml` are commented-out (alternative sections, Korean prose from earlier versions). Treat them as historical reference; don't delete unless asked.
