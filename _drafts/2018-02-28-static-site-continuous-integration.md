---
title: Static Site Continuous Integration
excerpt: Using TravisCI to build and test a free GitHub Pages static site
date: 2018-02-28 00:00:00
header:
  image: /assets/images/githubpages-travis.png
  image_description: GitHub Pages + TravisCI
tags: ci automation
published: true
---

In my [last post](http://dailyherold.net/2018/01/25/return-of-the-static-site/), I hinted at the "technical side" of how I got my GitHub Pages static site up and running again. Quick list of [things](https://github.com/dailyherold/dailyherold.github.io/compare/9c02c509e2a6b85a7c749248751ede1266d2e6eb...master) I did:

- Re-acquainted myself with [GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) and [Jekyll](https://jekyllrb.com/docs/home/) documentation.
- Added [TravisCI builds](https://travis-ci.org/dailyherold/dailyherold.github.io) for continuous integration goodness.
- Made the README somewhat meaningful, and added the TravisCI build badge! This still needs lots of improvement.
- Poured over the Minimal Mistakes theme [documentation](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/), which was very solid.
- Then spent a [lot of time](https://github.com/dailyherold/dailyherold.github.io/commit/e1ba743f0482d3af5413243c64ff35f3bde7441b) figuring out how to transition my four year old repo to a new external theme.
- Fix build errors along the way!

Also on that list should be the hour or so of figuring out why Ruby didn't like my Fedora 27 laptop (*[hint:](https://twitter.com/dailyherold_/status/953093008035074049)* `sudo dnf install rubygem-json`). But I digress, the point of this article is to talk about the TravisCI work I did, because I think it's pretty simple, effective, and cool. 

### Why Continuous Integration for a Simple Static Site?

Why not!? Continuous integration involves pushing changes daily to a mainline branch (e.g. `master`), and triggering an automated build with tests for each change. [Jez Humble](https://twitter.com/jezhumble) would take it a step further and say that if there is a build failure, it must also be repaired within [ten minutes](https://martinfowler.com/bliki/ContinuousIntegrationCertification.html)!

That sounds like a lot for a single author, slow moving, non-critical repository, powering a personal blog. Let's break it down into two parts: git workflow, and automated builds with tests.

#### Git Workflow

The workflow component to Jez's definition is, at least to me, good practice no matter what size repo, or how critical it is. Harness the power of git: commit frequently, figure out interactive rebasing, backup work by pushing to remote branches, integrate code daily, resist long running branches, etc.

#### Automated Builds with Tests

The automated builds with tests component can get a bit tricker. Every project is different and tons of tools exist to help with that goal. For GitHub Pages, the only way to receive remote build feedback is by pushing changes to your [publishing branch](https://help.github.com/articles/about-github-pages-and-jekyll/). I don't like being surprised by build failures with [limited visibility](https://help.github.com/articles/troubleshooting-github-pages-builds/), so let's bring in TravisCI to run builds with tests for any changes pushed to any remote branch!

### TravisCI

Since the repo behind this site is open source, I decided that using TravisCI would be a no brainer for some simple build feedback whenever I push changes. TravisCI provides free builds for public repos, and in general has an excellent GitHub integration. The basic setup instructions are as follows:

1. Link your TravisCI account to your GitHub account
2. "Flick the repository switch on" from your TravisCI profile
3. Add a `.travis.yml` to your repository
4. Trigger your build with a `git push`


#### .travis.yml

For my site, I created the following `.travis.yml` config file:

{% highlight yaml %}
language: ruby
rvm:
- 2.4
script:
- bundle exec jekyll build
- bundle exec github-pages health-check
{% endhighlight %}

I am using Ruby 2.4 (instead of 2.1) as my Fedora 27 workstation had it installed by default, and I want to test with the same version I develop on. If I could figure out the exact runtime GitHub Pages uses for published sites, I would change my local install and `.travis.yml` to use that version. I'm okay taking the "risk" on 2.4 however given that documentation said that Ruby >= 2.1 is [required](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#requirements).

`bundle exec jekyll build` is the command [recommended by GitHub](https://help.github.com/articles/viewing-jekyll-build-error-messages/#configuring-a-third-party-service-to-display-jekyll-build-error-messages), so let's go ahead and use that since it should be similar to what is used for publishing branch builds (I'm guessing `serve` is used which implicitly runs `build`).

`bundle exec github-pages health-check` I added myself after reading through the GitHub Pages Ruby Gem [README](https://github.com/github/pages-gem). Specifically, it "checks your GitHub Pages site for common DNS configuration issues." This is great, as I am using a custom domain, and it's an excellent additional test for my automated builds.


### Troubleshooting

Debugging failures from TravisCI builds is pretty transparent -- you know exactly what commands were run, the runtime environment, and can see the full console output. Hopefully any changeset that passes TravisCI will also be successful when published by GitHub Pages.

I had one instance where that wasn't the case, and involved me tweaking the site's [config.yml](https://github.com/dailyherold/dailyherold.github.io/commit/f22047043bbcb7810570b5b8cce625f3260441cf) so that the theme was pulled in remotely each build. I read through GitHub's [troubleshooting guide](https://help.github.com/articles/troubleshooting-github-pages-builds/), which was decent, but I ended up just trying different things related to the theme's config. Documentation around using `remote_theme` vs `theme`, when including a theme's gem [explicitly](https://github.com/dailyherold/dailyherold.github.io/blob/master/Gemfile#L3), in the context of GitHub Pages wasn't very clear.

### Result

I get sweet sweet [automated builds](https://travis-ci.org/dailyherold/dailyherold.github.io) kicked off on every push that tests my site's dependencies, templating, config, and DNS resolution for my custom domain. I think this is great, but some of you may be thinking...

> Why do all of this when I can just run `bundle exec jekyll build` and `bundle exec github-pages health-check` locally before pushing straight to `master`?

That is a very good point, and a very viable workflow when working on simpler projects with less contributors. Once more people start getting involved, or the build process becomes more complicated, maybe involving certain build steps/tests that can _only_ be run remotely, defining the build process as code and automating the running of builds with tests on every branch change becomes very helpful.

On a less philosophical note, I am lazy, and would get frustrated cleaning out all my cached gems often. Not to mention other variables introduced by my local workstation that could affect local validation. TravisCI's ephemeral build environments are much cleaner!


## Questions or comments?

Hope you enjoyed the read, if any questions or comments reach out via any of the links on the left!

