---
title: Running Jekyll on Cloud9
tags: Jekyll Cloud9
---
I use [Jekyll][jekyll] to generate my blog on [GitHub Pages][github-pages]. I am very new to both. I also try to do my web deveopment on a Chromebook. For this blog, I choose [Cloud9 IDE][cloud9]. I had to find out how to install, configure and run Jekyll using it.

<!-- more -->

A Google search led me to good starting points:

* [https://help.github.com/articles/using-jekyll-with-pages/][source1]
* [https://docs.c9.io/docs/jekyll][source2]
* [http://jekyllrb.com/docs/github-pages/][source3]

## Setup Jekyll

Ruby is already installed in Cloud9 workspaces so I skipped it.

I already had my GitHub.io repository cloned in my Cloud9 workspace so I did things a bit differently:

1. Install Bundler with `gem install bundler`.
2. Create a `Gemfile` the way Jekyll recommends to do it.
3. Move to my Git working copy folder with `cd /home/ubuntu/workspace`.
4. Install the GitHub pages and Jekyll gems with `bundle install`.
5. Generate a Jekyll blog in the current folder with `jekyll new .` (note the dot in the command to specify the working folder).

## Build and Serve the Content

At that point, I had a basic blog structure. The next step was to build and serve its content. The basic Jekyll command to do that is `jekyll serve`. It builds the blog and starts a local web server on port 4000. However, this doesn't work on Cloud9 because the port 4000 is not available. Every Cloud9 workspace have its unique live application URL with the `https://[workspace-name]-[user-name].c9users.io` format. The IP and port of the server running the application is unknown and can change between sessions.

To run Jekyll, Cloud9 recommends using `jekyll serve --host $IP --port $PORT --baseurl ''` instead. `$IP` and `$PORT` are environment variables holding the values of the workspace application IP and port. The `--baseurl ''` flag is recommended to override the blog setting defined in the `_config.yml` file to an empty string to match the Cloud9 live application URL. The `url` and `baseurl` settings are used by Jekyll to determine the root URL of the blog. In `http://example.com/blog` the `url` would be `http://example.com` and the `baseurl` would be `/blog`.

## Use a Separate File for Cloud9 Configuration

Instead of using the `--baseurl ''` flag, a separate configuration file can be used to store the Cloud9 specific configurations. The `jekyll serve --host $IP --port $PORT --config _config.yml,_local_config.yml` command can be used to specify to use the `_config.yml` file first and the `_local_config.yml` to override some settings. The second file just needs to specify 2 settings:

{% highlight yaml %}
url: "https://[workspace-name]-[user-name].c9users.io"
baseurl: ""
{% endhighlight %}

## Use Run Configurations to Save Time

Having to type the command at every session is not very efficient. A better approach is to use Cloud9 run configurations:

1. Open the "Run" menu.
2. Open the "Run Configurations" submenu.
3. Click the "New Run Configuration" menu item. A new tab will open.
4. Type "Jekyll" in the "Run Config Name" field.
5. Type `jekyll serve --host $IP --port $PORT --config _config.yml,_local_config.yml` in the "Command" field. The "Runner" should change to "Shell command" automatically.
6. Click the "CWD" button and choose the root folder of the Jekyll blog.
7. Click the "Run" button to build and serve the blog.

The run configuration is saved in the workspace and can be launched at any time from the "Run" menu.

[jekyll]: https://jekyllrb.com
[github-pages]: https://pages.github.com
[cloud9]: https://c9.io
[source1]: https://help.github.com/articles/using-jekyll-with-pages/
[source2]: https://docs.c9.io/docs/jekyll
[source3]: http://jekyllrb.com/docs/github-pages/
