# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is a Jekyll static site (theme: `minima`) for Jackmanimation — a personal/team site combining a blog (`_posts/`) with a catalog of tabletop games designed over the years (`games/`). It is primarily used as a test site (per README.md, "test website for Python testing of application and modules"), so content and structure are simple.

## Commands

Build and run locally (requires Ruby/Bundler):
```
bundle install
bundle exec jekyll serve   # live-reload dev server, default http://localhost:4000
bundle exec jekyll build   # outputs static site to _site/
```

There is no test suite, linter, or CI config in this repo — the `Jenkinsfile` is a deploy pipeline (see below), not a test runner.

## Architecture

- **Jekyll site config**: `_config.yml` sets the theme (`minima`), site metadata, and an `exclude` list that keeps non-site files (Jenkinsfile, Gemfile, README, LICENSE, vendor/) out of the built `_site/` output. When adding new top-level files that shouldn't be published, add them to this `exclude` list.
- **Content model**:
  - `index.md` — homepage, uses the `home` layout.
  - `_posts/` — dated blog posts (`YYYY-MM-DD-title.md`), standard Jekyll front matter (`layout: post`, `title`, `date`, `categories`).
  - `games/index.md` — a catalog/table of demo games designed 2002–2018, with links out to individual game subpages and external publications/blog posts.
  - `games/<game-name>/` — per-game subdirectories (currently `games/bopbop/`) each with their own `index.md`, plus supporting pages like `rules.md`/`update.md`, an `assets/` folder for images, and a `files/` folder for downloadable content (PDFs, zips). Follow this same subdirectory pattern when adding a new game.
- **Deployment**: `Jenkinsfile` defines a 3-stage pipeline — Toolchain (installs ruby/bundler via apt/gem if missing), Build (`bundle config set path 'vendor/bundle'` then `bundle exec jekyll build`), Deploy (rsyncs `_site/` to `/var/www/html/jackmanimationtest` on the target host, deleting stale files). This targets a home-lab Jenkins server, not a hosted CI service.
