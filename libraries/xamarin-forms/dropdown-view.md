# Dropdown List

## 1) By Xamarin Forms Picker (Easy solution for a same approach on ios/android)
https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/picker/populating-itemssource
https://www.hiimray.co.uk/2019/10/14/xamarin-forms-quick-and-easy-custom-picker-with-a-more-traditional-look/297
https://stackoverflow.com/questions/46469253/xamarin-forms-picker-selecteditem-not-firing

### View on Xamarin Forms
#### View Xaml
```
...
<Frame 
    Margin="10, 5" Padding="0,0"
    HeightRequest="50"
    BorderColor="{StaticResource LightBackground}" CornerRadius="5" HasShadow="False"
    HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand"
    >
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="1" />
            <ColumnDefinition Width="48" />
            <ColumnDefinition Width="1" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>

        <components:CustomDropdownPicker 
            Grid.Row="0" Grid.Column="0"
            x:Name="DropdownRegions"
            Margin="10, 0, 10, 0"
            TextColor="{StaticResource xxxBlue}"
            HorizontalOptions="FillAndExpand" VerticalOptions="Center" 
            SelectedIndexChanged="DropdownRegions_OnSelectedIndexChanged"
            />
        <BoxView
            Grid.Row="0" Grid.Column="1"
            BackgroundColor="Transparent"
            HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand"
            />
        <Button 
            Grid.Row="0" Grid.Column="2" 
            x:Name="BtnDropdown"
            Margin="0" WidthRequest="48"
            BackgroundColor="Transparent" 
            BorderRadius="0" BorderWidth="0" 
            VerticalOptions="Center" HorizontalOptions="Center" 
            >
            <Button.ImageSource>
                <FontImageSource 
                    FontFamily="{StaticResource FontAwesomeLight}" 
                    Glyph="&#xf0d7;"
                    Color="#ff0000"
                    Size="16"
                    />
            </Button.ImageSource>
        </Button>
    </Grid>

</Frame>
```
#### View Function
```
...
protected override void OnAppearing()
{
    ViewModel.PropertyChanged += ViewModelChanged;

    base.OnAppearing(); // timing to call ViewModels' ViewAppeared()
    ButtonDropdown.Clicked += DropdownRegions_ButtonClicked;
    ...
}

protected override void OnDisappearing()
{
    ...
    ButtonDropdown.Clicked -= DropdownRegions_ButtonClicked;
    ViewModel.PropertyChanged -= this.ViewModelChanged;
    base.OnDisappearing();
}


private void ViewModelChanged(object sender, PropertyChangedEventArgs args)
{
    if (args.PropertyName == nameof(ViewModel.SelectedRegion))
    {
        Device.BeginInvokeOnMainThread(() => {
            var regions = ViewModel.Regions;
            foreach (var region in regions)
            {
                region.IsSelected = region.Name == ViewModel.SelectedRegion.Name;
            }
            Update(regions);
        });
    }
}

private void Update(IEnumerable<ServerEndpoint> servers)
{
    var listRegions = servers.Select(r => r.Name).ToList();

    DropdownRegions.ItemsSource = listRegions;
    if (DropdownRegions.ItemsSource.Count > 0 && DropdownRegions.SelectedIndex == -1)
    {
        DropdownRegions.SelectedIndex = 0;
    }
}

private void DropdownRegions_OnSelectedIndexChanged(object sender, EventArgs e)
{
    if (DropdownRegions.SelectedIndex == -1) return;

    var selectedRegion = DropdownRegions.SelectedItem as string;
    if (string.IsNullOrEmpty(selectedRegion) 
        || !ViewModel.Regions.Any()) return;

    ViewModel.SelectLoginRegion(ViewModel.Regions.FirstOrDefault(l 
        => l.Name.Equals(selectedRegion)));
}

private void DropdownRegions_ButtonClicked(object sender, EventArgs e)
{
    DropdownRegions.Focus();
}

```
> ViewModel.PropertyChanged needs to call before On.Appearing() to fire the PropertyChange event so that Update can set the initial Load and Index.
> DropdownRegions_OnSelectedIndexChanged is inherited by Xamarin Picker library and set on cutom renderer on Xaml.

### ViewModel on Xamarin Forms
```
public class ServerEndpoint
{
    public ServerEndpoint(string name, string serverId)
    {
        Name = name;
        ServerId = serverId;
    }

    public string Name { get; }
    public string ServerId { get; }
    public bool IsSelected { get; set; }

    public override bool Equals(object obj)
    {
        return obj is ServerEndpoint &&
               ((ServerEndpoint)obj).ServerId == ServerId;
    }

    public override int GetHashCode()
    {
        return ServerId != null ? ServerId.GetHashCode() : 0;
    }

    public override string ToString()
    {
        return string.Format("{0}", Name);
    }
}

...
public ObservableCollection<ServerEndpoint> Regions { get; } = new ObservableCollection<ServerEndpoint>();
public ServerEndpoint SelectedRegion { get; set; }

private IEnumerable<ServerEndpoint> GetServerEndpoints()
{
    ...
    return servers;
}

private void LoadRegions()
{
    Regions.Clear();
    SelectedRegion = CreateLoginEndpointViewModel(_apiEndpointService.Selected);

    foreach (var location in GetServerEndpoints())
    {
        Regions.Add(location);
    }

    RaisePropertyChanged(() => this.SelectedRegion);
}

public void SelectRegion(ServerEndpoint serverEndpoint)
{
    // set the selected region to a new server config
}
```

### Component Model for Native
```
...
public class CustomDropdownPicker : Picker
{
    public CustomDropdownPicker() : base()
    {
    }
}
```

### Native (Android)
```
...
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;
using PickerRenderer = Xamarin.Forms.Platform.Android.AppCompat.PickerRenderer;

[assembly: ExportRenderer(typeof(CustomDropdownPicker), typeof(CustomDropdownPickerRenderer))]
namespace [project].Droid.Renderers
{
    public class CustomDropdownPickerRenderer : PickerRenderer
    {
        public CustomDropdownPickerRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Picker> e)
        {
            base.OnElementChanged(e);
            if (e.OldElement == null)
            {
                Control.Background = null;
            }
        }
    }
}
```
> PickerRenderer is set from Android.AppCompat as a defualt, using should be linked with this for PickerRender so that Control will work properly.
> Custom Renderer of Picker needs to set with new ctor with Context. (constructor without context has been deprecated)

### Native (iOS)
...
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using Picker = Xamarin.Forms.Picker;

[assembly: ExportRenderer(typeof(CustomDropdownPicker), typeof(CustomDropdownPickerRenderer))]
namespace [project].iOS.Renderers
{
    public class CustomDropdownPickerRenderer : PickerRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Picker> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
                return;

            Control.Layer.BorderWidth = 0f;
            Control.BorderStyle = UITextBorderStyle.None;
            
        }
    }
}
    
## 2) By ViewRenderer with Spinner (Android)

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
