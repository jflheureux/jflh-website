---
title: Sitcore JSS CLI Secret Commands
tags: [Sitecore, JSS]
sharing:
  linkedin: Think you know all the Sitecore JSS CLI commands? Here are 2 secret commands you probably missed!
---

Troubleshooting an issue by looking at the source code of a module can sometimes be very entertaining. Here are the 2 hidden gems I discovered by looking at the Sitecore JavaScript Services (JSS) CLI source code.

<!-- more -->

## Is There an Elephant in the Room?

```
> jss elephant-in-the-room
JSS CLI is running in global mode because it was not installed in the local node_modules folder.
   _.-- ,.--.
   .'   .'    /
   | @       |'..--------._
  /      \._/              '.
 /  .-.-                     \
 (  /    \                     \
 \\      '.                  | #
  \\       \   -.           /
   :\       |    )._____.'   \
    "       |   /  \  |  \    )
            |   |./'  :__ \.-'
            '--'

 Won't someone please address me?
```

## What's the Password?

```
> jss whats-the-password
JSS CLI is running in global mode because it was not installed in the local node_modules folder.
Why it's b, of course.
```