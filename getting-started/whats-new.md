---
layout: page
title : What's New in 1.7 RC3
group: Getting Started
weight: 3

---

We are very proud to announce the **final experimental build** of **Awesomium.NET 1.7**, we think you'll be pretty impressed with all the new features and enhancements we've added these past few months.

* [Download the Awesomium 1.7 RC3 SDK (Includes Awesomium.NET binaries and samples)](http://www.awesomium.com/download)
* [Awesomium.NET 1.7 RC3 API Reference](http://www.awesomium.com/docs/1_7_rc3/sharp_api)
* [Setting up on Windows](http://wiki.awesomium.net/getting-started/setting-up-on-windows.html)

A lot has gone into this release. Before you go through the changes in Awesomium.NET, take a look at the major changes in native Awesomium:

* [Awesomium 1.7 Final Candidate](http://forums.awesomium.com/viewtopic.php?f=3&t=86)

Here is a quick summary of the big changes in **Awesomium.NET**:

### New Features

* Added support for the new, *windowed* web-views that render directly to `HWND`. Read more in [Introduction to Web-Views](introduction-to-web-views.html).
* Added the **Windows Forms [`WebControl`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Windows_Forms_WebControl)**. The control now wraps a *windowed* web-view.
* Added [`IResourceInterceptor`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Core_IResourceInterceptor). You can assign your implementation to the `WebCore` to intercept requests for resources, modify requests, and return custom responses. See an example of usage, in the included **JavascriptSample**.
* Added the [`IWebViewPresenter`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Windows_Controls_IWebViewPresenter) interface to the *Awesomium.Windows.Controls* assembly, for creating custom presenters of an `IWebView` instance, that can be added to the style of the `WebControl`.
* The **WPF `WebControl`** can now wrap a *windowed* web-view. A predefined presenter has been added (see [`WebViewHost`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Windows_Controls_WebViewHost)), that is used to present a *windowed* web-view, taking advantage of full hardware acceleration. Read more in: [Working with Windowed Web-Views](working-with-windowed-web-views.html).
* The WPF and Windows Forms WebControls, now handle downloads and selecting local files automatically, by showing the necessary dialogs. Custom handling is still in place and improved. Read more about custom handling, here: [`FileDialogEventArgs.Handled`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=P_Awesomium_Core_FileDialogEventArgs_Handled).
* Significantly improved the powerful [`DataSource`]() API that allows you to provide a custom resource loader for a set of URLs that match a certain prefix. See the **API Changes** section below, and read the [Using Data-Sources](using-data-sources.html) article. All predefined DataSources are now under the new *Awesomium.Core.Data* namespace.
* Added [`DirectoryDataSource`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Core_Data_DirectoryDataSource), a predefined `DataSource` that loads resources from a local directory. Resources can also be cached in-memory for fast response.
* Added [`ResourceDataSource`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Core_Data_ResourceDataSource), a predefined `DataSource` that loads resources from embedded or packed (Build Action: Resource) application resources. The `ResourceDataSource` can also load resources from an external managed assembly.
* Added the base [`DataSourceProvider`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Windows_Data_DataSourceProvider), a WPF `DataSource` provider that can be used in XAML.
* Added WPF DataSourceProviders under the new [`Awesomium.Windows.Data`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=N_Awesomium_Windows_Data) namespece, that provide the respective DataSources and can be used in XAML (see [Using Data-Sources](using-data-sources.html)).
* Added the ability to specify one ore more [`DataSourceProvider`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Windows_Data_DataSourceProvider) instances to a `WebSessionProvider` in XAML (see the **API Changes** section below).
* The WPF `WebSessionProvider` can now provide a `WebSession` that is synchronized to disk.
* Added the [`IWebViewIMEComposition`](http://awesomium.com/docs/1_7_rc3/sharp_api/?tc=T_Awesomium_Core_IWebViewIMEComposition) service for showing your own **I**nput **M**ethod **E**ditor widget into **offscreen** web-views (useful for input of Asian languages). For more details, read the **Features as a Service** section in [Introduction to Web-Views](introduction-to-web-views.html).


### API Changes

### Bug Fixes
