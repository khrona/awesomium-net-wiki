---
layout: page
title : Using the WebView
group: General Use
weight: 2

---

### Introduction

The [`WebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebView) is the primary managed wrapper of an Awesomium Web-View. It is provided through the *Awesomium.Core* assembly on MS Windows and also through *Awesomium.Mono* (platform independent) and *Awesomium.Unity.Core* (Unity game engine). It can be used in any technology.

The `WebView` is mainly used as a *windowless* web-view component in applications without GUI (Console applications, services etc.), MVVM applications or managed technologies where **Awesomium.NET** does not already provide a web-view component.

A `WebView` can be of any type, *offscreen* or *windowed*. For more details read [Introduction to Web-Views](introduction-to-web-views.html).

### Creating a WebView

A new `WebView` instance can only be created through the `WebCore`, using any of the following methods:

* `WebCore.CreateWebView(Int32, Int32)`
* `WebCore.CreateWebView(Int32, Int32, WebSession)`
* `WebCore.CreateWebView(Int32, Int32, WebViewType)`
* `WebCore.CreateWebView(Int32, Int32, WebSession, WebViewType)`

Here are some examples:

{% highlight csharp %}
// Create an offscreen WebView of the specified
// width and height.
WebView view = WebCore.CreateWebView(640, 480);
{% endhighlight %}

Here's an example of how to create a windowed `WebView` to be used in a Windows Forms application:

{% highlight csharp %}
private WebView webView;

public WebForm()
{

    InitializeComponent();

    // Create a windowed WebView instance matching
    // the size of the container.
    webView = WebCore.CreateWebView( 
        this.ClientSize.Width, 
        this.ClientSize.Height, 
        ViewType.Window );
}

protected override void OnHandleCreated(EventArgs e)
{
	base.OnHandleCreated(e);
	
	if (webView == null)
	    return;
	
	// Before using a windowed WebView, we need
	// to assign a parent window.    
	webView.ParentWindow = this.Handle;
}
{% endhighlight %}

### Loading Content

The primary way to load content into any `IWebView` instance, is via the [`IWebView.Source`](http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_Source) property. For example, to begin navigating a `WebView` to Google, you would call:

{% highlight csharp %}
webView.Source = new Uri( "http://www.google.com" );
{% endhighlight %}

All URLs should be properly formatted before attempting to create a `Uri`. You can check if a URL is valid via `System.Uri.IsWellFormedUriString`. The .NET Framework also provides the `System.UriBuilder` utility class, to create or modify complicated URIs. Additionally, in Awesomium.NET we have added an extension method to the `String` class, for creating URIs. Here is how to use it:

{% highlight csharp %}
webView.Source = "http://www.google.com".ToUri();
{% endhighlight %}

Note however that this may fail if the specified string is not a well formatted URL. In this case, `ToUri` fails silently and returns a blank URI ( `about:blank` ).

#### Loading Resources

`IWebView.Source` makes it really easy to load remote content on the Internet, but what about local resources? If you would like to load local resources with your application, we recommend using a [`DataSource`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Data_DataSource). This powerful bit of API allows you to provide a custom resource loader for a set of URLs that match a certain prefix.

**See [this article](using-data-sources.html) for an introduction to `DataSource` and the available, predefined `DataSource` classes that come with Awesomium.NET.**

### Displaying the WebView

How a `WebView` component is displayed, depends on its [type]().

*Offscreen* `WebView` components can be displayed to Windows Forms and WPF using any of the predefined [`ISurface`](http://docs.awesomium.net/?tc=T_Awesomium_Core_ISurface) classes available with Awesomium.NET. For more advanced scenarios or for displaying a `WebView` in technologies where Awesomium.NET does not already provide a web-view component, you can create a custom `ISurface` implementation.

*Windowed* `WebView` components, render directly to a platform window (`HWND`, `NSView`, etc.) and capture all input themselves. In certain technologies, resizing is also handled automatically.

For more details, read:

* [Working with Offscreen Web-Views](working-with-offscreen-web-views.html)
* [Working with Windowed Web-Views](working-with-windowed-web-views.html)

### Cleaning Up

All `IWebView` instances implement `IDisposable` and expose a `Dispose` method. For details about how to properly dispose a `WebView`, read the **Cleaning Up** section in **[Introduction to Web-Views](introduction-to-web-views.html)**.

### Read some more articles

* [Introduction to Web-Views](introduction-to-web-views.html)
* [Basic Concepts](basic-concepts.html)
