---
layout: page
title : Web-View Initialization Sequence
group: General Use
weight: 5

---

### Events Sequence

It is important to understand the sequence of events that occur when a native web-view is being wrapped by an **Awesomium.NET** component. Below is a list of events in the Awesomium.NET API, that are being fired once during the initialization of all [IWebView](http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView) instances, in that order:

1. [IWebView.InitializeView](http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_InitializeView): Fired right before a native web-view is wrapped. This is the last moment that properties that cannot be changed when [IsLive](http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_IsLive) is `true`, can be set.
2. [WebCore.CreatedView](http://docs.awesomium.net/?tc=E_Awesomium_Core_WebCore_CreatedView): Fired when the new Web-View component is added to the [WebCore.Views](http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Views) collection.
3. [IWebView.NativeViewInitialized](http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_NativeViewInitialized): Fired after the native web-view is created, initialization is complete and settings have been applied to view.
4. [IWebView.ProcessCreated](http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ProcessCreated): Fired when it's certain that the child process of the view (*awesomium_process*), has been started. This is usually when the first page starts loading.

> All these events are only fired once for the lifetime of the component.

### Initialization Sequence

The following presents the detailed initialization sequence for all managed web-view components on MS Windows and helps you decide the moment certain operations can be performed on the component:

<dl>
<dt><h4><code>WebView</code> Component</h4></dt>

<dd>
<ol>
<li>
The component is being created (either through <a href="http://docs.awesomium.net/?tc=Overload_Awesomium_Core_WebCore_CreateWebView"><code>WebCore.CreateWebView</code></a> or, for child views, through the relevant <a href="http://docs.awesomium.net/html/M_Awesomium_Core_WebView__ctor.htm">constructor</a>).
<ul>
<li>The component is initialized.</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_IsLive"><code>IsLive</code></a> is <code>true</code>.</li>
<li>The component is added to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Views"><code>WebCore.Views</code></a>.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_WebCore_CreatedView"><code>WebCore.CreatedView</code></a> is fired.</li>
</ul>
All steps are performed simultaneously during construction. For <b>offscreen views only</b>, it is now safe to access or call members on the <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView"><code>IWebView</code></a> instance.
</li>
<li>For <i>windowed</i> views <b>you should now set the <a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_ParentWindow"><code>IWebView.ParentWindow</code></a> property</b> (the view cannot create a window until a parent is set).
<ul>
<li>You can now safely access or call members on the <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView"><code>IWebView</code></a> instance.</li>
</ul>
</li>
</ol>
</dd>

<hr/>
<dt><h4>WPF <code>WebControl</code> Component</h4></dt>

<dd>
<ol>
<li>The component is created.</li>
<li>
The component is initialized (<code>InitalizeComponent</code> of self and then of parent container, if any).</li>
<li>The visual tree is being built and Styles are applied (see <a href="http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement.onapplytemplate.aspx"><code>OnApplyTemplate</code></a>)<br />
- or -<br />
The control is loaded for presentation for the first time (see <a href="http://msdn.microsoft.com/en-us/library/system.windows.frameworkelement.loaded.aspx"><code>Loaded</code></a>).
</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_InitializeView"><code>IWebView.InitializeView</code></a> is fired. This is the moment to set any of the following properties, if they have not been set in the designer:
<ul>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_NativeView"><code>WebControl.NativeView</code></a> for <b>child views</b> (see <a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ShowCreatedWebView"><code>IWebView.ShowCreatedWebView</code></a>)</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_WebSession"><code>WebControl.WebSession</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a></li>
</ul>
</li>
<li>A new native web-view is instanciated and wrapped or, for child views, the native view assigned to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_NativeView"><code>WebControl.NativeView</code></a> is being wrapped.</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_IsLive"><code>IsLive</code></a> is <code>true</code>.
<ul>
<li>If <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a> is set to <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_WebViewType"><code>Offscreen</code></a> (default), it is now safe to access or call members on the <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView"><code>IWebView</code></a> instance.</li>
<li>The following properties can no longer be changed:
<ul>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_NativeView"><code>WebControl.NativeView</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_WebSession"><code>WebControl.WebSession</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a></li>
</ul>
</li>
</ul>
</li>
<li>The component is added to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Views"><code>WebCore.Views</code></a>.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_WebCore_CreatedView"><code>WebCore.CreatedView</code></a> is fired.</li>
<li>Additional initialization is performed, such as:
<ul>
<li>Applying settings specified at design-time (if <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a> is set to <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_WebViewType"><code>Offscreen</code></a>).</li>
<li>Preparing Javascript Interoperation.</li>
<li>Registering for events.</li>
<li>Creating internal helpers.</li>
</ul>
</li>
<li>If <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_ViewType"><code>WebControl.ViewType</code></a> is set to <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_WebViewType"><code>Window</code></a>:
<ul>
<li>The <code>HwndHost</code> hosting the <i>windowed</i> view is added to the visual tree and is initialized.</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_ParentWindow"><code>IWebView.ParentWindow</code></a> is internally set.</li>
<li>Settings specified at design-time are being applied.</li>
</ul>
</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_NativeViewInitialized"><code>IWebView.NativeViewInitialized</code></a> is fired.
<ul>
<li>You can now safely access or call members on the <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView"><code>IWebView</code></a> instance.</li>
<li>This is the right moment to start creating Global Javascript Objects (see <a href="http://docs.awesomium.net/?tc=M_Awesomium_Core_IWebView_CreateGlobalJavascriptObject"><code>IWebView.CreateGlobalJavascriptObject</code></a>).</li>
</ul>
</li>
<li>The source assigned to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Controls_WebControl_Source"><code>WebControl.Source</code></a> or the default blank page (<code>about:blank</code>) is being loaded.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ProcessCreated"><code>IWebView.ProcessCreated</code></a> is fired.
<ul>
<li>You can now start accessing the DOM of the loaded page. For subsequent navigations, wait for the <a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_DocumentReady"><code>IWebView.DocumentReady</code></a> event.</li>
</ul>
</li>
</ol>
</dd>

<hr/>
<dt><h4>WinForms <code>WebControl</code> Component</h4></dt>

<dd>
<ol>
<li>The component is created.</li>
<li>
The component is initialized (<code>InitalizeComponent</code> of self and then of parent container, if any). This is the right moment to set any of the following properties, if they have not been set in the designer:
<ul>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_NativeView"><code>WebControl.NativeView</code></a> for <b>child views</b> (see <a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ShowCreatedWebView"><code>IWebView.ShowCreatedWebView</code></a>)</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_WebSession"><code>WebControl.WebSession</code></a></li>
</ul>
</li>
<li>The control's handle is created and <a href="http://msdn.microsoft.com/en-us/library/system.windows.forms.control.handlecreated.aspx"><code>HandleCreated</code></a> is fired.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_InitializeView"><code>IWebView.InitializeView</code></a> is fired. This is the <b>latest</b> moment to set any of the following properties, if they have not been set in the designer:
<ul>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_NativeView"><code>WebControl.NativeView</code></a> for <b>child views</b> (see <a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ShowCreatedWebView"><code>IWebView.ShowCreatedWebView</code></a>)</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_WebSession"><code>WebControl.WebSession</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_ViewType"><code>WebControl.ViewType</code></a></li>
</ul></li>
<li>A new native web-view is instanciated and wrapped or, for child views, the native view assigned to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_NativeView"><code>WebControl.NativeView</code></a> is being wrapped.</li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_IWebView_IsLive"><code>IsLive</code></a> is <code>true</code>.
<ul>
<li>You can now safely access or call members on the <a href="http://docs.awesomium.net/?tc=T_Awesomium_Core_IWebView"><code>IWebView</code></a> instance.</li>
<li>The following properties can no longer be changed:
<ul>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_NativeView"><code>WebControl.NativeView</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_WebSession"><code>WebControl.WebSession</code></a></li>
<li><a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_ViewType"><code>WebControl.ViewType</code></a></li>
</ul>
</li>
</ul>
</li>
<li>The component is added to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Core_WebCore_Views"><code>WebCore.Views</code></a>.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_WebCore_CreatedView"><code>WebCore.CreatedView</code></a> is fired.</li>
<li>Additional initialization is performed, such as:
<ul>
<li>Applying settings specified at design-time.</li>
<li>Preparing Javascript Interoperation.</li>
<li>Registering for events.</li>
<li>Creating internal helpers.</li>
</ul>
</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_NativeViewInitialized"><code>IWebView.NativeViewInitialized</code></a> is fired.
<ul>
<li>This is the right moment to start creating Global Javascript Objects (see <a href="http://docs.awesomium.net/?tc=M_Awesomium_Core_IWebView_CreateGlobalJavascriptObject"><code>IWebView.CreateGlobalJavascriptObject</code></a>).</li>
</ul>
</li>
<li>The source assigned to <a href="http://docs.awesomium.net/?tc=P_Awesomium_Windows_Forms_WebControl_Source"><code>WebControl.Source</code></a> or the default blank page (<code>about:blank</code>) is being loaded.</li>
<li><a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_ProcessCreated"><code>IWebView.ProcessCreated</code></a> is fired.
<ul>
<li>You can now start accessing the DOM of the loaded page. For subsequent navigations, wait for the <a href="http://docs.awesomium.net/?tc=E_Awesomium_Core_IWebView_DocumentReady"><code>IWebView.DocumentReady</code></a> event.</li>
</ul>
</li>
</ol>
</dd>
</dl>


### Read some more articles

* [Introduction to Web-Views](introduction-to-web-views.html)
* [Using the WebView](using-the-webview.html)
* [Using the WPF WebControl](wpf-webcontrol.html)
* [Using the Windows Forms WebControl](winforms-webcontrol.html)
