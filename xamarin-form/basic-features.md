# Basic Feature in Xamarin.Forms

```xaml
```
```c#
```

## Navigation
### Stack Navigation
move a subpage and back to the Main

[App.xaml.cs]
```c#
public App()
{
    InitializeComponent();

    MainPage = new NavigationPage(new NavigationPageView());
}
```

[NavigationPageView.xaml]
```xaml
<ContentPage ... >
  <StackLayout>
    <Button
      Text="aaa"
      x:Name="aaa"
      Clicked="aaaHandler"
    <Button
      Text="bbb"
      x:Name="bbb"
      Clicked="bbbHandler"
  </StackLayout>
</ContentPage>
```

[NavigationPageView.xaml.cs]
```c#
public partial class NavigationPageView : ContentPage
{
    public NavigationPageView()
    {
        InitializeComponent();
    }

    private async void aaaHandler(object sender, EventArgs e)
    {
        await Navigation.PushAsync(new Screens.aaaPageView());
    }

:
:
```

> ContentPages X.Forms template need to be created at [Screens > aaaPageView.xaml / .xaml.cs]

### Tabbed Navigation
place a set of Tab button and update the page
[App.xaml.cs]
```c#
public App()
{
    InitializeComponent();

    MainPage = new NavigationPageView();
}
```

[NavigationPageView.xaml]
```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:screens="clr-namespace:[project name].Screens"
             x:Class="[project name].NavigationPageView">
  <screens:aaaPageView></screens:aaaPageView>
  <screens:bbbPageView></screens:bbbPageView>
</TabbedPage>
```
> in order to access another namespace class, xmlns notation is required

[NavigationPageView.xaml.cs]
```c#
public partial class NavigationPageView : TabbedPage
{
    public NavigationPageView()
    {
        InitializeComponent();
    }

:
:
```
> Handler is not required of tab link.
> But, ContentPages X.Forms template need to be created at [Screens > aaaPageView.xaml / .xaml.cs]

### Drawer(Master/Detail) Navigation
Hamburger Icon and show master menu list and change Detail page
[App.xaml.cs]
```c#
public App()
{
    InitializeComponent();

    MainPage = new NavigationPageView();
}
```

[MasterNavigationItem.cs]
```c#
public class MasterNavigationItem
{
    public string Title { get; set; }
    public string Icon { get; set; }
    public Type Target { get; set; }
}
```

[Screens > MasterPageView.xaml]
```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:[proj name]="clr-namespace:[proj name];assembly=[proj name]"
             xmlns:screens="clr-namespace:[proj name].Screens;assembly=[proj name]"
             x:Class="[proj name].Screens.MasterPageView"
             Title="Navigation">
  <StackLayout>
    <ListView x:Name="NavigationListView" x:FieldModifier="public">
      <ListView.ItemsSource>
        <x:Array Type="{x:Type [proj name]:MasterNavigationItem}">
          <[proj name]:MasterNavigationItem 
            Title="aaa" 
            Icon="aaa.png"
            Target="{x:Type screens:aaaPageView}" />
          <[proj name]:MasterNavigationItem 
            Title="bbb"
            Icon="bbb.png"
            Target="{x:Type screens:bbbPageView}" />
        </x:Array>
      </ListView.ItemsSource>
      <ListView.ItemTemplate>
        <DataTemplate>
          <ViewCell>
            <Grid Padding="5">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="30"></ColumnDefinition>
                <ColumnDefinition Width="*"></ColumnDefinition>
              </Grid.ColumnDefinitions>
              <Image Source="{Binding Icon}" />
              <Label Grid.Column="1" Text="{Binding Title}" />
            </Grid>
          </ViewCell>
        </DataTemplate>
      </ListView.ItemTemplate>
    </ListView>
  </StackLayout>
</ContentPage>
```

[NavigationPageView.xaml]
```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:screens="clr-namespace:[proj name].Screens;assembly=[proj name]"
             x:Class="[proj name].NavigationPageView">
             
  <MasterDetailPage.Master>
    <screens:MasterPageView x:Name="masterView"></helpers:MasterPageView>
  </MasterDetailPage.Master>
  
  <MasterDetailPage.Detail>
    <NavigationPage>
      <x:Arguments>
        <screens:aaaPageView></screens:aaaPageView>
      </x:Arguments>
    </NavigationPage>
  </MasterDetailPage.Detail>
  
</MasterDetailPage>
```

