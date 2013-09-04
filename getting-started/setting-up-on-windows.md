---
layout: page
title : Setting Up on Windows
group: Getting Started
weight: 2

---

### Before We Begin

This tutorial assumes that you are developing for the **.NET Framework**. If you want to use Awesomium with a C++ Application, then you'll need to follow our C++ tutorial instead.

### Checklist
You will need the following:

* The latest **Awesomium SDK for Windows** (download it [here](http://www.awesomium.com/download/))
* Microsoft **Visual Studio 2010** or **Visual Studio 2012** (or any of the free **Express** editions)


### Installing the SDK

If you have a previous version of the **Awesomium SDK** installed, you can update to the latest version by executing **Check for Updates** under the Awesomium SDK Start Menu folder.

During installation, you will be asked to select the **installation folder** of the SDK.

**Typical installation** will:

* Install the native Awesomium libraries and C++ headers.
* Install the Awesomium.NET assemblies in GAC and on disk.
* Deploy documentation and support files.
* Deploy the Awesomium (C++) Samples.
* Deploy the Awesomium.NET Samples.

Select **Custom installation** to choose if you want to:

* Deploy the Awesomium (C++) Samples.
* Deploy the Awesomium.NET Samples.
* Deploy Awesomium.NET redistribution modules for Windows installer.

#### Installation Folder

If you have not specified a custom installation folder during installation, the default installation folder of the SDK (hereafter, `%SDKFolder%` or just `%%`) is:

    %ProgramFiles%\Awesomium Technologies LLC\Awesomium SDK\1.7.x.x

> If you have been updating from version 1.6.x, the `Awesomium Technologies LLC` folder may be named `Khrona LLC`. The automatic updater does not > change the root folder for consistency reasons.

#### Folder Structure of the SDK

The installer creates the following folders in the installation directory:

* *build/bin*: Contains DLLs that you will need to bundle with your application.
* *build/lib*: Contains static libraries used by C++ applications.
* *include*: Contains headers used by C++ applications.
* **_wrappers/Awesomium.NET_**: Contains the assemblies, documentation and support files for the official **.NET** bindings (**Awesomium.NET**).


### Awesomium.NET Assemblies

The Awesomium.NET assemblies and their native dependencies, are installed to the system's **Global Assembly Cache** (GAC) by default, when you install the SDK.

These files are also available on disk, under:

    %SDKFolder%\wrappers\Awesomium.NET\Assemblies

Below is a list of those files and a short description for them:

#### Core

This is the core assembly of **Awesomium.NET**. It must be referenced by all projects using Awesomium. The core assembly is linked at compilation to all the necessary native Awesomium libraries.

* Awesomium.Core.dll (Core assembly)
* Awesomium.Core.XML (XML Documentation used by VS IntelliSense)
* avcodec-53.dll
* avformat-53.dll
* avutil-51.dll
* awesomium.dll
* awesomium_process
* icudt.dll
* libEGL.dll
* libGLESv2.dll
* xinput9_1_0.dll
* inspector.pak (Awesomium Inspector assets)

#### WPF

The WPF assembly includes the WPF [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebControl) other controls and utilities for using Awesomium in a WPF application.

* Awesomium.Windows.Controls.dll
* Awesomium.Windows.Controls.XML

#### Windows Forms

The Windows Forms assembly includes the Windows Forms [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl) other controls and utilities for using Awesomium in a Windows Forms application.

* Awesomium.Windows.Forms.dll
* Awesomium.Windows.Forms.XML

#### The `Design` subfolder

This folder contains the design assemblies, used to provide design-time features for the Awesomium.NET components, in Visual Studio 2010 & Visual Studio 2012.

> **THESE ASSEMBLIES ARE NOT REDISTRIBUTABLE.**

If you have used the Windows Installer to install the SDK, these assemblies must already be in your system's GAC.

#### The `Packed` subfolder
       
This folder contains a build of the *Awesomium.Core.dll* assembly, that has been linked to the **packed** (compressed) native libraries available under `%SDKFolder%\build\bin\packed\`.

If you want to use the packed (compressed) native libraries with your projects, you will have to use this version of *Awesomium.Core.dll* instead of the one under `Assemblies` (or the one deployed in GAC).

To use this assembly and the packed (compressed) native dependencies, follow the steps below:

1. Follow the steps in **[Distributing Awesomium.NET - Manual Deployment](redist.html#clickonce_or_manual_deployment)**, to make sure the **Awesomium.NET** assemblies are copied to your project's output directory, when you build your project.
2. Build your project. All Awesomium.NET assemblies including the necessary native Awesomium libraries, will be copied to your output directory.
3. When you are ready to deploy your application, copy the following files alongside your executable, **overwriting** the ones already there:
    * %%\wrappers\Awesomium.NET\Assemblies\\**Packed**\Awesomium.Core.dll
    * %%\build\bin\\**packed**\awesomium.dll
    * %%\build\bin\\**packed**\icudt.dll

   You can perform this operation automatically, by using a **Post Build** event in Visual Studio.

***
**Note:** Awesomium.NET 1.7.x, targets the **.NET Framework 4.0 Client Profile**.

***


### Awesomium.NET Samples

Samples for Awesomium.NET 1.7, are deployed under:

    %PublicDocuments%\Awesomium SDK Samples\1.7.x.x\Awesomium.NET\Samples

There are samples available for all desktop technologies (Console, WPF and Windows Forms) and the **C#** and **VB.NET** languages. The structure of the *Awesomium.NET\\Samples* folder, follows the **_technology\\language_** scheme.

#### Building the Samples

To build the included samples, follow these steps:

1. Open **Visual Studio**.
2. Select **File -> Open -> Project/Solution** and navigate to the samples folder (see above).
3. Open the **_Awesomium.NET Samples.sln_** solution.
4. From the **Build** menu, select **Rebuild Solution**.

If you are using any of the **Visual Studio 2010** or **Visual Studio 2012** **Express** editions, you will have to open the individual projects, depending on the edition you use.

The default **Output Directories** of the included samples, are: 

##### Debug Mode:

    %PubDocs%\Awesomium SDK Samples\1.7.x.x\Awesomium.NET\Assemblies\Debug
    
##### Release Mode:

    %PubDocs%\Awesomium SDK Samples\1.7.x.x\Awesomium.NET\Assemblies\Release

#### Examining the Samples

You should go through the source code of the included samples. Most of the code in samples is commented and can be used as a fast tutorial to start working with Awesomium.NET.


### Set up a Project
    
To start working with Awesomium.NET 1.7.x, follow these steps:

1. Create a new .NET Framework **Windows** project.
2. Make sure your project targets either **.NET 4 Framework Client Profile** or the full **.NET Framework 4**.
3. Make sure the target platform of your project is **x86**.

Depending on the technology you use, in addition to the aforementioned steps, follow the steps bellow.

#### Windowless Applications

For a **Console** application (or any non-GUI application that uses only the **Core** features of Awesomium, such as a Windows Service):

1. From the **Project** menu, select **Add Reference...**
2. In the **Add Reference** dialog, click on **Browse...** and navigate to the **_%SDKFolder%\wrappers\Awesomium.NET\Assemblies_** folder.
3. Add a reference to the **_Awesomium.Core.dll_** assembly.

**Note:** You do not need to manually add references to your project when using any of the **Awesomium.NET** **WPF** or **Windows Forms** controls. All necessary references will be added automatically to your project, when you drag & drop an Awesomium.NET WPF or Windows Forms control to a UI container in the **Visual Studio** designer. For specific technologies, read the sections below.

**Note:** The *Awesomium.Core.dll* assembly, does not contain any components that can be used in a designer.

#### WPF Applications

For a WPF application:

1. Open the design view of a `Window` or `Control`.
2. Open the **Toolbox** window.
3. You should be able to see the **Awesomium.NET** tab (if you do not see the **Awesomium.NET** tab, read the **[Visual Studio Toolbox](#visual_studio_toolbox)** section below).
4. The tab must contain the following components:
    * [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebControl)
    * [`WebPageInfoControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebPageInfoControl)
5. Drag & drop the [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebControl) to your `Window` or `Control`. This will also add the necessary references to your project and activate design-time features.

**For more details, read the [Using the WPF WebControl](http://wiki.awesomium.net/general-use/wpf-webcontrol.html) article.**

#### Windows Forms Applications

For a Windows Forms application:

1. Open the design view of a `Window` or `Control`.
2. Open the **Toolbox** window.
3. You should be able to see the **Awesomium.NET** tab (if you do not see the **Awesomium.NET** tab, read the **[Visual Studio Toolbox](#visual_studio_toolbox)** section below).
4. The tab must contain the following components:
    * [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl)
    * [`WebSessionProvider`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebSessionProvider)
    * [`AddressBox`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_AddressBox)
    * [`WebControlContextMenu`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControlContextMenu) (Only used when you want to extend the default context menu)
5. Drag & drop the [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl) to your form. This will also add the necessary references to your project and activate design-time features.

**For more details, read the [Using the Windows Forms WebControl](http://wiki.awesomium.net/general-use/winforms-webcontrol.html) article.**

#### Awesomium.NET Namespaces

Finally, in your code files, depending on the technology you develop for, you will have to import (`using`) any of the available Awesomium.NET namespaces. These are:

* [`Awesomium.Core`](http://docs.awesomium.net/?tc=N_Awesomium_Core) (Core features, [`WebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebView) etc.)
* [`Awesomium.Core.Data`](http://docs.awesomium.net/?tc=N_Awesomium_Core_Data) ([`DataSource`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Data_DataSource) wrappers)
* [`Awesomium.Windows.Controls`](http://docs.awesomium.net/?tc=N_Awesomium_Windows_Controls) (WPF [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebControl), other controls, surfaces and Utilities)
* [`Awesomium.Windows.Data`](http://docs.awesomium.net/?tc=N_Awesomium_Windows_Data) (WPF [`DataSource`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Data_DataSource) providers for use in XAML)
* [`Awesomium.Windows.Forms`](http://docs.awesomium.net/?tc=N_Awesomium_Windows_Forms) (Windows Forms [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl), other controls, surfaces and Utilities)

#### Visual Studio Toolbox

If for any reason you do not see the **Awesomium.NET** tab in the **Toolbox** window of **Visual Studio** when the **WPF** or **Windows Forms** **designer view** is open or if the contents of the Toolbox are accidentally corrupted, you can restore the **Awesomium.NET** tab and its contents by following these steps:

1. Close **Visual Studio**.
2. Execute the **_Setup.exe_** utility under **_%SDKFolder%\wrappers\Awesomium.NET\Assemblies_**.
3. Click on **Uninstall**.
4. When the operation completes and the *Setup* utility exits, open **Visual Studio**.
5. Make sure you open the **Toolbox** window. You do not need to have a project loaded. Just opening the Toolbox window is enough for Visual Studio to read the relevant registry keys and update its contents.
6. Close **Visual Studio**.
7. Execute the **_Setup.exe_** utility under **_%SDKFolder%\wrappers\Awesomium.NET\Assemblies_** again.
8. Click on **Install**. This will re-install the assemblies in GAC and restore the necessary registry keys to add the **Awesomium.NET** components in the **Visual Studio Toolbox**.

Next time you start **Visual Studio** and open the **Toolbox** window, its contents will be updated and you should be able to see the **Awesomium.NET** tab when the WPF or Windows Forms designer view is open.


### Distributing Awesomium.NET

When working on a system where the Awesomium SDK is installed, projects that use Awesomium.NET reference the Awesomium.NET assemblies and native Awesomium libraries deployed in your system's GAC by the SDK's installer.

To distribute your application, you must make sure all the necessary references are deployed with your executable to the target system.

**For details, read the [Distributing Awesomium.NET](redist.html) article.**


### Read some more articles

* [Basic Concepts](basic-concepts.html)
* [What's New in the latest release](http://wiki.awesomium.net/changelogs/whats-new.html)
* [Distributing Awesomium.NET](redist.html)


### Additional Resources

* [Awesomium.NET Online API Reference](http://docs.awesomium.net)
* [Community Forums](http://answers.awesomium.com/spaces/11/)
* [Codeplex](http://awesomium.codeplex.com/)
