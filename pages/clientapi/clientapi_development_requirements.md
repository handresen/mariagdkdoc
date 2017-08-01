---
title: Development Requirements
keywords: GDK overview
tags: [nuget, requirements, service ]
sidebar: clientapi_sidebar
permalink: clientapi_development_requirements.html
summary: This section describes the development envirement prerequisites for creating applications with Maria GDK. 
---

## Visual Studio
* ***Visual Studio 2010*** or later
    * .Net Framework 4, *Full installation!* <br>
     Client Profile is currently ***not*** sufficient!
    * NuGet package manager. See [Installing NuGet Package manager](maria_gdk/programming/prerequisites&#installing_nuget_package_manager) (below).
    * Use of R# ReSharper, from JetBrains, is recommended. <br>
      See [http://www.jetbrains.com/resharper](http://www.jetbrains.com/resharper)

## SlimDX
* ***SlimDX*** with bitness matching your application (x64 and/or x86) ( *End User Runtime January 2012*)
    * [SlimDX Download](http://slimdx.org/download.php)

## Maria GDK Services
* Access to desired ***Maria GDK Services*** <br>
  Installation and maintenance of the services is described in ***[Maria GDK core documentation](core_landing_page.html#services).***

## Maria GDK Nuget Packages {#mariagdkpackages}
To be able to create applications utilizing the features of Maria GDK, you need to download one or more of the **Maria GDK NuGet packages**, using the **NuGet Package manager**

### Installing NuGet Package manager

If you don’t have the package manager installed, you can locate and download it one of the following ways:

*  Download and install from [http://nuget.codeplex.com/](http://nuget.codeplex.com/) 
*  With the Visual Studio Extension Manager *Tools⇒Extension Manager* 

{% include image.html file="clientapi/devrequirements/installpackgmgr.png" caption="Installing package manager" %}

#### About NuGet packages
*  For general information on use of NuGet packages, see:<br>
 [http://docs.nuget.org/](http://docs.nuget.org/)

*  For more information on how to use the NuGet manager, see:<br> [http://docs.nuget.org/docs/Reference/Package-Manager-Console-PowerShell-Reference](http://docs.nuget.org/docs/Reference/Package-Manager-Console-PowerShell-Reference)

*  For info on NuGet packages with source control, see:<br> [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)


### Loading Maria GDK Packages 
Maria GDK assemblies are added to your project(s) through NuGet packages with the NuGet package manager -- available from the project popup menu.<br>

{% include image.html file="clientapi/devrequirements/managenugetpackages.png" caption="Manage NuGet Packages" %}

To access the packages, you need to define the Maria GDK package source (if not already set by nuget.congif files). Select the ***Settings*** button to get the ***Package Manager Options*** page to create the source.

{% include image.html file="clientapi/devrequirements/nuget_packagesources.png" caption="Maria GDK NuGet Package Sources" %}

The Maria GDK Nuget packages are loaded from the Teleplan Globe support site. Use the following source:

| Maria GDK Release | https://www.myget.org/F/maria-gdk/api/v3/index.json |
| Maria GDK Nightly | https://www.myget.org/F/maria-gdk-nightly/api/v3/index.json |

Here you will find the available Maria GDK Nuget packages for download to your project(s), and also available updates when already installed.

{% include image.html file="clientapi/devrequirements/mariagdk_nugetpackages.png" caption="Maria GDK NuGet Packages" %}

Make sure that the **Maria GDK version** downloaded matches the **Maria Services** you are using.

{% include links.html %}