---
title: Sitecore Instance Manager Export Hangs While Assembling ZIP Package
tags: [Sitecore, SIM Tool]
---

Have you ever experienced a never ending instance export in the SIM Tool? It happened to me today. It was always stuck at the last step, Assembling zip package.

After trial and error, I discovered it was likely a security access issue and that the SIM tool didn't have write access to the root of my D drive.

<!-- more -->

## Solution

The solution is to specify the SIM Tool "Instances Root Folder" or one of its subfolder as the export folder. For me, it is `C:\inetpub\wwwroot\`. It works because the SIM Tool is already granted full access to this folder at installation time.