---
layout: page
title : Handling or Simulating User Input with the WPF WebControl
group: WPF
weight: 2

---

[external]: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAVklEQVR4Xn3PgQkAMQhDUXfqTu7kTtkpd5RA8AInfArtQ2iRXFWT2QedAfttj2FsPIOE1eCOlEuoWWjgzYaB/IkeGOrxXhqB+uA9Bfcm0lAZuh+YIeAD+cAqSz4kCMUAAAAASUVORK5CYII=

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

**Get the code, [here](https://gist.github.com/osmbot/9750442) ![][external]**.

### Additional Resources

* [Using the WPF WebControl](../wpf/webcontrol.html)



