---
layout: page
title : Handling or Simulating User Input with the WPF WebControl
group: WPF
weight: 2

---

### Overview

The WPF `WebControl` uses WPF specific presenters ([`IWebViewPresenter`](http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebViewPresenter)), depending on the specified `ViewType`. Presenters are used in the visual tree when styling a WPF `WebControl`. These can be custom WPF surfaces also implementing [`ISurface`](http://docs.awesomium.net/?tc=T_Awesomium_Core_ISurface) (for *offscreen* views) or any other control providing custom presentation of an `IWebView` instance.

When [`WebControl.ViewType`](http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType) is set to [`Offscreen`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebViewType) (default), the predefined [`WebViewPresenter`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebViewPresenter) is used for presentation. This is an `ISurface` component that uses 100% WPF logic to copy the view's pixel buffer to a bitmap that it then renders for presentation.

`WebViewPresenter` inherits `FrameworkElement` and is part of the WPF WebControl's default style. When the visual tree is loaded for presentation, the WPF `WebControl` uses  the `IWebViewPresenter` interface to communicate with the `WebViewPresenter` that takes care of all UI related operations which besides rendering include:

* Handling user input
* Presentation of dialogs
* Presentation of menus

**When using the WPF `WebControl`, it is the `WebViewPresenter` and not the `WebControl` itself that handles user input**. This means that handling user input related events on the `WebControl`, will not prevent the `WebViewPresenter` from handling these events and passing them to the native view.

Since the `WebViewPresenter` is part of the WebControl's visual tree, the appropriate way to handle such events yourself, is by handling the *`Preview`* events.

> Native *windowed* views handle user input themselves and there's no way to handle or simulate user input on the managed side. If you need to handle or simulate user input, make sure that [`WebControl.ViewType`](http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType) is set to [`Offscreen`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebViewType) (default).

### Example

The following example demonstrates:

* How to prevent the presenter from handling the TAB and Space characters and passing them to the native view. 
* How to translate right-arrow to a Space hit.

#### XAML (*`MainWindow.xaml`*)

{% highlight xml %}
<Window 
    x:Class="WpfApplication1.MainWindow" 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
    xmlns:osm="http://schemas.awesomium.com/winfx" 
    Title="MainWindow" 
    Height="480" 
    Width="640">
    <Grid>
        <osm:WebControl
            x:Name="webControl"
            Source="http://google.com/" 
            PreviewKeyDown="WebControl_PreviewKeyDown" 
            PreviewKeyUp="WebControl_PreviewKeyUp" 
            PreviewTextInput="WebControl_PreviewTextInput"/>
    </Grid>
</Window>
{% endhighlight %}

#### C#  (*`MainWindow.xaml.cs`*)

{% highlight csharp %}
using System;
using System.Windows;
using System.Windows.Input;
using Awesomium.Core;
using Awesomium.Windows.Controls;

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void WebControl_PreviewKeyDown( object sender, KeyEventArgs e )
        {
            // Prevent passing TAB and Space to the view.
            if ( ( e.Key == Key.Tab ) || ( e.Key == Key.Space ) )
                e.Handled = true;

            // Translate right-arrow to a Space.
            if ( e.Key == Key.Right )
            {
                // Get the IWebView interface the control implements.
                IWebView webView = (IWebView)webControl;

                // Create a WPF KeyDown event for Space.
                KeyEventArgs ev = new KeyEventArgs( 
                    (KeyboardDevice)e.Device, 
                    e.InputSource, 
                    e.Timestamp, 
                    Key.Space );
                // The GetKeyboardEvent extensions provided by Awesomium.Windows.Controls.Utilities,
                // can translate this to a WebKeyboardEvent needed by Awesomium.
                WebKeyboardEvent webEvent = ev.GetKeyboardEvent( WebKeyboardEventType.KeyDown );
                // Inject the event, simulating a Space hit.
                webView.InjectKeyboardEvent( webEvent );

                // A normal Space hit, would be followed by a PreviewTextInput event.
                // We need to simulate this too.
                TextCompositionEventArgs txtEv = new TextCompositionEventArgs( 
                    e.Device, 
                    new TextComposition( InputManager.Current, webControl, " " ) );

                if ( txtEv.GetKeyboardEvent() != null )
                {
                    // Get the equivalent WebKeyboardEvent.
                    webEvent = (WebKeyboardEvent)txtEv.GetKeyboardEvent();
                    // Inject it to the view.
                    webView.InjectKeyboardEvent( webEvent );
                }

                // Consume the right-arrow hit. We translated it to a Space.
                e.Handled = true;
            }
        }

        private void WebControl_PreviewKeyUp( object sender, KeyEventArgs e )
        {
            // Prevent passing TAB and Space to the view.
            if ( ( e.Key == Key.Tab ) || ( e.Key == Key.Space ) )
                e.Handled = true;

            if ( e.Key == Key.Right )
            {
                // We translate this to a Space in PreviewKeyDown.
                // A KeyUp is not needed for Space.
                e.Handled = true;
            }
        }

        private void WebControl_PreviewTextInput( object sender, TextCompositionEventArgs e )
        {
            // Should never be fired since we handle Space
            // in KeyDown, but just in case.
            if ( e.Text == " " )
                e.Handled = true;
        }
    }
}
{% endhighlight %}

