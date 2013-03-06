---
layout: page
title : What's New in 1.7.0
group: Getting Started
weight: 3

---

We are very proud to announce the **final release** of **Awesomium.NET 1.7.0**, we think you'll be pretty impressed with all the new features and enhancements we've added these past few months.

* [Download the Awesomium 1.7.0 SDK (deploys Awesomium.NET binaries and samples)](http://www.awesomium.com/download)
* [Awesomium.NET 1.7.0 API Reference](http://docs.awesomium.net)
* [Setting up on Windows](http://wiki.awesomium.net/getting-started/setting-up-on-windows.html)

A lot has gone into this release. Before you go through the changes in Awesomium.NET, take a look at the major changes in native Awesomium:

* [Awesomium 1.7.0 Final Release](http://labs.awesomium.com/whats-new-in-1-7-0/)

Here is a summary of changes in **Awesomium.NET**:

### New Features

* All API members' names with an ID suffix, have been renamed to have an Id suffix (eg, IWebView.ProcessId). These would be too many to list above.
* Added protections that prevent JS interoperation methods being called on an IWebView instance, before DocumentReady.
* Added protections that prevent the use of JS-related API being called, when WebPreferences.Javascript is disabled.
* JSObject no longer inherits DynamicObject. It now provides advanced low level dynamic API support through .NET's IDynamicMetaObjectProvider.
* Added protections that prevent the use of synchronous JS API calls, when the context is synchronous (synchronous custom method handler).
* Implemented internal downloads management and added the DownloadCollection and DownloadItem classes that allow you to easily monitor and bind your MVVM application to download	operations.
* Added support for Zoom setting to views and the ability to retrieve settings for specific hosts from WebSession.
* Made it so the WPF/WinForms WebSessionProviders provide a previously created WebSession with the same DataPath, instead of attempting to create a new one.
* Made it so the IsManipulationEnabled of WPF presenters is bound to the WebControl (this will propagate the ManipulationDelta routed event to the WebControl).
* Added design-time support for WebPreferences (available through WebSessionProviders).
* All properties and event args that accept or return a Uri, are now ready to silently respond to malformed URIs.
* Removed the ResourceInterceptor class. IResourceInterceptor interface remains.
* Added support for JavaScript dialogs.
* Added support for handling Certificate errors and obtaining page's security related information.
* Added the ability cancel/handle navigations asynchronously, through the new INavigationInterceptor service. The service also supports white/blacklisting.
* Added default dialog for selecting a folder at Print requests, SelectLocalFiles and design-time, to the WPF WebControl and design-time support.
* Added default drop-down (popup) menu to WPF WebControl (see WebPopupMenu and WebPopupMenuBase).
* Added default WPF certificate error dialog.
* Added default WPF web page info cpntrol and popup. (see WebPageInfoPopup and WebPageInfoControl).
* Added default dialog layers for handling Javascript dialogs to the WPF WebControl.
* Added the ability to handle all user input (including system and control keyboard events) through the WPF WebControl's Preview events.
* Added default dialogs for handling Javascript dialogs to the Windows Forms WebControl.
* Added default ContextMenu to the Windows Forms WebControl.
* Extended design-time support for the WPF WebControl.
* Added design-time support for the Windows Forms WebControl.

### API Changes

#### New API:

##### *Awesomium.Core*

* WebCore.Configuration (returns the configuration settings used to initialize the core).
* WebSession.ClearCookies()
* WebSession.IsJavascriptEnabled (no JS related API can called if it's false).
* WebSession.GetZoomForURL (zoom setting for a particular host).
* WebSessionCollection.Item (indexer to access sessions by data path)
* WebSessionCollection.Contains (overload)
* NativeViewModel (split from ViewModel. Separates disposal logic from MVVM logic so that ViewModel can be used by non-disposable types).
* NetError enumeration. (Provides description for the error code of the LoadingFrameFailed event).
* CertError (Certificate errors)
* JavascriptAsynchMethodEventHandler (delegate used in dyamic model to create asynchronous custom methods).
* DownloadItem (MVVM friendly info for a dowanload operation)
* DownloadCollection
* WebCore.DownloadBegin (fired before a download starts; provides the DownloadItem just added to Downloads).
* WebCore.Downloads (list of active, complete or canceled download operations)
* INavigationInterceptor (Service available on IWebView instances. Allows you to handle navigation requests or add filtering ruls - whitelist, blacklist).
* IWebView.WindowClosed (event fired when 'window.close' is called from Javascript).
* JSDialogFlags (used with ShowJavascriptDialog).
* IWebView.ShowJavascriptDialog (alert, confirm, prompt)
* IWebView.ConsoleMessage (event fired for Javascript console messages).
* IWebView.InitializeNativeView (event fired right before the underlying web-view of a technology specfic WebControl, is instantiated).
* IWebView.NativeViewInitialized (event fired right after the underlying web-view of a technology specfic WebControl, is instantiated).
* IWebView.SynchronousMessageTimout (sets the maximum amount of time to wait for a response from a synchronous IPC message dispatched to the view's renderer process).
* IWebView.ZoomIn (Zooms in by 20%)
* IWebView.ZoomOut (Zooms out by 20%)
* IWebView.ResetZoom (Resets to 100%)
* IWebView.Zoom
* IWebView.Printing (event for when a printing operation starts.
* IWebView.IsPrinting (property reflecting printing operation status)
* IWebView.RequestPageInfo (Requests page's security related information)
* IWebView.ShowPageInfo (Event fired in response to RequestPageInfo)
* WebPageInfo (Available through ShowPageInfo)
* WebPopupMenuInfo.Item (indexer; WebMenuItem is now `IEnumerable<WebMenuItem>`).
* IWebView.CertificateError (Event fired on certificate errors. Allows you to ignore errors).
* IWebView.Identifier (Unique, global web-view identifier).
* WebPopupMenuInfo.Count
* WebViewCollection.GetByProcessId
* WebViewCollection.GetById (See IWebView.Identifier).
* WebViewCollection.Contains (overload for seeking by process ID).
* UrlEventArgs.HasErrors (indicates that the original URI provided, is malformed).
* UrlEventArgs.OriginalString (the original URI string)
* CreatedWebViewEventArgs.IsWindowOpen
* CreatedWebViewEventArgs.IsUserSpecsOnly
* CreatedWebViewEventArgs.Specs (JSWindowOpenSpecs)
* JSWindowOpenSpecs structure (represents the specs specified by users in a Javascript 'window.open' call, including size and positioning, if any).
* IWebView.IsJavascriptEnabled (Prevents calling Javascript-related API when WebPreferences.Javascript is false)

##### *Awesomium.Windows.Controls* (WPF)

* WebPopupMenu (default drop-down, popup menu)
* WebControl.PopupMenuResourceKey (ComponentResourceKey for the default popup menu).
* SourceBinding (MarkupExtension; binds TextBoxes to WebControl.Source and provides features to make them act as address-boxes).
* UriValueConverter (IValueConverter for Uri).
* InputEventManager (WeakEventManager that allows to use the "weak event pattern" for UIElement user input events; used by the SourceBinding).
* WebViewEventManager (WeakEventManager that allows to use the "weak event pattern" for IWebView events).

##### *Awesomium.Windows.Forms* (Windows Forms)

* WebSessionProvider (equivalent to the WPF WebSessionProvider, allows you to assign a WebSession to a Windows Forms WebControl at design-time).
* PromptForm (default dialog for Javascript 'window.confirm').
* WebControl.ViewType (property that tells the Windows Forms WebControl to optionally wrap an offscreen web-view. - Enables transparency).

#### Modified API

- Changed all event triggers to not accept a sender (having a sender is a wrong pattern).
- IWebView.Download -> WebCore.Download
- IWebView.BeginNavigation (See INavigationInterceptor).
- WebPopupMenuInfo.Items
- CreatedWebViewEventArgs.InitialPos
- ShowCreatedWebViewEventArgs -> CreatedWebViewEventArgs

### Bug Fixes

* Fixed issue with ResourceDataSource that would not allow requests with queries.
* Fixed issue in designer of the Windows Forms and WPF WebControl, that would cause the core to attempt to make p/invoke calls to the Awesomium library at design-time.
* Fixed bug that would prevent escaped URIs from being properly passed to servers.
* Fixed bug that would cause the SurfaceFactory to throw an exception, when destroying a Surface.
* Fixed bug in default implementation of dialog callbacks in WebView. Events are now properly canceled.
* Removed all useless production-time GC.Collect() calls.
* Made it so default navigation of new child views, is properly cancelled until the view is wrapped.
* Removed useless protection from CreateGlobalJavascriptObject, that would prevent the method from being called before ProcessCreated. This is no longer a requirement.
* Fixed bug that would cause an exception be thrown at user input, when using the WPF WebViewPresenter independently.
* Fixed issues with setting a NativeView to the WPF WebControl.
* Fixed false LoadingFrameFailed events occuring before downloads.
* Fixed bug that would prevent setting the Source property to the WPF WebControl, at runtime.
* Fixed bug that would prevent the Source property of the WPF WebControl, to reflect the currently loaded page URL.
* Fixed bug introduced in RC3 that would prevent tranparency to the WPF WebControl.
* Fixed AddressChanged event not being fired on WPF WebControl.
* Fixed issues with Unicode strings not being properly passed to and from native Awesomium.
* Fixed issues with the IsCrashed property not reflecting the actual status of the view, and issues with the Crashed event not being fired.
* Fixed exception that could occur when attempting to Shutdown the core during internal update.
* Fixed bug that could occassionally cause a NullReferenceException at ExecuteJavascript.
* Fixed InvalidCastException when setting ViewType on the WPF WebControl.
* Fixed issue in WPF WebControl that caused the right side to be cropped when DPI was set to any value other than the default.
* Fixed view crash when WebPreferences.Javascript was set to false on the view's WebSession.
* Fixed bug causing a WebControl crash when repeatedly opening/closing a WebPopupMenu.
* Fixed "Width and Height must be positive" exceptions on the WPF WebControl.
* Fixed focusing editable content issues on the WPF WebControl.



Please [read here](http://www.awesomium.com/ChangeLog.txt), the list of bugs fixed in native Awesomium. Some of them affected Awesomium.NET.


### Known Issues

* On **Windows 8**, **WebGL** is currently not supported.
* On some **Windows XP** installations, you may get a **DllNotFoundException** when trying to execute an application that uses Awesomium (native or .NET bindings). This indicates you need to install the latest update of **Direct X**, available here: [DirectX End-User Runtimes (June 2010)](http://www.microsoft.com/en-us/download/details.aspx?id=8109). **Note:** If you are using the Windows Installer to deploy the Awesomium SDK, the setup package takes care of this.

#### Under production:

* When you are using the Windows Forms WebControl, drop-down (popup) menus (e.g., HTML: `<select>`), are not displayed automatically. Predefined drop-down (popup) menus have been added to the WPF WebControl but not to the Windows Forms WebControl yet. However, the new powerful API allows you to design and display these yourself, by handling the [`ShowPopupMenu`](http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ShowPopupMenu) event.