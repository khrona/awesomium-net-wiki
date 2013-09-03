---
layout: page
title : Introduction to Web-Views
group: General Use
weight: 1

---

### What is a Web-View?

A Web-View is like a tab in a browser. You load pages into it, interact with it, and display it in some pre-defined manner.

There are two different types of Web-Views:

* #### Offscreen Views

  Offscreen Views are rendered continuously to a pixel-buffer surface (see [`Surface`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Surface)). It is your responsibility to display this surface in your application and pass all mouse/keyboard events. This gives you the flexibility to display web-content in any environment (such as a 3D engine or headless renderer).
  
  In **Awesomium.NET**, we provide predefined surfaces for every technology that take care of copying the native pixel buffer to managed equivalent bitmaps and help you render the buffer to technology specific surfaces.

* #### Windowed Views

  Windowed Views are rendered directly to a platform window (`HWND`, `NSView`, etc.) and capture all input themselves.

  When using a windowed [`WebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebView) on MS Windows, you should call [`WebView.ParentWindow`](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebView_ParentWindow) immediately after creating this type of view (the view cannot create a window until a parent is set). **Technology specific `WebControls`, handle this internally.**

### The [`IWebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView) Interface

The `IWebView` interface exposes the full API of a native Awesomium Web-View, as well as .NET specific API added to web-views. It is provided for advanced coding and in certain scenarios, it allows you to override much of the .NET internal logic and call on the native web-view directly.

**All managed Web-View components, implement this interface.** It provides most of the common API of all Awesomium.NET Web-View components. It also exposes **all the events** that a web-view component can fire.

In **Awesomium.NET** documentation and articles, a Web-View instance is usually referred to as an **`IWebView` instance** and common members of this interface implemented by each Awesomium.NET Web-View component, are referenced through this interface.

