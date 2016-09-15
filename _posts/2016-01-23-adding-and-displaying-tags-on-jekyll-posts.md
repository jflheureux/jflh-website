---
title: Adding and Displaying Tags on Jekyll Posts
tags: Jekyll Tags
---
When I started this blog, I knew I wanted to tag the posts and use tag based navigation. Unfortunately, my [Jekyll][jekyll] theme didn't display post tags so I modified it tonight. This post covers how to add tags to posts and how to display them.

<!-- more -->

## Defining Tags

Tags must be defined in the post [YAML Front Matter][yaml-front-matter].

{% highlight yaml %}
---
layout: post
title:  A Post Without Tags
---
{% endhighlight %}

There are two alternatives to define the tags. The first one is the simplest. Just add the tags, separating them by at least one space. This is suitable when the tags are single words.

{% highlight yaml %}
---
layout: post
title:  A Post With One Word Tags
tags:   FirstTag SecondTag    ThirdTag
---
{% endhighlight %}

If the tags are made of more than one word, the second alternative is needed. The tags must be in a YAML array, separated by at least one space.

{% highlight yaml %}
---
layout: post
title:  A Post With Multi Word Tags
tags:   [ First Tag, Second Tag,    Third Tag ]
---
{% endhighlight %}

## Displaying Tags

The tag array is available in the `post.tags` variable.

### Determining Whether Tags Should Be Displayed

The `size` attribute of the tags array can be checked to determine whether they should be displayed.

{% highlight liquid %}
{% raw %}
{% if post.tags.size > 0 %}
  # Display tags
{% endif %}
{% endraw %}
{% endhighlight %}

### Displaying the Singular or Plural Form

When there's only one tag, "Tag:" should be displayed instead of "Tags:". All this code must be on a single line unless spaces will be added in place of new lines.

{% highlight liquid %}
{% raw %}
Tag{% if post.tags.size > 1 %}s{% endif %}:
{% endraw %}
{% endhighlight %}

### Displaying Tags

The [Liquid template engine][liquid] used by Jekyll offers a lot of [filters][liquid-filters]. The `sort` filter is useful to sort the values of an array and the `join` filter can be used to separate each entry of an array by a string. It is useful to separate each tag by a comma.

{% highlight liquid %}
{% raw %}
{{ post.tags | sort | join: ", " }}
{% endraw %}
{% endhighlight %}

### Assemble Everything

Once assembled, the code to display the tags is not very complicated.

{% highlight liquid %}
{% raw %}
{% if post.tags.size > 0 %}
  Tag{% if post.tags.size > 1 %}s{% endif %}:
  {{ post.tags | sort | join: ", " }}
{% endif %}
{% endraw %}
{% endhighlight %}

The result for this post is:

{% highlight text %}
Tags: Jekyll, Tags
{% endhighlight %}

## Conclusion

With tagged posts, pages to list posts by tag would be nice. Jekyll doesn't generate those by default, but plugins exist to do it. Unfortunately, plugins are not allowed on GitHub Pages. The workaround would be to generate the Jekyll site locally with plugins enabled, and commit only the output to the GitHub repository.

[jekyll]: https://jekyllrb.com
[yaml-front-matter]: http://jekyllrb.com/docs/frontmatter
[liquid]: https://github.com/Shopify/liquid
[liquid-filters]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers