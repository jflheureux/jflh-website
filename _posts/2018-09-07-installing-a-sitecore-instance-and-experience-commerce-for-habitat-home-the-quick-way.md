---
title: Installing a Sitecore Instance and Experience Commerce for Habitat Home - The Quick Way
tags: [Sitecore, Habitat Home, PowerShell]
sharing:
  linkedin: Ever wanted to automate the installation of Sitecore 9, Experience Commerce 9, and all the additional modules required to run the Habitat Home demo? This blog post is for you!
---

When it comes the time to install the [Habitat Home](https://github.com/Sitecore/Sitecore.HabitatHome.Content) demo, one can be daunted by the large number of [additional modules](https://github.com/Sitecore/Sitecore.HabitatHome.Content#additional-modules) to install on top of the Sitecore instance. Installation of the modules can be really time consuming, especially when having to install the demo on multiple machines. Not to mention the [Habitat Home Commerce](https://github.com/Sitecore/Sitecore.HabitatHome.Commerce) demo that can be installed on top of Habitat Home which requires [Sitecore Experience Commerce 9](https://github.com/Sitecore/Sitecore.HabitatHome.Commerce#prerequisites).

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

The install script is `install-xp0.ps1`. It installs the Sitecore Install Framework, downloads Sitecore 9 and all the required modules from dev.sitecore.net, installs Sitecore and the modules, creates and binds SSL certificates, adds application pool memberships, and updates SXA Solr cores. It is very complete but it cannot be called yet. It requires a `configuration-xp0.json` configuration file which is created by running 2 scripts.

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

Downloading Sitecore and the modules is quite long. If you already have them and you want a faster install, you can copy the files in the assets folder:

* .\assets\\[Sitecore 9.0.2 rev. 180604 (WDP XP0 packages).zip](https://dev.sitecore.net/~/media/F53E9734518E47EF892AD40A333B9426.ashx)
* .\assets\\[WebPlatformInstaller_amd64_en-US.msi](https://download.microsoft.com/download/C/F/F/CFF3A0B8-99D4-41A2-AE1A-496C08BEB904/WebPlatformInstaller_amd64_en-US.msi)
* .\assets\packages\\[Connect for Microsoft Dynamics 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/ADBAF4CC6736499EBA0EBA6A9767D825.ashx)
* .\assets\packages\\[Data Exchange Framework 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/C50B044E45FE4C4DA9E675CBEED3AA09.ashx)
* .\assets\packages\\[Dynamics Provider for Data Exchange Framework 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/819FB4C75CC74A8C984C343BEF7B53F1.ashx)
* .\assets\packages\\[Sitecore Experience Accelerator 1.7.1 rev. 180604 for 9.0.zip](https://dev.sitecore.net/~/media/1FF242BE683E4DE989925F74B78978FC.ashx)
* .\assets\packages\\[Sitecore PowerShell Extensions-4.7.2 for Sitecore 8.zip](https://marketplace.sitecore.net/services/~/media/3D2CADDAB4A34CEFB1CFD3DD86D198D5.ashx?data=Sitecore%20PowerShell%20Extensions-4.7.2%20for%20Sitecore%208&itemId=6aaea046-83af-4ef1-ab91-87f5f9c1aa57)
* .\assets\packages\\[Sitecore Provider for Data Exchange Framework 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/D57A1FBB98ED4125B78D740E5B5F1772.ashx)
* .\assets\packages\\[SQL Provider for Data Exchange Framework 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/F243222B9A95497BAB6B591D39560E95.ashx)
* .\assets\packages\\[xConnect Provider for Data Exchange Framework 2.0.1 rev. 180108.zip](https://dev.sitecore.net/~/media/C61F1265BB494CAFA4229FC9FC704AB0.ashx)

> **Note:** The list of assets might have changed since the publication of this blog post. Check the `assets.json` file for the required assets.

### Installing

Once your license is in place, start the installation by running this script in a PoweShell console:

```
.\install-xp0.ps1
```

## Installing Sitecore Experience Commerce 9

The Sitecore Experience Commerce (XC) 9 installation is very similar to the Experience Platform. The files are in the `.\XC\install` folder.

### Creating a Configuration File

The `set-installation-defaults.ps1` script requires the `configuration-xp0.json` file from the `.\XP\install` folder created to install Experience Platform. It is merged with the Experience Commerce default values to create the `configuration-xc0.json` file. You call it in a PowerShell console:

```
.\set-installation-defaults.ps1
```

### Overriding Configuration Values

Same as with Experience Platform, you have to duplicate the `set-installation-overrides.ps1.example` script file, remove the `.example` extension, and edit the file to remove or add anything you want to override.

Once your `set-installation-overrides.ps1` file is ready, apply the overrides to your `configuration-xc0.json` configuration file by running it in a PowerShell console:

```
.\set-installation-overrides.ps1
```

### Speeding Up the Installation

If you already have the assets and you want a faster install, you can copy the files in the assets folder:

* .\assets\Commerce\\[Habitat Home Product Images.zip](https://sitecore.box.com/shared/static/bjvge68eqge87su5vg258366rve6bg5d.zip)
* .\assets\Downloads\\[msbuild.microsoft.visualstudio.web.targets.14.0.0.3.nupkg](https://www.nuget.org/api/v2/package/MSBuild.Microsoft.VisualStudio.Web.targets/14.0.0.3)
* .\assets\Downloads\\[Sitecore.Commerce.2018.07-2.2.126.zip](https://dev.sitecore.net/~/media/F374366CA5C649C99B09D35D5EF1BFCE.ashx)

> **Note:** The list of assets might have changed since the publication of this blog post. Check the `set-installation-defaults.ps1` and `install-xc0.ps1` files for the required assets.

### Installing

Start the installation by running this script in a PoweShell console:

```
.\install-xc0.ps1
```

## Conclusion

Next time you see [Jean-François Larente](https://github.com/jeanfrancoislarente) in person, buy him a drink! He deserves it.