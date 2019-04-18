dailyherold.io
===========

[![Build Status](https://travis-ci.org/dailyherold/dailyherold.github.io.svg?branch=master)](https://travis-ci.org/dailyherold/dailyherold.github.io)

This is my personal blog, [dailyherold.io](https://dailyherold.io). I write about stuff I am interested in, and what I would like to share with our community.

All content created by John Paul Herold and powered by [GitHub Pages](https://pages.github.com/).

## Requirements

1. Ruby dev package (e.g. on Ubuntu, run `apt install ruby-dev`)
2. [bundler](https://bundler.io/)

## Build

- `bundle exec jekyll serve` to build and serve the static site locally (http://127.0.0.1:4000).
- Otherwise you can simply build with `bundle exec jekyll build`, which is the same command run by Travis CI.

## Test

- A successful static site generation via the build steps above is the majority of "testing" today.
- In addition, the following command is run on Travis CI to test the DNS setup for the site's custom domain: `bundle exec github-pages health-check`.
