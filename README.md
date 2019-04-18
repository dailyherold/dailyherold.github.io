dailyherold.io
===========

[![Build Status](https://travis-ci.org/dailyherold/dailyherold.github.io.svg?branch=master)](https://travis-ci.org/dailyherold/dailyherold.github.io)

This is my personal blog, [dailyherold.io](https://dailyherold.io). I write about stuff I am interested in, and what I would like to share with our community.

All content created by John Paul Herold and powered by [GitHub Pages](https://pages.github.com/).

## Requirements

1. Ruby dev package (e.g. on Ubuntu, run `apt install ruby-dev`)
2. [bundler](https://bundler.io/)

## Build/Serve

- `bundle exec jekyll build`: build and generate static site, the same command run by Travis CI.
- `bundle exec jekyll serve --drafts`: serve site with drafts locally, navigate to http://127.0.0.1:4000 to view.

## Test

- A successful static site generation via the build steps above is the majority of "testing" today.
- `bundle exec github-pages health-check`: run by Travis CI to check for common DNS configuration issues when using a custom domain ([docs](https://github.com/github/pages-gem#health-check)).

## Write

- Serve the site locally with drafts enabled (see Build/Serve section).
- Create or edit drafts under `_drafts/`.
- Move file to `_posts/` with `YEAR-MONTH-DAY-title.md` filename.
- Modify the post's [front matter](https://jekyllrb.com/docs/front-matter/), using the doc link and other posts for reference. Pay attention to the `published` variable, in case you don't want to publish the post on the next build.
- Merge PR with new post into `master` which is the only branch used for static site generation.

## TODO

- Add [favicons](https://github.com/mmistakes/minimal-mistakes/issues/949) to site.
- Use rvm to pin a version of Ruby, same as CI builds, for local development workflow.
