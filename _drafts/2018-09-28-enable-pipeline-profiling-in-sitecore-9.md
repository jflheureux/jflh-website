---
title: Enable Pipeline Profilling in Sitecore 9
tags: [Sitecore, Pipelines]
sharing:
  linkedin: 
---

https://habitathome.coveodemo.com/sitecore/admin/pipelines.aspx

<!-- more -->

## Initial

> Pipeline profiling is disabled. No data is currently available.
> To enable pipeline profiling, rename the /App_Config/Include/Sitecore.PipelineProfiling.config.disabled file to Sitecore.PipelineProfiling.config and set the value of the "Pipelines.Profiling.Enabled" setting to "true" if it is set to "false".
> 
> To measure CPU usage during pipeline profiling, set the value of the "Pipelines.Profiling.MeasureCpuTime" setting to "true". Measuring CPU usage adds a performance overhead to the pipeline but provides additional information about the behavior of the processors.

## Configuration File Location

Not \App_Config\Include\Sitecore.PipelineProfiling.config.disabled
Now \App_Config\Environment\Sitecore.PipelineProfiling.config

## Environment Requirement Instead of Disabled Extension

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/" xmlns:set="http://www.sitecore.net/xmlconfig/set/" xmlns:env="http://www.sitecore.net/xmlconfig/env/">
  <sitecore env:require="Profiling">
```

## Add Profiling to Environment

web.config file

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="env:define" value="Local,Profiling"/>
  </appSettings>
</configuration>
```

