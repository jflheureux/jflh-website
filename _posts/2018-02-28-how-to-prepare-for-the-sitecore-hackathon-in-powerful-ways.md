---
title: How to Prepare for the Sitecore Hackathon in Powerful Ways
tags: [Sitecore, Hackathon]
sharing:
  linkedin: Preparation is key. See how to prepare for the Sitecore hackathon in powerful ways!
---

![Sitecore Hackathon Logo]({{ '/img/2018-02-28-hackathon/Sitecore-Hackathon-logo-small.png' | absolute_url }}){: .center-image }

Preparation is key for a 24 hours hackathon event. You do not want to lose time on setup or tooling. You want to use those precious hours to code and be creative.

In this post, I cover how I prepared for the [2018 Sitecore hackathon](http://www.sitecorehackathon.org/sitecore-hackathon-2018/).

<!-- more -->

## Beer

![Beer](https://upload.wikimedia.org/wikipedia/commons/8/87/Brewdog-hardcore-ipa.jpg){:style="width:400px;"}{: .center-image }

You need to have fun! This is the most important aspect of a hackathon. If it were not fun, nobody would participate. The topics are made public at 7pm EST. It is the perfect time to crack open a few beers while starting the project in powerful ways.

#### Checklist:
* You have enough for you and your teammates.
* It is in the fridge at least 12 hours before it starts.

<div style="height:60px;"></div>

## Coffee

![Coffee](https://images.pexels.com/photos/591651/coffee-bean-drink-grind-591651.jpeg?w=940&h=650&auto=compress&cs=tinysrgb){:style="width:400px;"}{: .center-image }

As the Hackathon starts at 7pm for me, it means I cannot get sleep between my work day and the event. After a full work day and a long night of coding, I will need something strong to stay awake.

#### Checklist:
* Refill the coffee stocks.
* Make sure you have enough filters as well.

<div style="height:60px;"></div>

## Food

![Lunches](https://c1.staticflickr.com/6/5099/5529530102_e58e6a2ae2_b.jpg){:style="width:400px;"}{: .center-image }

Your brain need food to think sharp and code wisely. Prefer homemade meals over fast food.

#### Checklist:
* Do not waste time cooking during the hackathon.
* Make an additional serving at dinner time the days before the hackathon.
* Microwave when you are hungry.

<div style="height:60px;"></div>

## Tools

Nothing is more frustrating than having problems with your tools while you are in a rush.

#### Checklist:
* Update your tools to the latest version. This includes Visual Studio, its extensions, Visual Studio Code.
* Make sure NuGet, nodejs, and npm are working fine.

<div style="height:60px;"></div>

## GitHub Repository

![GitHub](https://cdn4.iconfinder.com/data/icons/iconsimple-logotypes/512/github-512.png){:style="width:400px;"}{: .center-image }

This year, the organization team is providing each team a GitHub repository with boilerplate folders, documentation, Visual Studio solution, and instructions about where to place everything. This is really helpful.

#### Checklist:
* Clone the repository on your local machine.
* Read all the markdown files.
* Take the lead by creating your Visual Studio projects in advance. Even add Unicorn from NuGet and prepare your configuration files if you plan to use it.

<div style="height:60px;"></div>

## Sitecore Instances

This year's Sitecore version requirement is the bleeding edge 9.0 Update-1 release. It requires a Solr server for xConnect to work. With the help of [SIF-less](https://bitbucket.org/RAhnemann/sif-less), I installed 3 instances named hackathondev, hackathonstaging, and hackathonprod. I will work in the dev instance, use the staging one to test my Sitecore package, and use the prod one to record my video which will include a clean package installation.

#### Checklist:
* Ensure you have a Solr instance running. Install it with [Jeremy Davis' script](https://jermdavis.wordpress.com/2017/10/30/low-effort-solr-installs/) if needed.
* Install a few Sitecore instances using your preferred method.

<div style="height:60px;"></div>

## Sitecore Rocks

[Sitecore Rocks](https://marketplace.visualstudio.com/items?itemName=JakobChristensen.SitecoreRocks) is a fast way to work in Sitecore from Visual Studio. It is an extension. If you are new to it, I highly recommend watching [Dheer Rajpoot's presentation at SUGNCR](https://www.youtube.com/watch?v=hPEKGesrerg).

Sitecore Rocks cannot connect to a Sitecore 9 instance by default. You have to modify the Sitecore web.config to allow the connection. Follow [Rob Ahnemann's solution](http://www.rockpapersitecore.com/2017/10/sitecore-rocks-with-sitecore-9/) to make it work.

#### Checklist:
* Ensure Sitecore Rocks is installed.
* Modify the web.config of your Sitecore instances.
* Add the connections to your Sitecore instances in Sitecore Rocks.

<div style="height:60px;"></div>

## Sitecore Rocks VS Code

[Sitecore Rocks VS Code](https://marketplace.visualstudio.com/items?itemName=refactor11.sitecore-rocks-vscode) is the little brother of the original Sitecore Rocks extension. It is only for VS Code and was released during Sitecore Symposium 2017. It comes handy when developing with Sitecore JSS. It requires a Sitecore package to be installed in your instances. If you hope JSS will be an entry category for this year's hackathon like me, I recommend you to install it.

#### Checklist:
* Install VS Code if not already done. What are you waiting for to use the best code editor out there!
* Install the Sitecore Rocks VS Code extension.
* Install the [Sitecore.ContentDelivery.zip](https://ci.appveyor.com/project/JakobChristensen/sitecore-contentdelivery/build/artifacts) package in your Sitecore instances.
* Add the connections to your Sitecore instances.

<div style="height:60px;"></div>

## Sleep

A good preparation must have a lot of sleep in the days before a hackathon. Now that everything is ready, get as many hours of sleep as possible.

Enjoy your Sitecore hackathon!