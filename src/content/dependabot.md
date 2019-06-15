---
layout: post
title: Dependabot dependency manager
image: img/seal.jpg
author: Thulani S. Chivandikwa
date: 2019-03-15T10:00:00.000Z
tags: [Dependabot, Github, GitHub Tools, GitHub Marketplace, Bots]
draft: false
---

Thanks to a colleague I go to find out about Dependabot, a GitHub tool that helps manage dependencies and keep a repository secure and up to date. It works with most popular languages including .net and JavaScript, you can see a full list [here](https://dependabot.com/#languages).

Thanks to Dependabot being recently acquired by GitHub it is now in Preview and free of charge

### So what does it do?

Dependabot will scan the dependencies looking for updates and open Pull Requests to upgrade each one individually. This is great because you can have your CI pipeline invoked per PR and therefore run tests etc per update in isolation. You can configure the tool to update on a live, daily, weekly or monthly schedule. The first time you enable it depending on how far you are with you npm, nuget, etc updates you may get a bit overwhelmed with PRs, I know for sure at some point my team was hogging the shared TeamCity build agents thanks to this.

> By default dependabot will create 5 PRs per day until you are all caught up. You can manually request more PRs and indeed in the beginning that is how you can get overwhelmed.

Additionally you have control over which branch you want the tool to be watching, default mater. You can also decide to only have the tool look for security updates. You can also setup dependabot to auto add reviewers and/or assignees and if you make use of label in your repo, you can have the tool add labels to the PR.

Dependabot has a dashboard to keep track of/or add to repos you have installed to allowing adding new languages/directory and triggering new checks.

![dependabot-dashboard](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/dependabot-dashboard.jpg)

Let's have a look at an example PR that dependabot opened on the repo for this blog and dissect it.

![dependabot-sample](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/dependabot-sample.jpg)

The PR has a concise description indicating the details of the updated package and a complete changelog (collapsed) and commits history for the target. Where available the compatibility score will be shown. This score is based off the number of PRs for that library update that dependabot created across other repos that passed CI (Projects without CI, or without a previously passing test suite, are excluded from the score).

> PRO TIP: If you click the compatibility score you will be taken to a page where you can see PRs that failed CI and contributed to the score. If you have issues with a breaking change this may be useful to see how other developers may have addressed these problems to be able to merge the PR.

Dependabot will resolve any conflicts in the PR and rebase it accordingly if they are changes in the base branch, master in most cases. For this blog's repo, once a build and preview is deployed successfully the status is reported and merging is allowed. If it makes sense in your scenario you can configure dependabot to automatically merge PRs if CI completes successfully.

Finally, dependabot can also be controlled in the PR conversation through a set actions you can comment on the PR for example <code>@dependabot rebase</code> would trigger a rebase. You can even do interesting things like mark a specific dependency to be ignored going forward and more. I tried out <code>@dependabot badge me</code> to add a dependabot enabled badge to my repo, dependabot reacted with some love and immediately added the badge.

![dependabot-badge-me](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/dependabot-badge-me.jpg)

![branch-success](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/branch-success.jpg)

### Conclusion

This really makes what can otherwise be a tedious and timely task super easy and fast. Dependabot is getting popular, closing in on a million PRs merged. Having been acquired by GitHub it is a clear sign that it is a promising tool and will only get better. There is enough flexibility in this tool to be able to use it in a manner that best suits your needs
