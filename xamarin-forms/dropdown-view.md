# Dropdown List View 

## by Xamarin Forms Picker
https://www.hiimray.co.uk/2019/10/14/xamarin-forms-quick-and-easy-custom-picker-with-a-more-traditional-look/297
https://stackoverflow.com/questions/46469253/xamarin-forms-picker-selecteditem-not-firing


## Another way to create Dropdown List creation on Android

### Component Model on Xamarin Forms for DropdownList
```
sing System;
using System.Collections.Generic;
using Xamarin.Forms;

namespace [project].Components
{
    public class DropdownListView : View
    {
        public static readonly BindableProperty ItemsSourceProperty = BindableProperty.Create(
            propertyName: nameof(ItemsSource),
            returnType: typeof(List<string>),
            declaringType: typeof(List<string>),
            defaultValue: null);

        public static readonly BindableProperty SelectedIndexProperty = BindableProperty.Create(
            propertyName: nameof(SelectedIndex),
            returnType: typeof(int),
            declaringType: typeof(int),
            defaultValue: -1);

        public List<string> ItemsSource
        {
            get => (List<string>)GetValue(ItemsSourceProperty);
            set => SetValue(ItemsSourceProperty, value);
        }

        public int SelectedIndex { 
            get => (int)GetValue(SelectedIndexProperty);
            set => SetValue(SelectedIndexProperty, value);
        }

        public event EventHandler<ItemSelectedEventArgs> ItemSelected;

        public void OnItemSelected(int position)
        {
            ItemSelected?.Invoke(this, new ItemSelectedEventArgs()
            {
                SelectedIndex = position
            });
        }

    }

    public class ItemSelectedEventArgs : EventArgs
    {
        public int SelectedIndex { get; set; }
    }
}
```

### View on Xamarin Forms
```
...
<Frame Margin="10, 5" Padding="5,0" 
     HeightRequest="50"
     BorderColor="{StaticResource LightBackground}" CornerRadius="5" HasShadow="False"
     HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">

  <components:DropdownListView
    x:Name="DropdownRegions"
    BackgroundColor="{StaticResource Transparent}"
    VerticalOptions="Center" HorizontalOptions="FillAndExpand"
    />
...
```
