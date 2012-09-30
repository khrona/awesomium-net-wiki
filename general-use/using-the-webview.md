---
layout: page
title : Using the WebView
group: General Use
weight: 2

---

### Introduction

The [`WebView`](http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=3e2510f8-2e3c-2da0-1304-d247f0a80484) is the primary managed wrapper of an Awesomium Web-View. It is provided through the *Awesomium.Core* assembly and can be used in any technology.

It is mainly used as a *windowless* web-view component in applications without GUI (Console applications, services etc.), MVVM applications or managed technologies where Awesomium.NET does not already provide a web-view component.

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

### Handling Events

### Cleaning Up