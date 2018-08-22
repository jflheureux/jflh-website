---
title: Preparing a Sitecore Instance for Habitat Home: The Quick Way
tags: [Sitecore, Habitat Home, PowerShell]
sharing:
  twitter:  # Max 116 characters
  linkedin:
---

When it comes the time to install the [Habitat Home](https://github.com/Sitecore/Sitecore.HabitatHome.Content) demo, one can be daunted by the large number of [additional modules](https://github.com/Sitecore/Sitecore.HabitatHome.Content#additional-modules) to install on top of the Sitecore instance. Installation of the modules can be really time consuming, expecially when having to install the demo on multiple different machines. Not to talk about the [Habitat Home Commerce](https://github.com/Sitecore/Sitecore.HabitatHome.Commerce) demo that can be installed on top of Habitat Home that requires [Sitecore Experience Commerce 9](https://github.com/Sitecore/Sitecore.HabitatHome.Commerce#prerequisites).

Instead of installing all of this manually, a scripted solution exists.

<!-- more -->

Jean-François Larente, who works at Sitecore, created a very nice PowerShell solution to install everything with only a handful of commands. It is hosted in the [Sitecore.HabitatHome.Utilities](https://github.com/Sitecore/Sitecore.HabitatHome.Utilities) GitHub repository.

## Clone the Repository

The first step is to clone the repository on your computer

```
git clone https://github.com/Sitecore/Sitecore.HabitatHome.Utilities.git
```

## Installing Sitecore 9 and the Required Modules

The Sitecore Experience Platform (XP) 9 installation files are in the `.\XP\install` folder.

The install script is `install-xp0.ps1`. It will install the Sitecore Install Framework, download Sitecore 9 and all the required modules from dev.sitecore.net, install Sitecore and the modules, creates and bind SSL certificates, add application pool memberships, and update SXA Solr cores. It is very complete but it cannot be called yet. It requires a `configuration-xp0.json` configuration file created by running 2 scripts.

### Creating a Configuration File

The first script creates the `configuration-xp0.json` configuration file with default values. You call it in a PowerShell console:

```
.\set-installation-defaults.ps1
```

### Overriding Configuration Values

It is likely that you want to apply some changes to the default values for your installation. This is the role of the `set-installation-overrides.ps1.example` script.

You have to duplicate the script file, remove the `.example` extension, and edit the file. Inside, there are common overrides to the default configuration values like user names, passwords, site name, site location...

The variables are the same as the ones in the `set-installation-defaults.ps1` file. You can override any of the installation defaults script variables in the installation overrides script. The goal is to keep only the variables you need to override. Remove anything you want to keep as default.

Be aware that modifying variables like `$assets.root`, `$site.prefix`, `$site.suffix`, `$site.webroot`, `$site.hostName`, or `$xConnect.siteName` also requires to override any variable that depends on them to recompute their values.

Once your `set-installation-overrides.ps1` file is ready, apply the overrides to your `configuration-xp0.json` configuration file by running it in a PowerShell console:

```
.\set-installation-overrides.ps1
```

### Providing a Sitecore License

The last step before installing is to place a valid Sitecore license in the assets subfolder:

```
.\assets\license.xml
```

### Speeding Up the Installation

Downloading Sitecore and the modules is quite long. If you already have them and you want a faster install, you can copy the archives in the assets folder:

```
.\assets\Sitecore 9.0.2 rev. 180604 (WDP XP0 packages).zip
.\assets\packages\Connect for Microsoft Dynamics 2.0.1 rev. 180108.zip
.\assets\packages\Data Exchange Framework 2.0.1 rev. 180108.zip
.\assets\packages\Dynamics Provider for Data Exchange Framework 2.0.1 rev. 180108.zip
.\assets\packages\Sitecore Experience Accelerator 1.7.1 rev. 180604 for 9.0.zip
.\assets\packages\Sitecore PowerShell Extensions-4.7.2 for Sitecore 8.zip
.\assets\packages\Sitecore Provider for Data Exchange Framework 2.0.1 rev. 180108.zip
.\assets\packages\SQL Provider for Data Exchange Framework 2.0.1 rev. 180108.zip
.\assets\packages\xConnect Provider for Data Exchange Framework 2.0.1 rev. 180108.zip
```

### Installing

Once your license is in place, start the installation by running this script in a PoweShell console:

```
.\install-xp0.ps1
```

## Installing Sitecore Experience Commerce 9



## Conclusion

Next time you see [Jean-François Larente](https://github.com/jeanfrancoislarente) in person, buy him a drink! He deserves it.