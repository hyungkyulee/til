# FontAwesome

```xaml
```
```c#
```

## Environment
### Download the package
- Download for the desktop : https://fontawesome.com/v5.15/how-to-use/on-the-desktop
- add the three otf files into project resource folder
  > FileName should be matching with configuration so make it without space.
  > VS for mac is hassle free to add imediatly the files to project, and check the 'build action' type.
  > files need to be located at Android : ``` proj.Android > Assets ``` and IOS : ``` proj.ios > Resources ```
  > ![image](https://user-images.githubusercontent.com/59367560/121589161-aaee9180-ca2e-11eb-86fc-6ae6ad595b60.png)

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
