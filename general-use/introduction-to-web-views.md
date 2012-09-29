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
<a href=""><code>WebControl</code</a>
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
