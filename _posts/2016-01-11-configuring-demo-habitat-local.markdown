---
title: Configuring demo.habitat.local
tags: Sitecore Habitat
---
After following all the steps to [setup Sitecore Habitat][setupHabitat] on a local machine, there are 2 site root items in the `\sitecore\Content\` node of the Sitecore content tree: Habitat and Demo.

![Habitat and Demo site root items]({{ site.url }}{{ site.baseurl }}/img/2016-01-11-configuring-demo-habitat-local/two-site-root.png)

<!-- more -->

The Habitat item loads fine at [http://habitat.local/][habitatLocal]

The Demo item is served by the demo website which uses the `demo.habitat.local` hostname:

{% highlight xml %}
<site name="demo" hostName="demo.habitat.local" rootPath="/sitecore/content/demo" ... />
{% endhighlight %}

However, loading [http://demo.habitat.local/][demoHabitatLocal] is failing because of DNS entry not found for that domain. This is because the Sitecore installer only created a hosts file entry and an IIS binding to `Habitat.local`. To fix that, two things are needed:

1. A hosts file entry for demo.habitat.local:

       # C:\Windows\System32\drivers\etc\hosts

       127.0.0.1 Habitat.local
       127.0.0.1 demo.Habitat.local

2. An IIS binding for demo.habitat.local:

   1. In IIS Manager, right-click on the Habitat.local website ![Habitat.local website in IIS]({{ site.url }}{{ site.baseurl }}/img/2016-01-11-configuring-demo-habitat-local/iis-site.png) and select "Edit Bindings...".

      ![demo.Habitat.local page]({{ site.url }}{{ site.baseurl }}/img/2016-01-11-configuring-demo-habitat-local/before-demo.png)

   2. Click the "Add..." button.
   3. Enter "demo.Habitat.local" in the "Host name" field.

      ![demo.Habitat.local page]({{ site.url }}{{ site.baseurl }}/img/2016-01-11-configuring-demo-habitat-local/adding-demo.png)

   4. Click the "OK" buton.

      ![demo.Habitat.local page]({{ site.url }}{{ site.baseurl }}/img/2016-01-11-configuring-demo-habitat-local/after-demo.png)

   5. Click the "Close" button.

The [http://demo.habitat.local/][demoHabitatLocal] should now load as expected. A Google search page should be displayed because this site's utility is to provide a demo script starting point.

The `\src\Feature\Demo\specs\Mockup of External page.feature` Specflow file of the Habitat repository explains the utility of the page:

{% highlight text %}
Feature: Mockup of External page

In order to start a demo story from an external page
As a technical presales consultant
I want to be able to show a mockup of an external page, e.g. search engine page with adword links to a campaign on the website
{% endhighlight %}

[setupHabitat]: https://github.com/Sitecore/Habitat/wiki/01-Getting-Started
[habitatLocal]: http://habitat.local/
[demoHabitatLocal]: http://demo.habitat.local/
