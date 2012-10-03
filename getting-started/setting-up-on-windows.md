---
layout: page
title : Setting Up on Windows
group: Getting Started
weight: 1

---

### Before We Begin

This tutorial assumes that you are developing for the **.NET Framework**. If you want to use Awesomium with a C++ Application, then you'll need to follow our C++ tutorial instead.

### Checklist
You will need the following:

* The latest Awesomium v1.7 SDK for Windows (download it [here](http://www.awesomium.com/download/))
* Microsoft Visual Studio 2010 (or any of the free 'Express' editions)

### Install the SDK
After downloading the SDK, unzip the ZIP file to some convenient location.

#### Folder Structure of the SDK
The SDK should contain the following folders in the installation directory:

* *build/bin*: Contains DLLs that you will need to bundle with your application.
* *build/lib*: Contains static libraries used by C++ applications.
* *include*: Contains headers used by C++ applications.
* **_wrappers/Awesomium.NET_**: Contains the assemblies and samples for the official **.NET** bindings.

#### Awesomium.NET References

Before building the samples or developing for Awesomium.NET 1.7 RC, you will need to copy the necessary native dependenncies alongside the Awesomium.NET assemblies. This is how:

1. Copy all the **_.dll_** files from inside the **_build/bin_** folder.
2. Paste the files in the **_wrappers/Awesomium.NET/Assemblies_** folder. The final structure of the *wrappers/Awesomium.NET/Assemblies* folder, should look like this:

* Awesomium.Core.dll *
* Awesomium.Core.XML *
* Awesomium.Windows.Controls.dll *
* Awesomium.Windows.Controls.XML *
* Awesomium.Windows.Forms.dll *
* Awesomium.Windows.Forms.XML *
* avcodec-53.dll
* avformat-53.dll
* avutil-51.dll
* awesomium.dll
* awesomium_process *
* icudt.dll
* libEGL.dll
* libGLESv2.dll
* inspector.pak
* README.txt *

  ###### (*) Files with an asterisc should already be in the folder when you download the SDK in zip format.

Note that **Awesomium.NET 1.7 RC**, targets the **.NET Framework 4.0 Client Profile**.

### Awesomium.NET Samples

Samples for Awesomium.NET 1.7, are included under the **_wrappers/Awesomium.NET/Samples_** folder. These are preliminary samples. More will be added in the final release of Awesomium v1.7.

There are samples available for all desktop technologies (Console, Windows Forms, and WPF) and the **C#** and **VB.NET** languages. The structure of the *wrappers/Awesomium.NET/Samples* folder, follows the **_technology/language_** scheme.

#### Building the Samples

To build the included samples, follow these steps:

1. Open **Visual Studio 2010**
2. Select **File -> Open -> Project/Solution** and navigate to the **_wrappers/Awesomium.NET/Samples_** folder.
3. Open **_Awesomium.NET Samples.sln_**
4. From the **Build** menu, select **Rebuild Solution**.

The default **Output Directories** of the included samples, are: 

* **_wrappers/Awesomium.NET/Assemblies/Debug_** for **Debug** mode.
* **_wrappers/Awesomium.NET/Assemblies/Release_** for **Release** mode.

If you are using any of the **Visual Studio 2010 Express** editions, you will have to open the individual projects, depending on the edition you use.

#### Examining the Samples

You should go through the source code of the included samples. Most of the code of the samples is commented and can be used as a fast tutorial to start working with Awesomium.NET.

### Set up a Project

To start working with Awesomium.NET 1.7 RC, follow these steps:

1. Create a new .NET Framework **Windows** project.
2. From the **Project** menu, select **Add Reference...**
3. In the **Add Reference** dialog, click on **Browse...** and navigate to the **_wrappers/Awesomium.NET/Assemblies_** folder. Depending on the type of project you develop, add references to the following assemblies:

* **Console** (or any application using only the Core features of Awesomium):
  * Awesomium.Core.dll
* **Windows Forms**:
  * Awesomium.Core.dll
  * Awesomium.Windows.Forms.dll
* **WPF**
  * Awesomium.Core.DLL
  * Awesomium.Windows.Controls.dll

4. In your code files, depending on the technology you develop for, you will have to import (`using`) any of the available Awesomium.NET namespaces. These are:

* Awesomium.Core (Core features, `WebView` etc.)
* Awesomium.Core.Data (`DataSource` wrappers)
* Awesomium.Windows.Forms (Windows Forms `WebControl`, other controls, surfaces and Utilities)
* Awesomium.Windows.Controls (WPF `WebControl`, other controls, surfaces and Utilities)

Note that Awesomium.NET 1.7 RC is not deployed using an installer, the controls available with the SDK are not automatically added to the Visual Studio toolbox. To add the controls to the toolbox and use them in the designer, follow these steps:

1. Right-click in the **Toolbox** window and select **Add Tab**.
2. Give a name to your new tab.
3. Right-click in the new tab and select **Choose Items...**.
4. In the **Choose Toolbox Items** dialog, , click on **Browse...** and navigate to the **_wrappers/Awesomium.NET/Assemblies_** folder.
5. Select any of the following assemblies, depending on the technology:
  * Awesomium.Windows.Forms.dll
  * Awesomium.Windows.Controls.dll
6. Click **Open** and then **OK** on the **Choose Toolbox Items** dialog.

The *Awesomium.Core.dll* assembly, does not contain any UI components.

### Distributing Awesomium.NET

1. Expand the **References** folder in **Solution Explorer** (in VB.NET, go to the **References** tab of your project’s properties).
2. Select the Awesomium.NET assemblies used by your application (usually any assembly starting with: *Awesomium.*). Hold down **CTRL** and select all of them (if more than one is used). One of them, must always be: **_Awesomium.Core.dll_**: the major dependency of Awesomium.NET.
3. In the **Properties** window, set **Copy Local** to **True**.

  <a href="http://labs.awesomium.com/wp-content/uploads/distribute.png"><img src="http://labs.awesomium.com/wp-content/uploads/distribute-300x92.png" alt="Copying Awesomium.NET references in C# &amp; VB.NET" title="Copying Awesomium.NET references in C# &amp; VB.NET" width="300" height="92" class="size-medium wp-image-1137"></a>

  ######Copying Awesomium.NET references in C# & VB.NET

Next time you build your project, all Awesomium.NET assemblies including the necessary native Awesomium libraries, will be copied to your output directory. This is all you need to distribute your application.

### Read some more articles
* [Basic Concepts](basic-concepts.html)