For more details, read the documentation of [`IWebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView).

### Awesomium.NET Web-View Components

Awesomium.NET provides 3 different web-view components for MS Windows, 2 for Mac OSX and 2 for the Unity game engine. The following presents these components:

<dl>
<dt><h4>"<a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_WebView">WebView</a>" Component</h4></dt>

<dd>
<h5>Compatible With</h5>
All (WinForms, WPF, Mono, etc.)
<h5>Description</h5>
Provided by the <i>Awesomium.Core</i> assembly on MS Windows and by the equivalent <i>Awesomium.Mono</i> assembly for all platforms, it can be used in any technology. <b>For more details, read <a href="using-the-webview.html">Using the WebView</a>.</b>
<h5>View Types</h5>
<ul>
<li>
Offscreen (Default)
</li>
<li>
Windowed
</li>
</ul>
<blockquote><p>The type is defined during creation.</p></blockquote>
</dd>

<hr/>
<dt>
<h4>WPF "<a href="http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebControl">WebControl</a>" Component</h4></dt>

<dd>
<h5>Compatible With</h5>
WPF Only
<h5>Description</h5>
Provided by the <i>Awesomium.Windows.Controls</i> assembly. It can be used in WPF applications. <b>For more details, read <a href="wpf-webcontrol.html">Using the WPF WebControl</a>.</b>
<h5>View Types</h5>
<ul>
<li>
Offscreen (Default. Uses 100% WPF logic to copy and render the pixel buffer. This maintains the advantages of the Presentation Framework.)
</li>
<li>
Windowed (Renders a windowed web-view using a <code>HwndHost</code>. Misses many presentation features but supports full hardware acceleration.)
</li>
</ul>
<blockquote><p>The type is defined by setting the <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a> property.</p></blockquote>
</dd>

<hr/>
<dt>
<h4>WinForms "<a href="http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl">WebControl</a>" Component</h4></dt>

<dd>
<h5>Compatible With</h5>
WinForms Only
<h5>Description</h5>
Provided by the <i>Awesomium.Windows.Forms</i> assembly. It can be used in Windows Forms applications. <b>For more details, read <a href="winforms-webcontrol.html">Using the Windows Forms WebControl</a>.</b>
<h5>View Types</h5>
<ul>
<li>
Windowed (Default)
</li>
<li>
Offscreen
</li>
</ul>
<blockquote><p>The type is defined by setting the <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_ViewType"><code>WebControl.ViewType</code></a> property.</p></blockquote>
</dd>

<hr/>
<dt>
<h4>MonoMac "<a href="http://docs.awesomium.net/monomac/?tc=T_Awesomium_Mono_Mac_OSMWebView">OSMWebView</a>" Component</h4></dt>

<dd>
<h5>Compatible With</h5>
MonoMac (OSX) Only
<h5>Description</h5>
Provided by the <i>Awesomium.Mono.Mac</i> assembly on Mac OSX. It can be used in Cocoa applications on OSX using Mono. <b>For more details, read <a href="http://wiki.awesomium.net/monomac/using-the-osmwebview.html">Using the MonoMac OSMWebView</a>.</b>
<h5>View Types</h5>
<ul>
<li>
Windowed (<code>NSView</code>)
</li>
</ul>
</dd>

<hr/>
<dt>
<h4>Unity "<a href="http://docs.awesomium.net/unity/?tc=T_Awesomium_Unity_WebUIComponent">WebUIComponent</a>" Component</h4></dt>

<dd>
<h5>Compatible With</h5>
Unity Game Engine Only
<h5>Description</h5>
Provided by the <i>Awesomium.Unity</i> assembly. It can be used in Unity games on Windows and Mac OSX. <b>For more details, read <a href="http://wiki.awesomium.net/unity/using-the-webuicomponent.html">Using the Unity WebUIComponent</a>.</b>
<h5>View Types</h5>
<ul>
<li>
Offscreen (Renders to a Unity <code>Texture</code>)
</li>
</ul>
</dd>

</dl>


### Asynchronous API

Awesomium adopts a similar multi-process architecture to Chrome. Each `IWebView` instance is actually isolated and rendered in a separate process.

Most method calls are sent via a piped message to the child-process and may not complete immediately. To be notified of different events, read the **[Handling Events](#handling_events)** section below.

You should take extra care with the few methods that are actually synchronous. You can use [`IWebView.GetLastError`](http://docs.awesomium.net/?tc=M_Awesomium_Core_IWebView_GetLastError) to check if there was an error dispatching a synchronous method call. For example, [`IWebView.ExecuteJavascriptWithResult`](http://docs.awesomium.net/?tc=M_Awesomium_Core_IWebView_ExecuteJavascriptWithResult) is sent synchronously because it must return a value:

{% highlight csharp %}
// Execute some Javascript that returns a result.
JSValue result = webView.ExecuteJavascriptWithResult( scriptString );

// Check for any errors
if ( webView.GetLastError() != Error.None )
    System.Diagnostics.Debug.Print( "There was an error calling this synchronous method" );
{% endhighlight %}

### Handling Events

The `IWebView` interface exposes many events that you can handle and receive notifications. These events are implemented by all **Awesomium.NET** Web-View components. Here are some examples:

Using a [`WebView`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebView) component:

{% highlight csharp %}
bool finishedLoading = false;

// Load some content.
view.Source = new Uri( "http://www.awesomium.com" );

// Handle the LoadingFrameComplete event.
// For this example, we use a lambda expression.
view.LoadingFrameComplete += ( s, e ) =>
{
    // The main frame always finishes loading last for a given page load.
    if ( e.IsMainFrame )
        finishedLoading = true;
};

// Update the core while the main frame
// is not yet loaded.
while ( !finishedLoading )
{
    Thread.Sleep( 100 );
    WebCore.Update();
}

// A BitmapSurface is assigned by default to all WebViews.
BitmapSurface surface = (BitmapSurface)view.Surface;
// Save the buffer to a PNG image.
surface.SaveToPNG( "result.png", true );
{% endhighlight %}

Using the Windows Forms [`WebControl`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebControl):

{% highlight csharp %}
webControl.TitleChanged += OnTitleChanged;
webControl.AddressChanged += OnAddressChanged;

[...]

private void OnTitleChanged( object sender, TitleChangedEventArgs e )
{
    // Reflect the page's title to the window text.
    this.Text = e.Title;
}

private void OnAddressChanged( object sender, UrlEventArgs e )
{
    // Reflect the current URL to the window text.
    // Normally, after the page loads, we will get a title.
    // But a page may as well not specify a title.
    this.Text = e.Url.ToString();
}
{% endhighlight %}

#### Binding to Notifications

Most events set relative properties that reflect the current status of an `IWebView` instance. For example, when [`IWebView.TargetURLChanged`](http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_TargetURLChanged) is fired, the `IWebView.TargetURL` and `IWebView.HasTargetURL` properties, have already been updated.

The `WebView` component and the Windows Forms `WebControl`, implement [`INotifyPropertyChanged`](http://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx). Likewise, the WPF `WebControl` can provide notifications through its dependency model.

This allows you to set bindings to properties of an `IWebView` instance, directly in the Visual Studio designer. For example, you can use the **Data Bindings** setting in the **Properties** window of the Windows Forms designer, to bind the text of UI elements of your `Form` to properties of a `WebControl`.

Here is an example of setting bindings in Windows Forms programmatically, using a `WebView`:

{% highlight csharp %}
private BindingSource bindingSource;

[...]

// Create a BindingSource and set our WebView as DataSource.
bindingSource = new BindingSource() { DataSource = webView };

// Bind the Text property of the Form, to the Title property of the WebView.
this.DataBindings.Add( 
    new Binding( "Text", bindingSource, "Title", true ) );
{% endhighlight %}

And here is an equivalent WPF example:

{% highlight xml %}
<Window 
    x:Class="WebControlSample.MainWindow" 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:awe="http://schemas.awesomium.com/winfx"
    Height="600" 
    Width="800" 
    Title="{Binding Title, ElementName=webControl}">
    <Grid SnapsToDevicePixels="True">
        <awe:WebControl Name="webControl" Source="http://www.google.com"/>
    </Grid>
</Window>
{% endhighlight %}

> All `IWebView` events are dispatched asynchronously (meaning that the event may arrive a few milliseconds after the event actually happened in the child-process).


### Features as a Service

The availability of some features of an `IWebView`, occassionally depends on the current state of the view, or its type (*offscreen* or *windowed*). For this reason, the `IWebView` interface inherits [`IServiceProvider`](http://msdn.microsoft.com/en-us/library/system.iserviceprovider.aspx) and provides these features in the form of a service.

Users can query for a service, through `IServiceProvider.GetService`, specifying the type of the requested service.

In the following example, we query for the `IWebViewIMEComposition` service that allows you to pass text input via IME and be notified of any IME-related events. This feature is only available in *offscreen* `IWebView` instances.

{% highlight csharp %}
// Query for the IWebViewIMEComposition service.
IWebViewIMEComposition imeComposition = webControl.GetService( 
    typeof( IWebViewIMEComposition ) );

// GetService will return a null reference, if the service
// is not available.
if ( imeComposition != null )
{
    // Handle some events.
    imeComposition.Cancel += UserCanceledIME;
    // Activate IME.
    imeComposition.ActivateIME( true );
}
{% endhighlight %}


### Native Web-View Wrapping Sequence

It is important to understand the sequence of events that occur when a native web-view is being wrapped by an **Awesomium.NET** component. This helps you decide the moment certain operations can be performed on the component.

> For details, read the [Web-View Initialization Sequence](initialization-sequence.html) article.


### Cleaning Up

All `IWebView` instances implement `IDisposable` and expose a `Dispose` method. You should generally call `Dispose` on an `IWebView` instance when you're done using it. This allows the view to perform some cleanup and release resources.

> For offscreen `IWebView` instances, the `ISurface` instance currently assigned to `IWebView.Surface` is disposed when the view is disposed.

When an `IWebView` instance is displayed in a graphical environment (like when using a `WebControl`), you should not call `Dispose` while the container of the view is still visible.

> The `IWebView.IsDisposed` property can be used to check if a view has been disposed.

Here is an example using the WPF `WebControl`:

{% highlight csharp %}
// Override Window.OnClosed.
protected override void OnClosed( EventArgs e )
{
    base.OnClosed( e );

    // Dispose the WebControl.
    webControl.Dispose();

    // This was the only Window in our app.
    // Shutdown the WebCore while exiting.
    WebCore.Shutdown();
}
{% endhighlight %}

> All views maintained by the `WebCore` are automatically disposed when `WebCore.Shutdown` is called.


### Read some more articles

* [Basic Concepts](http://wiki.awesomium.net/getting-started/basic-concepts.html)
* [Using the WebView](using-the-webview.html)
* [Using the WPF WebControl](wpf-webcontrol.html)
* [Using the Windows Forms WebControl](winforms-webcontrol.html)
* [Using the Unity WebUIComponent](http://wiki.awesomium.net/unity/using-the-webuicomponent.html)
* [Using the MonoMac OSMWebView](http://wiki.awesomium.net/monomac/using-the-osmwebview.html)
* [Web-View Initialization Sequence](initialization-sequence.html)
