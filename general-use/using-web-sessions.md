---
layout: page
title : Using Web-Sessions
group: General Use
weight: 6

---

### What is a [WebSession](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebSession)?

A `WebSession` is responsible for storing all user-generated data (cookies, cache, authentication, etc.). It can either be purely in-memory or saved to disk (you will need to provide a writeable path to store the data at runtime).

Each `IWebView` instance must have a `WebSession` to store its data into. You can associate multiple `IWebView` instances with each `WebSession`.

### The Default WebSession

The `WebCore` creates a default, in-memory `WebSession` upon initialization. This session is used for all `IWebView` instances unless otherwise specified. You can access the `WebSession` assigned to a `IWebView` instance, through the [`IWebVew.WebSession`](http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_WebSession) property.

### Creating a WebSession

You can create a WebSession like so:

{% highlight csharp %}
WebSession session = WebCore.CreateWebSession( 
    @"C:\SessionDataPath", WebPreferences.Default );
{% endhighlight %}

This session will synchronize all of its data to: `C:\SessionDataPath\`.

> You can also specify a relative path for `dataPath`.

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

> In technology specific WebControls, the `IWebView.WebSession` property is implemented as a read-write property. You can use it to assign a `WebSession` to a `WebControl`. You can only set this property while the underlying web-view of the `WebControl`, has not yet been created (while `IsLive` is still *`false`*).

#### Assigning to a WPF WebControl

There are two ways to assign a `WebSession` to WPF `WebControl`: either programmatically or in the designer (XAML).
The *Awesomium.Windows.Controls* assembly provides a [`WebSessionProvider`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebSessionProvider) class that can be used to assign a `WebSession` to a WPF `WebControl`, through XAML. Here is how:

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
            Source="http://www.awesomium.com/"/>
        <awe:WebControl 
            Name="webControl1" 
            Grid.Column="1" 
            WebSession="{Binding Source={StaticResource mySession}}" 
            Source="http://www.google.com"/>
    </Grid>
</Window>
{% endhighlight %}

#### Assigning to a Windows Forms WebControl

There are two ways to assign a `WebSession` to Windows Forms `WebControl`: either programmatically or in the designer.
The *Awesomium.Windows.Forms* assembly provides a [`WebSessionProvider`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Forms_WebSessionProvider) component that can be used to assign a `WebSession` to a Windows Forms `WebControl` in the designer. Here is how:

1. Open the design view of a Windows Forms `Form` or `Control`.
3. From the *Toolbox*, under the *Awesomium.NET* tab, drag and drop a `WebControl` in your `Form` or `Control`. The *smart tags* popup immediately opens. Optionally click *Dock to parent container* to dock your `WebControl`.
4. From the *Toolbox*, under the *Awesomium.NET* tab, drag and drop a **`WebSessionProvider`** component in your `Form` or `Control`. The component will be added to the designer's Components' tray at the bottom of the view.
5. Select your `WebControl` and in the *Properties* window, set the `WebSessionProvider` property to the component you just added (a combo-box appears as editor for the property and allows you to select an available `WebSessionProvider` component). You can do the same using the WebControl's *smart tags* popup.
6. Select the `WebSessionProvider` component in the Components' tray.
7. In the *Properties* window you can now adjust your `WebSessionProvider` by editing the `WebPreferences`, assigning DataSources or setting a `DataPath`.

#### Assigning to a WebControl programmatically

The following example demonstrates how to assign a `WebSession` to a `WebControl` programmatically. The same procedure can be used in WPF and Windows Forms, if you do not want to use a `WebSessionProvider` in the designer.

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

#### Tips about WebSessionProviders

* You can specify a relative path when setting the `DataPath` property.
* If your application uses more than one `WebControl` containers (each with a `WebControl` and a `WebSessionProvider`), you can set the `DataPath` of all WebSessionProviders to the same path. This will tell all WebSessionProviders, to provide the same `WebSession` for your controls (therefore sharing cache and cookies).
* `DataPath` can remain empty. This will tell the provider to create an *in-memory* `WebSession`.


### Accessing WebSessions
The `WebCore` maintains all the `WebSession` instances that have been created, including the default one. You can access them, through the [`WebCore.Sessions`](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Sessions) property.

Each `WebSession` instance, also provides a [`Views`](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebSession_Views) property. This can be used to access the `IWebView` instances that this `WebSession` is currently assigned to.

You can access a `WebSession` assigned to an `IWebView` instance at runtime, to add a custom [`DataSource`](http://docs.awesomium.net/?tc=T_Awesomium_Core_Data_DataSource) (for more details, see: [Using Data-Sources](using-data-sources.html)) or set cookies. Note however that you cannot change the `WebPreferences` defined for a session, after the session's creation.

### Destroying the WebSession
`WebSession` is a disposable object. You can dispose `WebSession` instances that are no longer associated with a view. If you forget to do this, all `WebSession` instances maintained by the `WebCore`, are disposed at `WebCore.Shutdown`. This ensures that the `WebSession` will be saved to disk upon application exit.

Note however that a session **cannot not be destroyed until all `IWebView` instances associated with it have been destroyed.** Attempting to dispose a `WebSession` instance that is still associated with views, will throw an exception in Awesomium.NET.
