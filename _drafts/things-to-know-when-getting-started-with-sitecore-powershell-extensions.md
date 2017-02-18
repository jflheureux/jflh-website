---
title: Things to Know When Getting Started With Sitecore PowerShell Extentions
tags: [Sitecore, Sitecore PowerShell Extensions, PowerShell]
sharing:
  twitter: My experience on getting stared with Sitecore PowerShell Extentions # Max 116 characters
  facebook: My experience on getting stared with Sitecore PowerShell Extentions
  linkedin: My experience on getting stared with Sitecore PowerShell Extentions
---

Last week I started learning PowerShell and the [Sitecore PowerShell Extensions](https://marketplace.sitecore.net/en/Modules/Sitecore_PowerShell_console.aspx) module. In under 2 hours, I wrote a [script to add new renderings to an item presentation details](http://source.coveo.com/2017/02/16/automate-adding-a-coveo-search-box-in-a-page-using-sitecore-powershell-extensions/).

In the process, I learned the hard way by trial and error and by reading the documentation. After my first learning session, I had a chat with the authors of the module. They were very generous and answered a lot of my beginner questions. This post is my way of giving back to the community by highlighting what you should know before getting started and some things to pay attention to.

<!-- more -->

## Installation

Very easy

## Trying Commands

Console
Not case sensitive for commands/attributes/variables

### Help for the Get-Item Commands

In another documentation section
Get-Item -Path master: -Id ...
Get-Item . -Id ...


## Your First Script

ISE

### Always specify the database in commands

In case your script is run in the context of the core/web DB

## Where to Save Your Scripts

Modules
### New module beforehand
### New module at same time by right clicking

## Conclusion