---
layout: page
title : Walkthrough: Creating an Awesomium WPF Application
group: WPF
weight: 3

---

In this walkthrough you create a browser-like WPF application that uses the Awesomium WPF `WebControl` to render web content, as well as supporting WPF components provided by *`Awesomium.Windows.Controls`*.

> For details about the usage of the WPF `WebControl` and additional Awesomium WPF components, read: [Using the WPF WebControl](webcontrol.html)

In this walkthrough, you perform the following tasks:

* Create a WPF application that uses Awesomium.
* Initialize the Awesomium `WebCore`.
* Create a window to host a WPF `WebControl`.
* Create the UI to be bound to the `WebControl`.
* Handle important `WebControl` events.
* Test Awesomium WPF features.

### Prerequisites

You need the following components to complete this walkthrough:

* Awesomium SDK (available [here](http://www.awesomium.com/download/))
* Visual Studio (2013, 2012 or 2010, Express or full edition)

### Create a WPF application that uses Awesomium

**To create the project**

1. In Visual Studio, hit **New Project** and in the New Project dialog make sure the target framework is **NET Framework 4.0** (Full or Client Profile) **or newer**.
2. Create a new **WPF Application** project in Visual Basic or Visual C# named **StarterSample**.
3. Open the project's properties window and in the **Build** tab (for Visual C#) or the **Compile** tab (for Visual Basic), make sure your project targets **x86**.
4. Add a reference to the following assemblies:
    * *`Awesomium.Core`*
    * *`Awesomium.Windows.Controls`*

  In the **Reference Manager** dialog, you should be able to find the assemblies under Assemblies -> Extensions.
5. In **Solution Explorer**, change the name of the application's main window to **MainWindow.xaml**.

### Initialize the Awesomium WebCore

In this procedure you explicitly initialize the Awesomium [`WebCore`](http://docs.awesomium.net/?tc=T_Awesomium_Core_WebCore). The WebCore manages the creation and lifetime of all IWebView and WebSession instances and maintains useful services like:

 * Auto-Update
 * Network stack
 * Inter-process messaging
 * Surface creation
 * Download operations

Generally, you should initialize the `WebCore` providing your custom configuration settings, before creating any views and shut it down at the end of your program.

> If you do not explicitly initialize the `WebCore`, the core will automatically start, using default configuration settings, when you create the first [`IWebView`](../general-use/introduction-to-web-views.html) instance or when you create the first [`WebSession`](../general-use/using-web-sessions.html).

**To initialize and shutdown the WebCore**

1. Open the *`App.xaml.cs`* or *`Application.xaml.vb`* file.
2. Add the following namespaces to the top of the file. Replace the existing ones if there are any.

  {% highlight csharp %}
  using System;
  using System.Linq;
  using Awesomium.Core;
  using System.Windows;
  using System.Collections.Generic;
  {% endhighlight %}

3. Override the `OnStartup` method. This is where you explicitly initialize the `WebCore`.

  {% highlight csharp %}
  protected override void OnStartup( StartupEventArgs e )
  {
      // Initialization must be performed here,
      // before creating a WebControl.
      if ( !WebCore.IsInitialized )
        {
          WebCore.Initialize( new WebConfig()
          {
              HomeURL = "https://www.awesomium.com".ToUri(),
              LogPath = @".\starter.log",
              LogLevel = LogLevel.Verbose
          } );
      }

      base.OnStartup( e );
  }
  {% endhighlight %}

4. Override the `OnExit` method. This is where you shutdown the `WebCore`.

  {% highlight csharp %}
  protected override void OnExit( ExitEventArgs e )
  {
      // Make sure we shutdown the core last.
      if ( WebCore.IsInitialized )
          WebCore.Shutdown();

      base.OnExit( e );
  }
  {% endhighlight %}

5. Save changes and close the file.

### Create a Window hosting a WPF WebControl.

In this procedure you edit the application's main `Window` to create a window that will host the WPF `WebControl` and supporting UI. This window will be used as the application's main window, as well as a child (popup) window for displaying external links and JavaScript *`window.open`* calls.

**To create the window**

1. Open the *`MainWindow.xaml.cs`* or *`MainWindow.xaml.vb`* code file.

  The file opens in the Code Editor.
2. Add the following namespaces to the top of the file. Replace the existing ones if there are any.

  {% highlight csharp %}
  using System;
  using System.Linq;
  using System.Windows;
  using Awesomium.Core;
  using Awesomium.Windows.Controls;
  using System.Collections.Generic;
  {% endhighlight %}

3. Add the following dependency properties to the file, after the constructor that is automatically added by Visual Studio.

  {% highlight csharp %}
  // This will be set to the target URL, when this window does not
  // host a created child view. The WebControl, is bound to this property.
  public Uri Source
  {
      get { return (Uri)GetValue( SourceProperty ); }
      set { SetValue( SourceProperty, value ); }
  }
  
  // Identifies the <see cref="Source"/> dependency property.
  public static readonly DependencyProperty SourceProperty =
      DependencyProperty.Register( "Source",
      typeof( Uri ), typeof( MainWindow ),
      new FrameworkPropertyMetadata( null ) );
  
  
  // This will be set to the created child view that the WebControl will wrap,
  // when ShowCreatedWebView is the result of 'window.open'. The WebControl, 
  // is bound to this property.
  public IntPtr NativeView
  {
      get { return (IntPtr)GetValue( NativeViewProperty ); }
      private set { this.SetValue( MainWindow.NativeViewPropertyKey, value ); }
  }
  
  private static readonly DependencyPropertyKey NativeViewPropertyKey =
      DependencyProperty.RegisterReadOnly( "NativeView",
      typeof( IntPtr ), typeof( MainWindow ),
      new FrameworkPropertyMetadata( IntPtr.Zero ) );
  
  // Identifies the <see cref="NativeView"/> dependency property.
  public static readonly DependencyProperty NativeViewProperty =
      NativeViewPropertyKey.DependencyProperty;
  
  // The visibility of the address bar and status bar, depends
  // on the value of this property. We set this to false when
  // the window wraps a WebControl that is the result of JavaScript
  // 'window.open'.
  public bool IsRegularWindow
  {
      get { return (bool)GetValue( IsRegularWindowProperty ); }
      private set { this.SetValue( MainWindow.IsRegularWindowPropertyKey, value ); }
  }
  
  private static readonly DependencyPropertyKey IsRegularWindowPropertyKey =
      DependencyProperty.RegisterReadOnly( "IsRegularWindow",
      typeof( bool ), typeof( MainWindow ),
      new FrameworkPropertyMetadata( true ) );
  
  // Identifies the <see cref="IsRegularWindow"/> dependency property.
  public static readonly DependencyProperty IsRegularWindowProperty =
      IsRegularWindowPropertyKey.DependencyProperty;
  {% endhighlight %}

4. Save the file and close the Code Editor of `MainWindow`.

### Create the UI to be bound to the WebControl

In this procedure you create the user interface of the application's main `Window`. The interface includes the WPF `WebControl` and supporting elements that bind to actions of the `WebControl`, as well as a [`WebSessionProvider`](http://docs.awesomium.net/?tc=T_Awesomium_Windows_Controls_WebSessionProvider) that allows you to assign a `WebSession` with certain preferences to the `WebControl`.

This window will be used as the application's main window, as well as a child (popup) window for displaying external links and JavaScript *`window.open`* calls.

**To create the UI**

1. In XAML view for **_`MainWindow.xaml`_**, replace the existing XAML with the following:

  {% highlight xml %}
  <Window 
      x:Name="webWindow"
      x:Class="StarterSample.MainWindow" 
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
      xmlns:awe="http://schemas.awesomium.com/winfx"
      xmlns:data="http://schemas.awesomium.com/winfx/data"
      xmlns:core="clr-namespace:Awesomium.Core;assembly=Awesomium.Core"
      WindowStartupLocation="CenterScreen"
      Title="{Binding Title, ElementName=webControl}"
      Height="700" 
      Width="1200">
      <Window.Resources>
          <awe:WebSessionProvider x:Key="webSession" DataPath=".\Cache">
              <core:WebPreferences 
                  ShrinkStandaloneImagesToFit="False"
                  SmoothScrolling="True" />
          </awe:WebSessionProvider>
  
          <awe:UrlConverter x:Key="UrlConverter" />
          <BooleanToVisibilityConverter x:Key="booleanToVisibilityConverter" />
      </Window.Resources>
      <awe:WebDialogsLayer>
          <Grid>
              <Grid.RowDefinitions>
                  <RowDefinition Height="Auto"/>
                  <RowDefinition />
                  <RowDefinition Height="Auto" />
              </Grid.RowDefinitions>
              <DockPanel 
                  LastChildFill="True" 
                  Height="30"
                  Visibility="{Binding IsRegularWindow, ElementName=webWindow, Converter={StaticResource booleanToVisibilityConverter}}">
                  <Button 
                      Content="Back" 
                      Width="60" 
                      Command="{x:Static NavigationCommands.BrowseBack}" 
                      CommandTarget="{Binding ElementName=webControl}"/>
                  <Button 
                      Content="Forward" 
                      Width="60" 
                      Command="{x:Static NavigationCommands.BrowseForward}" 
                      CommandTarget="{Binding ElementName=webControl}"/>
                  <Button 
                      Content="Reload" 
                      Width="60" 
                      Command="{x:Static NavigationCommands.Refresh}" 
                      CommandParameter="False"
                      CommandTarget="{Binding ElementName=webControl}"/>
                  <Button 
                      Content="Home" 
                      Width="60" 
                      Command="{x:Static NavigationCommands.BrowseHome}"
                      CommandTarget="{Binding ElementName=webControl}"/>
                  <TextBox 
                      FontSize="16"
                      Padding="3,0"
                      VerticalContentAlignment="Center"
                      TextWrapping="NoWrap"
                      Text="{data:SourceBinding webControl}" />
              </DockPanel>
              <awe:WebControl 
                  Grid.Row="1"
                  x:Name="webControl"
                  NativeView="{Binding NativeView, ElementName=webWindow}"
                  WebSession="{Binding Source={StaticResource webSession}}"
                  Source="{Binding Source, ElementName=webWindow}"/>
              <StatusBar
                  Grid.Row="2" 
                  Height="25" 
                  BorderBrush="{DynamicResource {x:Static SystemColors.ActiveBorderBrushKey}}" 
                  BorderThickness="0,1,0,0"
                  Visibility="{Binding IsRegularWindow, ElementName=webWindow, Converter={StaticResource booleanToVisibilityConverter}}">
                  <StatusBarItem>
                      <TextBlock 
                          VerticalAlignment="Center" 
                          Padding="3" 
                          TextWrapping="NoWrap" 
                          TextTrimming="CharacterEllipsis" 
                          Text="{Binding TargetURL, ElementName=webControl, Converter={StaticResource UrlConverter}}"/>
                  </StatusBarItem>
                  <StatusBarItem HorizontalAlignment="Right">
                      <StackPanel Orientation="Horizontal">
                          <TextBlock 
                              VerticalAlignment="Center"
                              Margin="7,0" 
                              Text="Zoom:"/>
                          <Slider 
                              DataContext="{Binding ElementName=webControl}" 
                              Margin="3,0" 
                              Minimum="10" 
                              Maximum="400" 
                              Width="120"
                              VerticalAlignment="Center" 
                              Value="{Binding Zoom}" 
                              AutoToolTipPlacement="TopLeft" 
                              IsSnapToTickEnabled="True" 
                              IsMoveToPointEnabled="True" 
                              SmallChange="1" 
                              LargeChange="10" 
                              TickFrequency="10" 
                              Focusable="False" 
                              ToolTip="{Binding Zoom}">
                              <Slider.ContextMenu>
                                  <ContextMenu DataContext="{Binding PlacementTarget.DataContext, RelativeSource={RelativeSource Self}}">
                                      <MenuItem Command="{x:Static awe:WebControlCommands.ResetZoom}" CommandTarget="{Binding}" />
                                  </ContextMenu>
                              </Slider.ContextMenu>
                          </Slider>
                      </StackPanel>
                  </StatusBarItem>
              </StatusBar>
          </Grid>
      </awe:WebDialogsLayer>
  </Window>
  {% endhighlight %}

2. Save and close the file.

For details about designing the contents of the window, please read the following articles:

* [Using the WPF WebControl](webcontrol.html)
* [Using the WebSessionProvider](websessionprovider.html)
* [Using DataSourceProviders](datasourceprovider.html)
* [How to: Create WPF UI that binds to WebControl actions](howto-ui.html)
* [How to: Create a WPF TextBox that acts as an Address-Box](howto-addressbox.html)

