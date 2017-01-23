---
title: Run Sitecore SIM Tool Offline
tags: [Sitecore, SIM Tool]
---
I was on an airplane and tried to run the SIM Tool. To my surprise, I was greated with this error:

![SIM Tool error screenshot]({{ '/img/2016-07-22-run-sitecore-sim-tool-offline/SIMToolError.png' | absolute_url }})

<!-- more -->

Application download did not succeed. Check your network connection, or contact your system administrator or network service provider.

Clicking the details button, this error was logged:

{% highlight text %}
SOURCES
  Deployment url			: file:///C:/Apps/SimTool/SIM.Tool.application
  Deployment Provider url		: http://dl.sitecore.net/updater/sim/SIM.Tool.application

ERROR SUMMARY
  Below is a summary of the errors, details of these errors are listed later in the log.
  * Activation of C:\Apps\SimTool\SIM.Tool.application resulted in exception. Following failure messages were detected:
    + Downloading http://dl.sitecore.net/updater/sim/SIM.Tool.application did not succeed.
    + The remote name could not be resolved: 'dl.sitecore.net'

OPERATION PROGRESS STATUS
  * [2016-07-18 7:50:52 PM] : Activation of C:\Apps\SimTool\SIM.Tool.application has started.

ERROR DETAILS
  Following errors were detected during this operation.
  * [2016-07-18 7:50:52 PM] System.Deployment.Application.DeploymentDownloadException (Unknown subtype)
    - Downloading http://dl.sitecore.net/updater/sim/SIM.Tool.application did not succeed.
    - Source: System.Deployment
    - Stack trace:
      at System.Deployment.Application.SystemNetDownloader.DownloadSingleFile(DownloadQueueItem next)
      at System.Deployment.Application.SystemNetDownloader.DownloadAllFiles()
      at System.Deployment.Application.FileDownloader.Download(SubscriptionState subState)
      at System.Deployment.Application.DownloadManager.DownloadManifestAsRawFile(Uri& sourceUri, String targetPath, IDownloadNotification notification, DownloadOptions options, ServerInformation& serverInformation)
      at System.Deployment.Application.DownloadManager.DownloadDeploymentManifestDirect(SubscriptionStore subStore, Uri& sourceUri, TempFile& tempFile, IDownloadNotification notification, DownloadOptions options, ServerInformation& serverInformation)
      at System.Deployment.Application.DownloadManager.FollowDeploymentProviderUri(SubscriptionStore subStore, AssemblyManifest& deployment, Uri& sourceUri, TempFile& tempFile, IDownloadNotification notification, DownloadOptions options)
      at System.Deployment.Application.DownloadManager.DownloadDeploymentManifestBypass(SubscriptionStore subStore, Uri& sourceUri, TempFile& tempFile, SubscriptionState& subState, IDownloadNotification notification, DownloadOptions options)
      at System.Deployment.Application.ApplicationActivator.PerformDeploymentActivation(Uri activationUri, Boolean isShortcut, String textualSubId, String deploymentProviderUrlFromExtension, BrowserSettings browserSettings, String& errorPageUrl)
      at System.Deployment.Application.ApplicationActivator.ActivateDeploymentWorker(Object state)
    --- Inner Exception ---
    System.Net.WebException
    - The remote name could not be resolved: 'dl.sitecore.net'
    - Source: System
    - Stack trace:
      at System.Net.HttpWebRequest.GetResponse()
      at System.Deployment.Application.SystemNetDownloader.DownloadSingleFile(DownloadQueueItem next)
{% endhighlight %}

I understood that the SIM Tool tries to auto-update at startup. When dl.sitecore.net is unavailable or when offline, it refuses to even run.

The next time I had an Internet connection, I sniffed the network requests made by the SIM Tool to understand the process. To enable offline execution, I ended up creating a local website, setup IIS to serve it and modified my hosts file. This tricks the SIM Tool to believe the installed version is the latest version and enables the updater to succeed.

## Basic Setup

1. Create a folder on your hard drive to store your local website. I named mine `dl.sitecore.net.local`.
2. Create this structure in your folder:

       dl.sitecore.net.local
         updater
           sim
             Application Files

3. In IIS, create a `dl.sitecore.net.local` website with:

    * Site name: dl.sitecore.net.local

    * Physical Path: Your dl.sitecore.net.local folder

    * Host name: dl.sitecore.net

4. Add a disabled hosts file entry for your local website.

       # C:\Windows\System32\drivers\etc\hosts

       #127.0.0.1 dl.sitecore.net

## Before Every Offline Period

When you know you will go offline for a certain period of time and you might need to use the SIM Tool, do the following:

1. Use Fiddler2 or another HTTP proxy to record the network requests made by the SIM Tool when starting.
2. Save the `SIM.Tool.application` file response body to the `\dl.sitecore.net.local\updater\sim` folder.
3. Inspect the request URL of the `SIM.Tool.exe.manifest` file. It is located in a folder like `SIM.Tool_1_4_0_383` where the version number changes at each release.
4. Create this folder in your `\dl.sitecore.net.local\updater\sim\Application Files` folder. Your folder structure will look like:

       dl.sitecore.net.local
         updater
           sim
             Application Files
               SIM.Tool\_1\_4\_0\_383

5. Save the `SIM.Tool.exe.manifest` file response body to the newly created folder.

## Activating The Local Website

When offline, activate the website by uncommenting the hosts file entry and ensuring your local website is running in IIS.

Enjoy the SIM Tool!