---
title: Introducing Sitecore XM Cloud Extensions
tags: [Sitecore, XM Cloud]
---

I am very happy to introduce my latest project: [Sitecore XM Cloud Extensions](https://github.com/jflheureux/Sitecore-XM-Cloud-Extensions). A browser extension that improves Sitecore XM Cloud user experience.

<!-- more -->

I had the idea to build a browser extension since I first [connected my local XM Cloud instance to Sitecore XM Cloud Pages by setting a local storage value](https://doc.sitecore.com/xmc/en/developers/xm-cloud/connect-xm-cloud-pages-to-your-local-xm-cloud-instance.html). A few weekends ago, I built the extension using React, [Chakra UI](https://chakra-ui.com/), and [Sitecore Blok](https://blok.sitecore.com/). Then, I submitted it to the Chrome Web Store.

Today, the extension has been approved on the Chrome Web Store and is available to install: [https://chromewebstore.google.com/detail/sitecore-xm-cloud-extensi/pkonhbljhhbhbdjkacgmafheaebijmjh](https://chromewebstore.google.com/detail/sitecore-xm-cloud-extensi/pkonhbljhhbhbdjkacgmafheaebijmjh)

It is a work in progress with only one feature for the moment:

- Connecting XM Cloud Pages to your local XM Cloud instance.

When you are on Sitecore XM Cloud Pages and you open the extension, it shows you the status of your local instance connection:

![Sitecore XM Cloud Extensions - Disconnected]({{ '/img/2023-11-21-xmc-extension/disconnected.png' | absolute_url }}){: .center-image }

When you click the "Connect" button, it allows you to enter the URL of your local CM instance:

![Sitecore XM Cloud Extensions - Connecting]({{ '/img/2023-11-21-xmc-extension/connecting.png' | absolute_url }}){: .center-image }

And when you are connected, it shows tips to ensure a successful experience:

![Sitecore XM Cloud Extensions - Connected]({{ '/img/2023-11-21-xmc-extension/connected.png' | absolute_url }}){: .center-image }

In the future, I want to add other features that would help XM Cloud developers and users in their day to day tasks. Your suggestions are welcome. I also want to add support for more browsers.

You can check the project GitHub repository: [https://github.com/jflheureux/Sitecore-XM-Cloud-Extensions](https://github.com/jflheureux/Sitecore-XM-Cloud-Extensions)

I hope this extension will be useful to you.
