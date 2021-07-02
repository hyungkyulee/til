# SearchBar handling on Xamarin Forms

## Searchbar Custom Renderer (Usecase : border configuration)
Let's remove the border on Android because there is a default underline on searchbar.
> ref: https://stackoverflow.com/questions/46894420/search-bar-style-changes-in-xamarin-forms

### Common Part
Exmaple View.xaml
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             xmlns:extensions="clr-namespace:[YourNamespace];assembly=[proj name]">
    <ContentPage.Content>
      <extensions:ExtendedSearchBar Grid.Row="1" x:Name="SearchBar" TextChanged="OnTextChangeAsync" Placeholder="Search" BackgroundColor="White" />
      
      ...
      ...
```

New Custom controler (Renderer)
```xaml
using Xamarin.Forms;

namespace [Proj name].Extensions
{
    public class ExtendedSearchBar : SearchBar
    {
        public ExtendedSearchBar()
        {
        }
    }
}
```

### IOS Extended Renderer

```c#
...
protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
{
    if (Control != null)
    {
        Control.ShowsCancelButton = false;

        UITextField txSearchField = (UITextField)Control.ValueForKey(new Foundation.NSString("searchField"));
        txSearchField.BackgroundColor = UIColor.White;
        txSearchField.BorderStyle = UITextBorderStyle.None;
        txSearchField.Layer.BorderWidth = 1.0f;
        txSearchField.Layer.CornerRadius = 2.0f;
        txSearchField.Layer.BorderColor = UIColor.LightGray.CGColor;

    }
}
```
> Find textField of UISearchBar, and customise border style with this object.

### Android Extended Renderer

```c#
...
using Android.Graphics;
using Android.Widget;
...
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(ExtendedSearchBar), typeof(ExtendedSearchBarRenderer))]
namespace [proj name].Droid.Renderers
{
    public class ExtendedSearchBarRenderer : SearchBarRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<SearchBar> e)
        {
            base.OnElementChanged(e);
            
            if (Control != null)
            {
                var color = global::Xamarin.Forms.Color.LightGray;
                var searchView = Control as SearchView;

                var searchPlateId = searchView.Context.Resources.GetIdentifier("android:id/search_plate", null, null);
                var searchPlateView = (Android.Views.View) searchView.FindViewById(searchPlateId);
                searchPlateView.SetBackgroundColor(Android.Graphics.Color.Transparent);
            } 
        }
    }
}
```
