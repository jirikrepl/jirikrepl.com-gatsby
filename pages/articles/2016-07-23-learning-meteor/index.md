---
title: Learning Meteor
date: "2016-07-23T22:40:32.169Z"
layout: post
path: "/2016/07/23/Learning-Meteor/"
category: "Meteor"
description: "Meteor is awesome. A whole platform, not just a front-end JavaScript framework."
---

[Meteor](https://www.meteor.com/) is awesome. A whole platform, not just a front-end JavaScript framework.

[This article](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/) covers history web applications' development.
I have just realized that my app does not need an [API](http://nerds.airbnb.com/wp-content/uploads/2013/11/Screen-Shot-2013-11-07-at-10.29.32-AM.png). API is for third party developers. I don't need a back-end and a front-end divided by an API.

## Things I like about Meteor
I came from PHP and later Symfony with Angular background. Lastly I have worked with Gulp to build my own deployment model.
I just realized that in Meteor (if your app is simple enough) you don't need to use Gulp at all. Meteor give you sophisticated build and deployment process for free.

### Lifereload
When you start your your Meteor app on localhost, your project files are watched for changes. And browser reloads every time you save a file.
That means no more manual browser reloading. This mimic the same behavior with [gulp-watch](https://www.npmjs.com/package/gulp-watch) plugin and [Browsersync](http://www.browsersync.io/). In Meteor it's for free:D

### No script and link tags
In your *HTML* code you don't need to use `<script>` and `<link>` elements link your *JavaScript* and *CSS* files. Just put them in some directory and Meteor link them automatically.

### Minification
You don't need to concat and minify static assets, it is done automatically

### Cache busting
Automatic [cache busting](http://www.adopsinsider.com/ad-ops-basics/what-is-a-cache-buster-and-how-does-it-work/). Meteor adds new hash tag to your \*.js and \*.css files every time you change those files. Then any updated files are freshly downloaded to your browser. 

![](./meteorHashAssets.png)

## Useful Meteor packages
These are packages I maybe will use. I am building multilang web app.

[yogiben:admin](https://atmospherejs.com/yogiben/admin) - A complete admin dashboard solution

[tap:i18n](https://atmospherejs.com/tap/i18n) - A comprehensive internationalization solution for Meteor

[tap:i18n-ui](https://atmospherejs.com/tap/i18n-ui) - User interface for the tap-i18n package

[tap:i18n-db](https://atmospherejs.com/tap/i18n-db) - Internationalization for Meteor Collections

[martino:iron-router-i18n](https://atmospherejs.com/martino/iron-router-i18n) - Router with i18n support. Lang parameters in URL

[iron:router](https://atmospherejs.com/iron/router) - Router for Meteor. Here is [guide](http://iron-meteor.github.io/iron-router/)

## Things to consider
There is an article with great name about intricacies of the framework: [Meteor, the dark side of the moon](https://medium.com/@llaine/meteor-the-dark-side-of-the-moon-f885d8fdbf6a)
And here following discussion on [Crater.io](https://crater.io/posts/8j5iT2Z8mgLcbxkGp).

Deployment should be easy with [Meteor up](https://github.com/arunoda/meteor-up)