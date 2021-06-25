# FontAwesome

```xaml
```
```c#
```

## Environment
### Setup the fonts and projects
> ```diff 
> ! if ios has an issue to draw fontimage on deployment build, please see the appendix at this end of article. 
> ```

- Download for the desktop : https://fontawesome.com/v5.15/how-to-use/on-the-desktop
- add the three otf files into project resource folder
  > FileName should be matching with configuration so make it without space.
  > VS for mac is hassle free to add imediatly the files to project, and check the 'build action' type.
  > files need to be located at Android : ``` proj.Android > Assets (AndroidAsset) ``` and IOS : ``` proj.ios > Resources (Bundle Resource) ```
  > ![image](https://user-images.githubusercontent.com/59367560/121589161-aaee9180-ca2e-11eb-86fc-6ae6ad595b60.png)
  > ![image](https://user-images.githubusercontent.com/59367560/121592098-2dc51b80-ca32-11eb-866a-fa8ac4c2d591.png)


- [ios only] add the below code into 'info.plist' right below <Dict> tag
  ```xml
  <key>UIAppFonts</key>
  <array>
    <string>FontAwesome5BrandsRegular.otf</string>
    <string>FontAwesome5Regular.otf</string>
    <string>FontAwesome5Solid.otf</string>
  </array>
  ```
  
- add the below code into App.xaml
  > Key string should be same as the otf file name
  ```xaml
  <Application.Resources>
  <ResourceDictionary> 
    <OnPlatform x:TypeArguments="x:String" 
                x:Key="FontAwesomeBrands">
      <On Platform="Android" 
          Value="FontAwesome5Brands.otf#Regular" />
      <On Platform="iOS" 
          Value="FontAwesome5Brands-Regular" />
      <On Platform="UWP" 
          Value="/Assets/FontAwesome5Brands.otf#Font Awesome 5 Brands" />
    </OnPlatform> 
    
    <OnPlatform x:TypeArguments="x:String" 
                x:Key="FontAwesomeSolid"> 
      <On Platform="Android" 
          Value="FontAwesome5Solid.otf#Regular" /> 
      <On Platform="iOS" 
          Value="FontAwesome5Free-Solid" /> 
      <On Platform="UWP" 
          Value="/Assets/FontAwesome5Solid.otf#Font Awesome 5 Free" /> 
    </OnPlatform> 
    
    <OnPlatform x:TypeArguments="x:String" 
                x:Key="FontAwesomeRegular">
      <On Platform="Android" 
          Value="FontAwesome5Regular.otf#Regular" /> 
      <On Platform="iOS" 
          Value="FontAwesome5Free-Regular" /> 
      <On Platform="UWP" 
          Value="/Assets/FontAwesome5Regular.otf#Font Awesome 5 Free" /> 
    </OnPlatform>
  </ResourceDictionary>
  </Application.Resources>
  ```

- find an icon and copy the unicode : https://fontawesome.com/icons?d=gallery&m=free
  
  
## Usage : XAML (in page content)
```xaml
<Label Text="&#xf26e;"
       FontFamily="{StaticResource FontAwesomeBrands}" />
```

## Usage : XAML (in Page, mvvm)
```xaml
<?xml version="1.0" encoding="UTF-8"?>
<views:MvxContentPage  xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
                       xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Domain.Proj.Pages.ViewAA"
                       x:TypeArguments="viewModels:AAViewModel"
                       IconImageSource="{FontImage FontFamily={StaticResource FontAwesomeSolid}, Color=Gray, Size=Large, Glyph=&#xf080;}"
                       xmlns:Component="clr-namespace:Domain.Proj.Components"
                       Title="AAA"
                       xmlns:viewModels="clr-namespace:Domain.Proj.Core.ViewModels;assembly=Domain.Proj.Core"
                       xmlns:Navigation="clr-namespace:Domain.Proj.Views.Navigation">
```

## Usage : XAML (in Page, mvvm)
[MyIcon.xaml]
```xaml 
<?xml version="1.0" encoding="UTF-8" ?>
<ContentView
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Domain.Proj.MyIcon">
  <ContentView.Content>
    <Image x:Name="image" Aspect="AspectFit">
      <Image.GestureRecognizers>
        <TapGestureRecognizer Tapped="OnClick" />
      </Image.GestureRecognizers>
    </Image>
  </ContentView.Content>
</ContentView>
```

[MyIcon.xaml.cs]
```c# 
using System;
using System.Collections.Generic;
using System.Windows.Input;
using Xamarin.Forms;

namespace Domain.Proj.Components
{
    public enum MyIconSize
    {
        Small = 20,
        Large = 40,
    }
    public partial class MyIcon : ContentView
    {
        public static BindableProperty SourceProperty = BindableProperty.Create(nameof(Source), typeof(string), typeof(MyIcon), "", propertyChanged: UpdateImage);
        public static BindableProperty SizeProperty = BindableProperty.Create(nameof(Size), typeof(CustomIconSize), typeof(MyIcon), MyIconSize.Large, propertyChanged: UpdateSize);

        public string Source
        {
            get { return (string)GetValue(SourceProperty); }
            set { SetValue(SourceProperty, value); }
        }

        public MyIconSize Size
        {
            get { return (MyIconSize)GetValue(SizeProperty); }
            set { SetValue(SizeProperty, value); }
        }

        public MyIcon()
        {
            InitializeComponent();
        }

        private static void UpdateImage(BindableObject bindable, object prev, object curr)
        {
            var prevText = (string)prev;
            var currText = (string)curr;

            if (oldText == null || !oldText.Equals(currText))
            {
                var view = (CustomIcon)bindable;
                view.image.Source = new FontImageSource
                {
                   FontFamily = (Xamarin.Forms.OnPlatform<string>)Application.Current.Resources["FontAwesomeRegular"],
                   Color = Color.FromHex("#2cde6a"),
                   Glyph = "\uf28d",
                   Size = 20
                };
            }
        }

        public static BindableProperty TappedProperty =
            BindableProperty.Create(nameof(Tapped), typeof(ICommand), typeof(CustomIcon), null);

:
:
```
  
### [Appendix] IOS fontimage issue on deployment build
The above environment setup of a font resouce on IOS was an issue on deployment build even though it's working fine on my local build.
It mightbe ios seems not to dealing with the font resource as a embedded resource, and not properly linking with the resource on deployment build compile time.
  
#### A trial and change was as follows :
  
- change the build action on ios from 'BundleResource' to 'EmbeddedResource'
- remove the below info.plist AppFonts section (it's no need anymore)
  ```xml
  <key>UIAppFonts</key>
  <array>
    <string>FontAwesome5BrandsRegular.otf</string>
    <string>FontAwesome5Regular.otf</string>
    <string>FontAwesome5Solid.otf</string>
  </array>
  ```
- add the below assembly annotation in AssemblyInfo.cs
  ```c#
  [assembly: ExportFont("FontAwesome6ProLight.otf", Alias = "FontAwesomeLight")]
  ```
- use this Alias on App.xaml updating the platform branch code
  ```c#
  <ResourceDictionary> 
    <OnPlatform x:TypeArguments="x:String" 
                x:Key="FontAwesomeLight">
      <On Platform="Android" 
          Value="FontAwesome6ProLight.otf#Light" />
      <On Platform="iOS" 
          Value="FontAwesomeLight" />
    </OnPlatform>
  </ResourceDictionary>
  ```
  
![image](https://user-images.githubusercontent.com/59367560/123411214-1b0d2380-d5a8-11eb-802c-f1182687a5ff.png)

