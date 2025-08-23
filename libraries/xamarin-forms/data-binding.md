# Data Binding with some options

## By Property-Changed Event

## By Messanger

## Reusable component to share data
Page1View - ComponentView - ViewModel (or Native Extenders)
Page2View _|

1. Call the common component View Method with a different value from page views
2. a different value in the method will update the component View presentation linked with feature(or native view extenders/renderers).
3. the update of feature's property(setter) is bindable in the feature view/model/or native extenders.
4. the bindable property(getter) can be used to handle a native feacture in android/ios

Each Page(e.g. Page1View.xaml) will have the componentView as a ```xmlns:views="..."```, and 
this will be able to handle in View's handler(e.g. Page1View.xaml.cs) by calling the Componentview's method with a different set of parameter according to the purpose of the PageView (e.g. SetPageFeatureView(param1, param2))

e.g. send View's specific(different) value to a common component view
```
[Page1Vew.xaml]
<ContentPage ...
  xmlns:views="clr-namespace:[proj name].Views"
  ... >

  <views:ComponentView x:Name="viewComponent" ... >
  ...
  
[Page1Vew.xaml.cs]
...
viewComponent.SetPageFeatureView(true, false)

```

The method of the ComponentView will link the value from the different sourced-View into the ComponentView Property.
e.g.
```
[ComponentView.xaml]
<ContentView
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:extensions="clr-namespace:[proj name].Extensions"
    x:Class="[proj name].Views.ComponentView">
...

<extensions:ExtendedComponent x:Name="Feature" ... />


[ComponentView.xaml.cs]
...
public void SetPageFeatureView(bool aEnabled, bool bEnabled)
{
    SetAAAControl(aEnabled);
    SetBBBControl(bEnabled);
}

private void SetAAAControl(bool enabled)
{
    Feature.IsAAEnabled = enabled;
}

private void SetBBBControl(bool enabled)
{
    Feature.IsBBEnabled = enabled;
}
```

ExtendedComponent will bridge a native exnteders or renderers or etc.
The properties are defined as a pair of bindable getter/setter, and any change will be updated to the mapped View.
e.g.
```
namespace [proj name].Extensions
{
    public class ExtendedComponent : NativeComponent
    {
        public static readonly BindableProperty IsAAAPropertyEnabled =
            BindableProperty.Create(nameof(IsAAEnabled),
                typeof(bool),
                typeof(ExtendedMap),
                false);

        public static readonly BindableProperty IsBBBPropertyEnabled =
            BindableProperty.Create(nameof(IsBBEnabled),
                typeof(bool),
                typeof(ExtendedMap),
                false);

        public bool IsAAEnabled
        {
            get => (bool) GetValue(IsAAAPropertyEnabled);
            set => SetValue(IsAAAPropertyEnabled, value);
        }
        
        public bool IsBBEnabled
        {
            get => (bool) GetValue(IsBBBPropertyEnabled);
            set => SetValue(IsBBBPropertyEnabled, value);
        }
    }
```

ExtendedComponent can handle each native extenderer. (e.g. android)
```
...
[assembly: ExportRenderer(typeof(ExtendedComponent), typeof(ExtendedComponentRenderer))]
namespace [proj name].Droid.Renderers
{
    public class ExtendedComponentRenderer : NativeComponentRenderer
    {
        private ExtendedComponent _component;

        public ExtendedComponentRenderer(Context context) : base(context)
        {

        }

        private NativeComponent _nativeCompontent;
        
        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                _component = (ExtendedComponent)e.OldElement;
                ...
            }

            if (e.NewElement != null)
            {
                _map = (ExtendedComponent)e.NewElement;
                ...
            }
            
            // handle native features
            _nativeComponent.Feature1 = _component.IsAAEnabled;
            _nativeComponent.Feature2 = _component.IsBBEnabled;
            ...
        }
```
