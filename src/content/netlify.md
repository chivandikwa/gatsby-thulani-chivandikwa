---
layout: post
title: How I made my blog with Netlify
image: img/bird2.jpg
author: Thulani S. Chivandikwa
date: 2019-03-06T10:00:00.000Z
tags: [Netlify, Blog]
draft: false
---

## I chose Netlify :heart_decoration:

When I decided to start my blog I did a thorough research to see what of the many available options would suite my needs. I looked at Blogger and WordPress which I had used before, medium which I myself am a regular at and a bunch off others. In the end I decided to go with something where I had full control. That is when I discovered two tings, Gatsby, a framework based on React and Netlify, everything you need to build fast, modern websites: continuous deployment, serverless functions, and so much more. The beauty of it all being that I could use them both together, with a lot of other things I appreciate like GraphQL and TypeScript. I will try not to be too excited about it all and focus only on Netlify.

You can head over to the [Netlify website](https://www.netlify.com/) to find out what it is all about as I will not cover everything but only the things that I enjoyed.

In a nutshell I am using Netlify connected to my blog GitHub repo to build changes to master and deploy my website. With the free plan I am able to have my custom domain and HTTPS setup, get instant git integration, get continuos deployment, make preview deploys for my Pull Requests and access some addons like account signup and form fill on the site (<i>the signup and forms I am not using, yet...</i>), global CDN deployments, instant cache invalidation, 100GB bandwidth, unlimited rollbacks and guaranteed SOC2 compliance.

## Proof or it didn't happen

So let's see how I have setup some of this stuff and what it all means. To have the basic setup took me no more than an hour and each time I wanted to enable a new feature it was always a click away.

### Deployments

By default deployments will be made to https://site-name.netlify.com, so in my case https://thulanichivandikwa.netlify.com, I have however also setup a custom domain https://chivandikwa.com. Netlify deploys your site to global infrastructure in seconds with a single simple workflow. The multi-cloud infrastructure ensure that the site is speedy, automated to scale and secure.

<b>Build</b>
I have setup the repository to watch and indicated the build command as the command <code>npm run build</code> in accordance to the project I have setup. I have indicated the public directory from the build and the directory to publish. I am currently using the Ubuntu Xenial 16.04 build image, the default recommended for new sites over Ubuntu Trusty 14.04.
![build-settings](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/build-settings.jpg)

I have setup my production deploys to come from master and for Netlify to build preview deploys from all pull requests. You may wonder why I have pull requests for something only I contribute to. Well, while I do test locally I want to avoid a situation when I push something that will break a build or other issues to master. So instead I have my pull requests watched for preview builds and report that status to GitHub. I have then protected my branch in GitHub to not allow merges to master until Netlify reports a successful build.
![deploy-settings](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/deploy-settings.jpg)
![deploy-notifications](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/deploy-notifications.jpg)
![github-branch-protection](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/github-branch-protection.jpg)
![run-checks](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/run-checks.jpg)
![checks-complete](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/checks-complete.jpg)

To further make my site optimized I have enabled pretty urls, bundling and minification of css and JavaScript and lossless compression of all images. To also allow search engine crawlers to see all the content on my site I ahve enabled prerendering with Netlify.
![optimization](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/optimization.jpg)

<b>GitHub status badges</b>
Netlify has support for github status badges, something I immediately took advantage of.
![status-badge-setup](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/status-badge-setup.jpg)
![status-badge](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/status-badge.jpg)

<b>Split testing</b>
Netlify has a split testing option that allows one to deploy multiple branches serving different features to a subset of users. For instance if you want to test out a new change you can roll it out to as small group of your users first. I am not actively using but appreciate the out of the box availability. I am not sure if this feature is sticky, I hope it is.
![split-testing](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/split-testing.jpg)

> <b>Update</b> It is a good thing that I have setup pull requests on this repo and the preview deploys in Netlify. Later I decided to add dependabot to my repo to automatically update my npm packages and I do get a number of pull requests being created, some which may indeed cause breaking changes. It is nice to know when merging that a build has succeeded. If you have not heard of [dependabot](https://github.com/marketplace/dependabot-preview) it is now owned by GitHub/Microsoft and currently in preview, meaning it is free to try out. Watch this space and I will probably blog about it.
