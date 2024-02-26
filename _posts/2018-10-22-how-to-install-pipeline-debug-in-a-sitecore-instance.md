---
title: How to Install Pipeline.Debug in a Sitecore Instance
tags: [Sitecore, Debugging, NuGet, Pipelines, Visual Studio]
sharing:
  linkedin: Have you heard about Sitecore Pipeline.Debug? It is time to discover this new time saving tool.
---

There is a new tool in town and its name is [Pipeline.Debug](https://github.com/alphasolutionsrepo/pipeline.debug) (Visit the link for gifs of its features). It is an incredible time saver when troubleshooting the input or output of processors inside Sitecore pipelines. It allows to insert debug processors anywhere in a pipeline and log any information from the context or processor arguments. In this blog post, I will cover the steps to install it in a Sitecore instance.

<!-- more -->

## Installation

Pipeline.Debug is available as a NuGet package that must be installed in a Visual Studio website project.

### Adding the NuGet Feed Source

The tool has his own NuGet feed that first need to be added in Visual Studio.

1. Open the **TOOLS** menu > **NuGet Package Manager** > **Package Manager Settings**

   ![Package Manager Setting menu in Visual Studio]({{ '/img/2018-10-13-pipelinedebug/1-PackageManagerSettings.png' | absolute_url }})

2. In the left navigation, select **Package Sources**

   ![NuGet Package Sources in Visual Studio]({{ '/img/2018-10-13-pipelinedebug/2-PackageSources.png' | absolute_url }})

3. Click the top-right green plus (+) icon

4. In the bottom part, set the values of the new package source:
   - Name: Pipeline.Debug MyGet
   - Source: https://www.myget.org/F/pipelinedebug/api/v2

5. Click the **Update** button

6. Click the **OK** button

### Adding to the Project

1. Select one Visual Studio website project

2. Right click the project in the Visual Studio Solution Explorer

3. Choose **Manage NuGet Packages ...**

   ![Manage NuGet Packages menu in Visual Studio]({{ '/img/2018-10-13-pipelinedebug/3-ManageNugetPackages.png' | absolute_url }})
4. In the top-right corner **Package source** dropdown, select **Pipeline.Debug MyGet**

   ![Finding the pipeline.debug NuGet package in the NuGet Package Manager]({{ '/img/2018-10-13-pipelinedebug/4-NuGet.png' | absolute_url }})
5. In the top-left corner, select the **Browse** tab

6. Below the tabs, select the **pipeline.debug** package

7. On the right side, click the **Install** button

8. In the **Preview Changes** dialog, click the **OK** button

   ![Preview Changes Dialog]({{ '/img/2018-10-13-pipelinedebug/5-PreviewChanges.png' | absolute_url }})

This results in new files added to the project folders:

![Project files added by pipeline.debug]({{ '/img/2018-10-13-pipelinedebug/6-Result.png' | absolute_url }})

## Deployment

Re-deploy the website project to the Sitecore instance. If the deployment method is a custom script, include those files:

- App_Config/Include/PipelineDebug/PipelineDebug.config
- sitecore/admin/
  - PipelineDebug.css
  - PipelineDebug.html
  - PipelineDebug.js
- bin/PipelineDebug.dll

## Usage

The Pipeline.Debug tool can now be accessed in a browser: **[scheme]**://**[host]**/sitecore/admin/pipelinedebug.html

Replace **[scheme]** and **[host]** with the values of the Sitecore CM instance you are working with.

An easy way to keep this address handy, is to add it to the links section of the [Sitecore Extensions](https://github.com/alan-null/sc_ext) browser plugin. That way, it can be opened for any Sitecore instance, as long as you deploy the files on it before.
