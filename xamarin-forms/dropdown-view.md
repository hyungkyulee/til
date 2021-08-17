# Dropdown List

## 1) By Xamarin Forms Picker (Easy solution for a same approach on ios/android)
https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/picker/populating-itemssource
https://www.hiimray.co.uk/2019/10/14/xamarin-forms-quick-and-easy-custom-picker-with-a-more-traditional-look/297
https://stackoverflow.com/questions/46469253/xamarin-forms-picker-selecteditem-not-firing


## 2) By ViewRenderer with Spinner (Android)

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
#### Xamarin
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

#### Function
```
...

// *** Test code for a string list set
// public bool IsItem1 { get; set; } = true;
// public List<string> Items2 { get; set; }
// public List<string> Items1 { get; set; }
        
public [constructor]()
{
    InitializeComponent();
    
    // *** Test code for a string list set
    // Items1 = new List<string>();
    // Items2 = new List<string>();
    //
    // for (int i = 0; i < 4; i++)
    // {
    //     Items1.Add(i.ToString());
    // }
    //
    // for (int i = 0; i < 10; i++)
    // {
    //     Items2.Add(i.ToString());
    // }
    // _locationViewModel = Mvx.IoCProvider.Resolve<LocationViewModel>();

    // DropdownRegions.ItemsSource = Items1;
}

protected override void OnAppearing()
{
    ViewModel.PropertyChanged += ViewModelChanged;

    base.OnAppearing();
    
    DropdownRegions.SelectedIndex = 0;
    DropdownRegions.ItemSelected += OnDropdownSelected;
    ...
}

private void OnDropdownSelected(object sender, ItemSelectedEventArgs e)
{
    // ** Test code with sample strings
    // LabelRegion.Text = IsItem1 ? Items1[e.SelectedIndex] : Items2[e.SelectedIndex];
    // LabelRegion.Text = DropdownRegions.ItemsSource[e.SelectedIndex];

    // ** Linking with viewModel data
    ViewModel.SelectLoginLocation(ViewModel.LoginLocations.FirstOrDefault(l => l.Name.Equals(DropdownRegions.ItemsSource[e.SelectedIndex])));
}

private void ViewModelChanged(object sender, PropertyChangedEventArgs args)
{
    if (args.PropertyName == nameof(ViewModel.SelectedLoginLocation))
    {
        Device.BeginInvokeOnMainThread(() => {
            var locations = ViewModel.LoginLocations;
            foreach (var loc in locations)
            {
                if(loc.Name == ViewModel.SelectedLoginLocation.Name)
                {
                    loc.IsSelected = true;
                }
                else
                {
                    loc.IsSelected = false;
                }
            }
            Update(locations);
        });
    }
}

private void Update(IEnumerable<EndpointViewModel> endpoints)
{
    // ** Test code with sample strings
    // var items = new List<CustomListItem>();
    // foreach (var item in endpoints)
    // {
    //     items.Add(new CustomListItem { Text = item.Name, IsSelected = item.IsSelected, Payload = item });
    // }
    //
    // DropdownRegions.ItemsSource = items;
    
    var listLocations = new List<string>();
    foreach (var location in endpoints)
    {
        listLocations.Add(location.Name);
    }

    DropdownRegions.ItemsSource = listLocations;
    if (DropdownRegions.ItemsSource.Count > 0 && DropdownRegions.SelectedIndex == -1)
    {
        DropdownRegions.SelectedIndex = 0;
    }
}

// ******* eventhandler inherited from Xamarin Picker
private void DropdownRegions_OnSelectedIndexChanged(object sender, EventArgs e)
{
    if (DropdownRegions.SelectedIndex == -1)
    {
        return;
    }

    SetSelectedRegion(DropdownRegions.SelectedItem as string);
}

private void SetSelectedRegion(string region)
{
    if (string.IsNullOrEmpty(region) 
        || !ViewModel.LoginLocations.Any()) return;

    ViewModel.SelectLoginLocation(ViewModel.LoginLocations.FirstOrDefault(l => l.Name.Equals(region)));
}
```

### Native code for Android
```
using System.ComponentModel;
using System.Linq;
using Android.Content;
using Android.Widget;
...
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(DropdownListView), typeof(DropdownListRenderer))]
namespace [project].Droid.Renderers
{
    public class DropdownListRenderer : ViewRenderer<DropdownListView, Spinner>
    {
        private Spinner spinner;

        public DropdownListRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<DropdownListView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                spinner = new Spinner(Context);
                SetNativeControl(spinner);
            }

            if (e.OldElement != null)
            {
                Control.ItemSelected -= OnItemSelected;
            }

            if (e.NewElement != null)
            {
                var view = e.NewElement;
                if (view.ItemsSource == null) return;
                
                var adapter = new ArrayAdapter(Context, 
                        // Android.Resource.Layout.SimpleListItem1,
                        Resource.Layout.DropdownSpinner, // you can make your own layout resource for dropdown
                        view.ItemsSource);
                Control.Adapter = adapter;

                if (view.SelectedIndex != -1)
                {
                    Control.SetSelection(view.SelectedIndex);
                }

                Control.ItemSelected += OnItemSelected;
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var view = Element;
            if (e.PropertyName == DropdownListView.ItemsSourceProperty.PropertyName)
            {
                var adapter = new ArrayAdapter(Context, Android.Resource.Layout.SimpleListItem1, view.ItemsSource);
                Control.Adapter = adapter;
            }
            
            if (e.PropertyName == DropdownListView.SelectedIndexProperty.PropertyName)
            {
                Control.SetSelection(view.SelectedIndex);
            }
            
            base.OnElementPropertyChanged(sender, e);
        }

        private void OnItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
        {
            var view = Element;
            if (view != null)
            {
                view.SelectedIndex = e.Position;
                view.OnItemSelected(e.Position);
            }
        }
    }
}
```

> more customisation of dropdown list view by layout resource
 [e.g. DropdownSpinner.xml]
  ```
    <?xml version="1.0" encoding="utf-8"?>
    <TextView
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:textSize="16sp"
      android:gravity="left"
      android:textColor="#404040"
      android:padding="8dip"
    />
  ```
