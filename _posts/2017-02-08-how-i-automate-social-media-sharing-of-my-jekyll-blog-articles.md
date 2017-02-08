---
title: How I Automate Social Media Sharing of my Jekyll Blog Articles
tags: [Jekyll, Automation, Twitter, Facebook, LinkedIn, IFTTT]
sharing:
  twitter: How I Automate Social Media Sharing of my @jekyllrb Blog Articles # Max 116 characters
  facebook: How I Automate Social Media Sharing of my Jekyll Blog Articles
  linkedin: How I Automate Social Media Sharing of my Jekyll Blog Articles
---

I was tired of manually sharing my blog articles on my Twitter, LinkedIn and Facebook accounts so I decided to automate the process using [Jekyll front matter](https://jekyllrb.com/docs/frontmatter/) variables, RSS feeds, and [IFTTT](https://ifttt.com) applets.

<!-- more -->

## Choose What to Share and How

First, for each post I want to share on social media, I include additional front matter variables.

{% highlight yaml %}
---
title: How I Automate Social Media Sharing of my Jekyll Blog Articles
tags: [Jekyll, Automation, Twitter, Facebook, LinkedIn]
sharing:
  twitter: How I Automate Social Media Sharing of my @jekyllrb Blog Articles
  facebook: How I Automate Social Media Sharing of my Jekyll Blog Articles
  linkedin: How I Automate Social Media Sharing of my Jekyll Blog Articles
---
{% endhighlight %}

Since `twitter`, `facebook`, and `linkedin` are indented, `sharing` becomes an object. This object properties can be accessed using the dot notation `page.sharing.twitter` or the array notation `page.sharing["twitter"]`.

The sharing section is optionnal so is each social media site in the list. If only one of the site has a value, the post will be shared only on that site.

Twitter has a 140 characters limitation and any link is using 23 characters. This restricts the length of your message to 116 characters if you keep one for the space before the link to your post.

## Define an RSS Feed Layout

I need one RSS feed per social media site, so I created a `sharing_feed.xml` file in the `_layouts` folder to avoid repetition.

{% highlight xml %}
{% raw %}
---
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}/</link>
    <atom:link href="{{ page.url | absolute_url }}" rel="self" type="application/rss+xml" />
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% assign posts = site.posts | where_exp: "post", "post.sharing[page.sharing_site]" %}
    {% for post in posts limit:5 %}
      <item>
        <title>{{ post.sharing[page.sharing_site] | xml_escape }}</title>
        <pubDate>{{ post.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
        <link>{{ post.url | absolute_url }}</link>
        <guid isPermaLink="true">{{ post.url | absolute_url }}</guid>
      </item>
    {% endfor %}
  </channel>
</rss>
{% endraw %}
{% endhighlight %}

The important parts are:

* The use of the `page.sharing_site` variable which contains the social media site of the current RSS feed.
* The `posts` variable I assign to only the subset of posts I want to share on that social media site.
* The `limit:5` of the `for` loop to limit the size of the RSS feed to a minimum. I don't plan to create more than 5 posts in a 15 minutes timeframe (IFTTT recipes run every 15 minutes).
* The `<title>` of RSS items contains the text to share on the social media site.
* The `<link>` of RSS items contains the link to your blog post to share on the social media site.

## Create the Feeds

Creating the feeds is very easy with this layout. I created one file per social media site. The `sharing_site` front matter variable value need to match the post front matter variable names.

### Twitter.xml
{% highlight yaml %}
---
layout: sharing_feed
sharing_site: twitter
---
{% endhighlight %}

### Facebook.xml

{% highlight yaml %}
---
layout: sharing_feed
sharing_site: facebook
---
{% endhighlight %}

### LinkedIn

{% highlight yaml %}
---
layout: sharing_feed
sharing_site: linkedin
---
{% endhighlight %}

## Create the IFTTT Applets

In IFTTT, I created one applet for each of the social media site.

### Twitter.xml

* This
  * Service: Feed
  * Trigger: New feed item
  * Feed URL: https://www.jflh.ca/feeds/twitter.xml
* That
  * Service: Twitter
  * Action: Post a tweet
  * Tweet text: {% raw %}{{EntryTitle}} {{EntryUrl}}{% endraw %}

### Facebook.xml

* This
  * Service: Feed
  * Trigger: New feed item
  * Feed URL: https://www.jflh.ca/feeds/facebook.xml
* That
  * Service: Facebook
  * Action: Create a link post
  * Link URL: {% raw %}{{EntryUrl}}{% endraw %}
  * Message: {% raw %}{{EntryTitle}}{% endraw %}

### LinkedIn

* This
  * Service: Feed
  * Trigger: New feed item
  * Feed URL: https://www.jflh.ca/feeds/linkedin.xml
* That
  * Service: LinkedIn
  * Action: Share an update
  * Status: {% raw %}{{EntryTitle}} {{EntryUrl}}{% endraw %}

## Workflow

With that in place, everytime I commit a new blog post, I can fill in the messages I want to share on social media sites. Under 15 minutes, the IFTTT applets are triggered and my post is shared automatically.