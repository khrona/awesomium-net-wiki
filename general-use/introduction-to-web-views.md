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

  Offscreen Views are rendered continuously to a pixel-buffer surface (see [`Surface`](http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=84e8ef47-da6a-8486-6e01-6f8bc4eaede1)). It is your responsibility to display this surface in your application and pass all mouse/keyboard events. This gives you the flexibility to display web-content in any environment (such as a 3D engine or headless renderer).
  
  In **Awesomium.NET**, we provide predefined surfaces for every technology that take care of copying the native pixel buffer to managed equivalent bitmaps and help you render the buffer to technology specific surfaces.

* #### Windowed Views

  Windowed Views are rendered directly to a platform window (`HWND`, `NSView`, etc.) and capture all input themselves.

  When using a windowed [`WebView`](http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=3e2510f8-2e3c-2da0-1304-d247f0a80484) on MS Windows, you should call [`WebView.ParentWindow`]() immediately after creating this type of view (the view cannot create a window until a parent is set). **Technology specific `WebControls`, handle this internally.**

### The [`IWebView`](http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=a6f96b6a-fe54-a975-92a7-ff6a4843c412) Interface

The `IWebView` interface exposes the full API of a native Awesomium Web-View, as well as .NET specific API added to web-views. It is provided for advanced coding and in certain scenarios, it allows you to override much of the .NET internal logic and call on the native web-view directly.

**All managed Web-View components, implement this interface.** It provides most of the common API of all Awesomium.NET Web-View components. It also exposes **all the events** that a web-view component can fire.

In **Awesomium.NET** documentation and articles, a Web-View instance is usually referred to as an **`IWebView` instance** and common members of this interface implemented by each Awesomium.NET Web-View component, are referenced through this interface.

For more details, read the documentation of [`IWebView`](http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=a6f96b6a-fe54-a975-92a7-ff6a4843c412).

### Awesomium.NET Web-View Components

Awesomium.NET for MS Windows provides 3 different web-view components. The following table presents these components:

<table>
<tr>
<th>
Technology
</th>
<th>
Component
</th>
<th>
Description
</th>
<th>
Web-View Type
</th>
</tr>
<tr>
<td>
Any
</td>
<td>
<a href="http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=3e2510f8-2e3c-2da0-1304-d247f0a80484"><code>WebView</code></a>
</td>
<td>
Provided by the <i>Awesomium.Core</i> assembly, it can be used in any technology. For more details, read For more details, read <a href="using-the-webview.html">Using the WebView</a>.
</td>
<td>
<ul>
<li>
Offscreen (Default)
</li>
<li>
Windowed
</li>
</ul>
The type is defined during creation.
</td>
</tr>
<tr>
<td>
WPF
</td>
<td>
<a href="http://awesomium.com/docs/1_7_rc2/sharp_api/?tc=240bd8f8-1c7b-1901-cb57-d17266304266"><code>WebControl</code></a>
</td>
<td>
Provided by the <i>Awesomium.Windows.Controls</i> assembly. It can be used in WPF applications. For more details, read <a href="wpf-webcontrol.html">Introducing the WPF WebControl</a>.
</td>
<td>
<ul>
<li>
Offscreen (Default. Uses 100% WPF logic to copy and render the pixel buffer. This maintains the advantages of the Presentation Framework.)
</li>
<li>
Windowed (Renders a windowed web-view using a <code>HwndHost</code>. Misses many presentation features but supports full hardware acceleration.)
</li>
</ul>
The type is defined by setting the <a href=""><code>WebControl.ViewType</code></a> property.
</td>
</tr>
<tr>
<td>
Windows Forms
</td>
<td>
<a href=""><code>WebControl</code></a>
</td>
<td>
Provided by the <i>Awesomium.Windows.Forms</i> assembly. It can be used in Windows Forms applications. For more details, read <a href="winforms-webcontrol.html">Introducing the Windows Forms WebControl</a>.
</td>
<td>
<ul>
<li>
Windowed
</li>
</ul>
</td>
</tr>
</table>

### Asynchronous API

Awesomium adopts a similar multi-process architecture to Chrome. Each `IWebView` instance is actually isolated and rendered in a separate process.

Most method calls are sent via a piped message to the child-process and may not complete immediately. To be notified of different events, read the **Handling Events** section in **[Introduction to Web-Views](introduction-to-web-views.html)**.

You should take extra care with the few methods that are actually synchronous. You can use [`IWebView.GetLastError`](http://www.awesomium.com/docs/1_7_rc2/sharp_api/?tc=af0c71ab-8ed3-2244-bbbf-fb45376e6ce2) to check if there was an error dispatching a synchronous method call. For example, [`IWebView.ExecuteJavascriptWithResult`](http://www.awesomium.com/docs/1_7_rc2/sharp_api/?tc=22724015-123d-2bc3-cab8-c82c6975a0fe) are sent synchronously because it must return a value:

{% highlight csharp %}
// Execute some Javascript that returns a result.
JSValue result = webView.ExecuteJavascriptWithResult( scriptString );

// Check for any errors
if ( webView.GetLastError() != Error.None )
    System.Diagnostics.Debug.Print( "There was an error calling this synchronous method" );
{% endhighlight %}

### Handling Events

The `IWebView` interface exposes many events that you can handle and receive notifications. These events are implemented by all **Awesomium.NET** Web-View components. Here are some examples:

Using a [`WebView`]() component:

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

Using the Windows Forms [`WebControl`]():

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

Most events set relative properties that reflect the current status of an `IWebView` instance. For example, when [`IWebView.TargetURLChanged`](http://www.awesomium.com/docs/1_7_rc2/sharp_api/?tc=fb3e097f-9154-3d89-0e17-0ec76898baba) is fired, the `IWebView.TargetURL` and `IWebView.HasTargetURL` properties, have already been updated.

The `WebView` component and the Windows Forms `WebControl`, implement [`INotifyPropertyChanged`](http://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx). Likewise, the WPF `WebControl` can provide notifications through its dependency model.

This allows you to set bindings to properties of an `IWebView` instance, directly in the Visual Studio designer. For example, you can use the **Data Bindings** setting in the **Properties** window of the Windows Forms designer, to bind the text of UI elements of your `Form` to properties of a `WebControl`.

Here is an example of setting bindings in Windows Forms programmatically, using a `WebView`:

{% highlight csharp %}
private BindingSource bindingSource;

[...]

// Create a BindingSource and set our WebView as DataSource.
bindingSource = new BindingSource() { DataSource = webView };

// Bind the Text property of the Form, to the Title property of the WebView.
this.DataBindings.Add( new Binding( "Text", bindingSource, "Title", true ) );
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

Please note that all `IWebView` events are dispatched asynchronously (meaning that the event may arrive a few milliseconds after the event actually happened in the child-process).

### Cleaning Up

All `IWebView` instances implement `IDisposable` and expose a `Dispose` method. You should generally call `Dispose` on an `IWebView` instance when you're done using it. This allows the view to perform some cleanup and release resources.

For offscreen `IWebView` instances, the `ISurface` instance currently assigned to `IWebView.Surface`, is disposed when the view is disposed.

When an `IWebView` instance is displayed in a graphical environment (like when using a `WebControl`), you should not call `Dispose` while the container of the view is still visible.

An `IWebView.IsDisposed` property indicates if an view has been disposed.

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

All views maintained by the `WebCore`, are internally disposed when `WebCore.Shutdown` is called.