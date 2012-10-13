---
layout: page
title : Using Web-Sessions
group: General Use
weight: 3

---

### What is a [WebSession](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebSession)?

A `WebSession` is responsible for storing all user-generated data (cookies, cache, authentication, etc.). It can either be purely in-memory or saved to disk (you will need to provide a writeable path to store the data at runtime).

Each `IWebView` instance must have a `WebSession` to store its data into. You can associate multiple `IWebView` instances with each `WebSession`.

### The Default WebSession

The `WebCore` creates a default, in-memory `WebSession` upon initialization. This session is used for all `IWebView` instances unless otherwise specified. You can access the `WebSession` assigned to a `IWebView` instance, through the [`IWebVew.WebSession`](http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_WebSession) property.

### Creating a WebSession

You can create a WebSession like so:

{% highlight csharp %}
WebSession session = WebCore.CreateWebSession( "C:\\SessionDataPath", WebPreferences.Default );
{% endhighlight %}

This session will synchronize all of its data to: `C:\SessionDataPath\`.

If you would like to create an in-memory session, use the provided overload:

{% highlight csharp %}
WebSession session = WebCore.CreateWebSession( WebPreferences.Default );
{% endhighlight %}

#### Specifying Custom Preferences
You can supply custom preferences for each `WebSession` via the *`prefs`* parameter. Here's an example:

{% highlight csharp %}
// Create an in-memory session with our custom preferences
WebSession session = WebCore.CreateWebSession( new WebPreferences()
{
    Plugins = false,
    SmoothScrolling = true,
    CustomCSS = "body { background-color : rgb(153, 255, 204); }"
} );
{% endhighlight %}

### Assigning the WebSession to one or more IWebView instances
You can assign your `WebSession` instace to one more `IWebView` instances. The procedure differs depending on the component you use.

#### Assigning to a WebView

You should specify the `WebSession` instance when you create the `WebView`:

{% highlight csharp %}
// Create a WebSession.
WebSession session = WebCore.CreateWebSession( new WebPreferences()
{
    CustomCSS = "::-webkit-scrollbar { visibility: hidden; }"
} )
// Assign it to a new WebView.
WebView view = WebCore.CreateWebView( 1280, 960, session );
{% endhighlight %}

In technology specific WebControls, the `IWebView.WebSession` property is implemented as a read-write property. You can use it to assign a `WebSession` to a `WebControl`. You can only set this property while the underlying web-view of the `WebControl`, has not yet been created (while `IsLive` is still *`false`*).

#### Assigning to a WPF WebControl

There are two ways to assign a `WebSession` to WPF `WebControl`: either programmatically, or in the designer (XAML).
The *Awesomium.Windows.Controls* assembly provides a `WebSessionProvider` class that can be used to assign a `WebSession` to a WPF `WebControl`, through XAML. Here is how:

{% highlight xml %}
<Window 
    x:Class="WebControlSample.MainWindow" 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:core="clr-namespace:Awesomium.Core;assembly=Awesomium.Core"
    xmlns:awe="http://schemas.awesomium.com/winfx"
    Height="350" 
    Width="525">
    <Window.Resources>
        <awe:WebSessionProvider x:Key="mySession">
            <core:WebPreferences 
                SmoothScrolling="True" 
                CustomCSS="input[type='text'] { background-color : rgb(153, 255, 204); border: 3px double #CCCCCC; width: 230px; }" 
                LoadImagesAutomatically="False"/>
        </awe:WebSessionProvider>
    </Window.Resources>
    <Grid SnapsToDevicePixels="True">
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <awe:WebControl 
            Name="webControl" 
            WebSession="{Binding Source={StaticResource mySession}}" 
            Source="http://www.w3schools.com/tags/tryit.asp?filename=tryhtml_select_multiple"/>
        <awe:WebControl 
            Name="webControl1" 
            Grid.Column="1" 
            WebSession="{Binding Source={StaticResource mySession}}" 
            Source="http://www.google.com"/>
    </Grid>
</Window>
{% endhighlight %}

#### Assigning to a Windows Forms WebControl

The following example demonstrates how to assign a `WebSession` to a `WebControl` programmatically. This is currently the only procedure supported in Windows Forms. The same procedure can be used in WPF, if you do not want to use the `WebSessionProvider` in XAML.

{% highlight csharp %}
public MainWindow()
{
    // Create a WebSession.
    WebSession session = WebCore.CreateWebSession( new WebPreferences() 
    { 
        SmoothScrolling = true,
        CustomCSS = "body { background-color : rgb(153, 255, 204); }" 
    } );

    InitializeComponent();

    // Assign it to the control. This should only occur here, before the
    // the underlying view of the control is created.
    webControl.WebSession = session;
    webControl.Source = new Uri( "http://www.awesomium.com" );
}
{% endhighlight %}

### Accessing WebSessions
The `WebCore` maintains all the `WebSession` instances that have been created, including the default one. You can access them, through the [`WebCore.Sessions`](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Sessions) property.

Each `WebSession` instance, also provides a [`Views`](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebSession_Views) property. This can be used to access the `IWebView` instances that this `WebSession` is currently assigned to.

You can access a `WebSession` assigned to an `IWebView` instance at runtime, to add a custom [`DataSource`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Data_DataSource) (for more details, see: [Using Data-Sources](using-data-sources.html)) or set cookies. Note however that you cannot change the `WebPreferences` defined for a session, after the session's creation.

### Destroying the WebSession
`WebSession` is a disposable object. You can dispose `WebSession` instances that are no longer associated with a view. If you forget to do this, all `WebSession` instances maintained by the `WebCore`, are disposed at `WebCore.Shutdown`. This ensures that the `WebSession` will be saved to disk upon application exit.

Note however that a session **cannot not be destroyed until all `IWebView` instances associated with it have been destroyed.** Attempting to dispose a `WebSession` instance that is still associated with views, will throw an exception in Awesomium.NET.
