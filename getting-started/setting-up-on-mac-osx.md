---
layout: page
title : Setting Up on Mac OSX
group: Getting Started
weight: 3

---

### Before We Begin

This tutorial assumes that you are developing for the **Mono** runtime. If you want to use Awesomium with a C++ Application, then you'll need to follow our C++ tutorial instead.

### Checklist
You will need the following:

* The **Awesomium SDK for Mac OSX** (download it [here](http://www.awesomium.com/download/))
* The **Mono Runtime** (download it [here](http://www.go-mono.com/mono-downloads/download.html))
* Latest **Xamarin Studio** (download it [here](http://xamarin.com/download))


### Installing the SDK

If you have a previous **1.7.x** version of the **Awesomium SDK** installed, it is suggested that you first remove it by executing the **Remove Awesomium 1.7.x** tool available under `/Applications/Awesomium/` (should show in Launchpad).

After downloading and mounting the SDK image (*awesomium-sdk-1.7.x.dmg*), run the **Awesomium SDK v1.7.x** installation package.

**Standard installation** will:

* Deploy the native Awesomium framework bundle.
* Deploy the Awesomium.NET assemblies.
* Deploy the Awesomium (C++) Samples.
* Deploy the Awesomium.NET Samples.
* Deploy Demo applications.

Select **Custom installation** to choose if you want to:

* Deploy the Awesomium (C++) Samples.
* Deploy the Awesomium.NET Samples.
* Deploy Demo applications.

#### Installation Folders

The native Awesomium framework bundle, is deployed at:

    /Library/Frameworks/
    
The main installation folder of the SDK (hereafter, `%SDKFolder%`) where samples and Mono assemblies are located, is:

    /Users/Shared/Awesomium/1.7.x/
    
Demo applications and the SDK Removal tool, are deployed at:

    /Applications/Awesomium/

#### Folder Structure of the SDK

The installer creates the following folders in the installation folder:

* *samples/bin*: Contains precompiled C++ demos and their assets.
* *samples/src*: Contains the source code of C++ samples.
* *include*: Contains headers used by C++ applications.
* **_wrappers/Awesomium.NET_**: Contains the assemblies, samples and their assets for the official **Mono** bindings (**Awesomium.NET** and **Awesomium for MonoMac**).


### Awesomium.NET Assemblies

The Awesomium.NET assemblies used by **Mono** and **MonoMac** applications, can be found under:

    %SDKFolder%/wrappers/Awesomium.NET/Assemblies/

Below is a list of those files and a short description for them:

#### Core

This is the core assembly of **Awesomium.NET**. It must be referenced by all **Mono** projects using Awesomium.

* Awesomium.Mono.dll (Core assembly)
* Awesomium.Mono.dll.config (Configuration file used by the **Mono** runtime to locate and load the native Awesomium library)
* Awesomium.Mono.xml (XML Documentation)

> The contents of this assembly are identical to the contents of the *Awesomium.Core* assembly available with **Awesomium.NET** on MS Windows.

#### Awesomium for MonoMac

The *Awesomium.Mono.Mac.dll* assembly includes the MonoMac [`OSMWebView`](http://docs.awesomium.net/monomac/?tc=T_Awesomium_Mono_Mac_OSMWebView), other components and utilities for building Cocoa applications on OSX using Mono.

* Awesomium.Mono.Mac.dll (Awesomium for MonoMac)
* Awesomium.Mono.Mac.dll.config (Configuration file used by the **Mono** runtime to locate and load the native Awesomium library)
* Awesomium.Mono.Mac.xml (XML Documentation)
* Awesomium.MonoMac.dll (*MonoMac.dll* replacement. For details, read below.)

**About _Awesomium.MonoMac.dll_:**

For **Awesomium.NET**, we compile a custom **MonoMac** assembly using the latest [MonoMac](https://github.com/mono/monomac) project source code. The contents of this assembly are absolutely identical to the contents of the *MonoMac.dll* assembly added by default by **Xamarin Studio** as reference to projects using MonoMac. The only difference is that this assembly is signed with a strong name.

We had to compile **MonoMac** from source, since the **Awesomium.NET** assemblies are also signed with a strong name and therefore they cannot reference an assembly that is not signed (the precompiled *MonoMac.dll* provided by Mono and Xamarin Studio, does not have a strong name).

> Compiling MonoMac from source is a procedure suggested by the Mono project itself.

When developing an application that uses **Awesomium for MonoMac**, you should always replace the *MonoMac.dll* reference added to your project by **Xamarin Studio**, with this assembly.

**For details, read the [Creating a Project on Mac OSX](creating-a-project-on-mac-osx.html) article.**


### Awesomium.NET Samples

Samples for Awesomium.NET 1.7, are deployed under:

    %SDKFolder%/wrappers/Awesomium.NET/Assemblies/Samples/Mono

There are samples available for *windowless* (non-GUI) applications and **MonoMac** applications. Samples are written in **C#**.

#### Building the Samples

To build the included samples, follow these steps:

1. Open **Xamarin Studio**.
2. Select **File -> Open...** and navigate to the samples folder (see above).
3. Open the **_Awesomium.Mono.Mac.Samples.sln_** solution.
4. From the **Build** menu, select **Rebuild All**.

The default **Output Directories** of the included samples, are: 

##### Debug Mode:

    %SDKFolder%/wrappers/Awesomium.NET/Assemblies/Debug
    
##### Release Mode:

    %SDKFolder%/wrappers/Awesomium.NET/Assemblies/Release

#### Examining the Samples

You should go through the source code of the included samples. Most of the code in samples is commented and can be used as a fast tutorial to start working with Awesomium.NET.


### Set up a Project
    
You are now ready to start creating your own **Awesomium.NET** projects on Mac OSX.

Irrespective of the technology you're going to be developing for (Console, MonoMac, Gtk# etc.), you should always make sure that:

* Your project targets: **Mono / .NET 4**
* The target platform of your project is **x86**.

**For details about how to start creating your own projects, read the [Creating a Project on Mac OSX](creating-a-project-on-mac-osx.html) article.**


### Read some more articles

* [Basic Concepts](basic-concepts.html)
* [What's New in the latest release](http://wiki.awesomium.net/changelogs/whats-new.html)


### Additional Resources

* [Awesomium for MonoMac Online API Reference](http://docs.awesomium.net/monomac/)
* [Community Forums](http://answers.awesomium.com/spaces/11/)
