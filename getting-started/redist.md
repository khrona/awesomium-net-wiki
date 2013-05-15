---
layout: page
title : Distributing Awesomium.NET
group: Getting Started
weight: 4

---

When working on a system where the Awesomium SDK is installed, projects that use Awesomium.NET reference the Awesomium.NET assemblies and native Awesomium libraries deployed in your system's GAC by the SDK's installer.

To distribute your application, you must make sure all the necessary references are deployed alongside your executable or to the target system's **Global Assembly Cache** (GAC).

### Windows Installer

The **Awesomium SDK** installation package includes a Windows Installer **Merge Module** that takes care of deploying all necessary Awesomium.NET assemblies and Awesomium native libraries to the target system's **Global Assembly Cache** (GAC).

The merge module is not deployed by default when installing the SDK. To get the merge module, when installing the SDK:

1. Select **Custom installation**.
2. Check the **Redistribution Modules** under **Awesomium.NET**.

If you have already installed the SDK, follow these steps:

1. Start the installer by either selecting the **Awesomium SDK** under the **Programs and Features** page (**Add Remove Programs** on **Windows XP**) of the **Control Panel** and clicking on **Change**, or by selecting **Setup** under the SDK's **Start Menu** folder.
2. When the installer opens, click on **Modify**.
3. Check the **Redistribution Modules** under **Awesomium.NET**.
4. Click the **Modify** button to apply the changes.

After installing the SDK, the default location of the merge module is:

    %SDKFolder%\wrappers\Awesomium.NET\Redistribute
    
The name of the merge module file (last character depends on the version) is: **_AWENET017X.msm_**. Include the merge module to your Windows Installer project (the method depends on the Windows Installer editor used). This is all you need to distribute your application.

### ClickOnce or Manual Deployment

If you want to use other methods of deploying your application (such as **ClickOnce** or manual deployment, or third-party installation tools), you need to make sure that all the necessary **Awesomium** and **Awesomium.NET** binaries are deployed alongside your executable.

The following steps describe how you can **automatically** copy all the Awesomium.NET assemblies and native libraries to your project's output directory, when developing with **Visual Studio**.

1. Expand the **References** folder in **Solution Explorer** (for a VB.NET project, open your project's properties and select the **References** tab).
2. Select all the **Awesomium.NET** assemblies used by your application (all the assemblies starting with: *Awesomium.*). One of them, must always be **_Awesomium.Core.dll_**: the major dependency of **Awesomium.NET**.
4. In the **Properties** window, set **Copy Local** to **True**.

[![Copying Awesomium.NET references in C# and VB.NET](http://labs.awesomium.com/wp-content/uploads/distribute-300x92.png "Copying Awesomium.NET references in C# and VB.NET")](http://labs.awesomium.com/wp-content/uploads/distribute.png)

Next time you build your project, all Awesomium.NET assemblies including the necessary native Awesomium libraries, will be copied to your output directory. This is all you need to distribute your application.
