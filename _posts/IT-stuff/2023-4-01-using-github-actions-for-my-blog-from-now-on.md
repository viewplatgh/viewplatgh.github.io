---
layout: post
title: Using github actions for my blog from now on
tag: it-stuff
---

Github actions looks cool. I can't wait for using it even if it's still in 'beta' at the moment.

One benefit to use github actions for my blog I believe is to be able to find more diagnostic information if I make a mistake. I found it hard to know what's wrong if I made a mistake when using the classic Jekyll github page. Looks like github actions' workflow runs history can give me more hints if I make a mistake.

Here is the steps to migrate classic Jekyll gh-pages to github actions:

1. Repository - Settings - Actions - Actions permissions

   Choose "Allow all actions and reusable workflows".

2. Repository - Settings - Pages

   Under "Build and deployment", choose "Github Actions" as "Source".

3. Add the suggested Jekyll github actions workflow

   It's to add a .yml file under .github/workflows folder of the repository. Basically no need to modify the .yml file unless wanting to use a different branch other than `master` to deploy or deleting some verbose comments.

4. Merge my previous `gh-pages` branch, which is `roblao.com` back to `master` which is used for deployment in my .yml file

BTW, I found I can view roblao.com deployment history here: https://github.com/viewplatgh/viewplatgh.github.io/deployments
