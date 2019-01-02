---
title: Helix and Sitecore JavaScript Services (JSS)
tags: [Sitecore, Symposium, Helix, JSS, ESLint, React, Talk, Conference]
sharing:
  linkedin: If you missed my Sitecore Symposium session on JavaScript Services and Helix, you should read my blog post and watch the recording!
---

The last week was the most awesome of my carreer so far. Thanks to the [Sitecore Symposium 2018](https://symposium.sitecore.com/) organization who selected my paper and my employer, [Coveo](https://www.coveo.com), who supported me in this adventure. Not only I attended the Sitecore Symposium and Sitecore MVP Summit, I also presented a session on [Helix](https://helix.sitecore.net/) and [Sitecore JavaScript Services](https://jss.sitecore.net/) in front of a full room of 250 people. This blog post summarizes the session and its takeaway.

<!-- more -->

## Helix In a Front-End Project, Really?

Not everyone is sharing my point of view regarding Helix in JSS projects. Some people prefer to give all the freedom to front-end developers. In my opinion, a Sitecore JSS project is a software project like any other one. And it can be pretty complex. It is proven that a good code architecture with a clear dependency flow is important to keep the technical debt low and allow faster and easier maintenance, additions, and changes. In the Sitecore world we already have Helix to describe these principles and conventions. Why not using it with JSS as well?

## The Session

If you were not at the Symposium last week, were still sleeping or preferred to attend the session on Sitecore 9.1 and .Net Core ;), I recorded my session and published it on YouTube for you.

<div class="videoWrapper">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/jrBlmyaAOBo?rel=0" frameborder="0" allow="encrypted-media" allowfullscreen></iframe>
</div>

## Start a JSS Application Using the Helix Code Architecture

I have also published an Helix JSS app template named **react-helix** on my [jflheureux/jss-app-templates](https://github.com/jflheureux/jss-app-templates) GitHub repository. It contains the same boilerplate code and style guide sample application than the original JSS react app template. However, it uses the Helix code architecture. You can use it with the `jss create` command like this:

```
jss create <name-of-your-app> react-helix --repository jflheureux/jss-app-templates
```

## Next Steps

Of course, Helix covers more than the code architecture. For the moment, this is the first aspect I have focused my work on. The next things to address are:

### Unit Tests

Move the modules' code in a `code` folder and create a `tests` folder in each module for the unite tests.

### `data` and `sitecore` Folders

These folders contain the fake data used in disconnected mode and the Sitecore item definitions. They should be moved inside the module folders.

### Sitecore Items

Sitecore items are currently created in a folder of the project name under each section (templates, content, renderings...). According to Helix, they should be in their layer and module folders instead. It might be hard to achieve this as it requires changes in the packaging and deployment logic. I am still unsure if it requires changes in the JSS CLI which is not open source. It might be easier to work in Sitecore-first mode to achieve this goal.

### Implicit Dependencies

In the JSS Styleguide, the Bootstrap stylesheet is included in the page by a project layer file but the Bootstrap CSS classes are used by the feature layer modules. This is a dependency flow violation caused by an implicit dependency. I do not think ESLint can be used to detect such dependencies. Manual care must be applied to ensure a clean implicit dependency flow.