[NavigationPageView.xaml.cs]
```c#
public partial class NavigationPageView : MasterDetailPage
{
    public NavigationPageView()
    {
        InitializeComponent();
        
        masterView.NavigationListView.ItemSelected += NavigationListViewItemHandler;
    }

    private void NavigationListViewItemHandler(object sender, SelectedItemChangedEventArgs e)
    {
        if (e.SelectedItem is MasterNavigationItem item)
        {
            Detail = new NavigationPage((Page) Activator.CreateInstance(item.Target));
            masterView.NavigationListView.SelectedItem = null;
            IsPresented = false;
        }
    }

:
:
```

> ContentPages X.Forms template need to be created at [Screens > aaaPageView.xaml / .xaml.cs]

### Carousel Navigation
Slide pages to left and right
[App.xaml.cs]
```c#
public App()
{
    InitializeComponent();

    MainPage = new NavigationPageView();
}
```

[NavigationPageView.xaml]
```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:screens="clr-namespace:[proj name].Screens"
             x:Class="[proj name].CarouselPageView">
  <ContentPage>
    <Label FontSize="Large"
           VerticalOptions="CenterAndExpand"
           HorizontalOptions="CenterAndExpand">
      Welcome to Carousel Testing
    </Label>
  </ContentPage>
  <screens:aaaPageView></screens:aaaPageView>
  <screens:bbbPageView></screens:bbbPageView>
</CarouselPage>
```

[NavigationPageView.xaml.cs]
```c#
public partial class NavigationPageView : CarouselPage
{
    public NavigationPageView()
    {
        InitializeComponent();
    }

:
:
```

> ContentPages X.Forms template need to be created at [Screens > aaaPageView.xaml / .xaml.cs]

### Modal Navigation
Overlay the Modal View
[App.xaml.cs]
```c#
public App()
{
    InitializeComponent();

    MainPage = new NavigationPageView();
}
```

[NavigationPageView.xaml]
```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="[proj name].ModalPageView">
  <ContentPage.Content>
    <StackLayout>
      <Label Text="Hello, world!"
             VerticalOptions="CenterAndExpand"
             HorizontalOptions="CenterAndExpand" />
      <Button Text="View Larger Font"
              Clicked="ModalPageButtonHandler"></Button>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

[NavigationPageView.xaml.cs]
```c#
public partial class ModalPageView : ContentPage
{
    public ModalPageView()
    {
        InitializeComponent();
    }

    private async void ModalPageButtonHandler(object sender, EventArgs e)
    {
        await Navigation.PushModalAsync(new LargeTitlePageView());
    }
:
:
```

> ContentPages X.Forms template need to be created at [Screens > LargeTitlePageView.xaml / .xaml.cs]

## Layout
- Type of Layout 
  - with Single Child : ContentView, ScrollView, Frame, TemplatedView
    - ScrollView : it shows a smooth scrolling, combining with FlexLayout
      usage : <ScrollView><FlexLayout x:Name="flexLayout" Wrap="Wrap" JustifyContent="SpaceAround" >
  - Multiple Children : e.g. <StackLayout Orientation="Vertical" Spacing="5">
    1) StackLayout
       - spacing : space between items
       - Orientation : 'Vertifical' is default (e.g. Orientation="Horizontal")
    2) Grid : e.g. <>
       - Rows and columns : <RowDefinitions>, <ColumnDefinitions> (each definition's index starts from '0')
         usage : <BoxView Color="Red" Grid.Row="1" Grid.Column="0" />
         * nested Grid is available
       - Flexible sizing system : Absolute, Auto(child item's max size), Star(ratio) (the size filling order is Absolute -> Auto -> Star)
    3) RelativeLayout : a complicated layout type, but responsive
    4) AbsoluteLayout : not flexible, not commonly used.
    5) FlexLayout : X.Forms of version 3 add this option, based on CSS flex, more flexible than StackLayout
