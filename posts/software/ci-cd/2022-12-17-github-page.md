---
layout: post
title:
description: ''
date: '2022-12-17'
categories: CI-CD
note:
mathjax:
mermaid:
p5:
threeJS:
anchor:
publish: true
---

## Why?

As my blog getting bigger, I need it

## How?

### local

[Testing your GitHub Pages site locally with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)

### tailwind

follow [How to use Tailwind CSS with Jekyll on GitHub Pages](https://jekyll.ohsostatic.com/devops/how-to-use-tailwind-css-with-jekyll-on-github-pages)

### webpack

### continuous integration (CI) (TBC)

I should wrote easy test for it

```yaml
name: integration-workflow
run-name: ${{ github.actor }} integrating

on:
  pull-request:
...
```

### continuous deployment (CD)

```yaml
name: deployment-workflow
run-name: ${{ github.actor }} deploying

on:
  push:
    branches:
      - master

jobs:
  github-pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.1
          bundler-cache: true
      - name: Setup Node
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm install
      - name: Build site
        uses: limjh16/jekyll-action-ts@v2
        with:
          enable_cache: true
      - name: Build bundle.js
      - run: webpack
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
```

### workflow (TBC)

* A workflow is ?
* The template as follow

### deploy errors

[Viewing Jekyll build error messages in your pull request](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-jekyll-build-errors-for-github-pages-sites)

[Troubleshooting Jekyll build errors for GitHub Pages sites](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/troubleshooting-jekyll-build-errors-for-github-pages-sites)

## What

## Reference

[Deploy your pull requests with GitHub Actions and GitHub **Deployments**](https://sanderknape.com/2020/05/deploy-pull-requests-github-actions-deployments/)

[Using Jekyll with Tailwindcss on GitHub Pages](https://medium.com/@mehdi.h/using-jekyll-with-tailwindcss-on-github-pages-50c3d8401230)

[What does ubuntu-latest mean for GitHub Actions?](https://stackoverflow.com/questions/69840694/what-does-ubuntu-latest-mean-for-github-actions)

[CI/CD with Github Actions to deploy on Github Pages](https://medium.com/front-end-weekly/ci-cd-with-github-actions-to-deploy-on-github-pages-73e225f8f131#:~:text=At%20the%20root%20of%20your,to%20store%20your%20workflow%20files.)

[Everything you need to know about getting started with GitHub Actions](https://resources.github.com/whitepapers/github-actions-cheat/)

[How to use Tailwind CSS with Jekyll on GitHub Pages](https://jekyll.ohsostatic.com/devops/how-to-use-tailwind-css-with-jekyll-on-github-pages)
