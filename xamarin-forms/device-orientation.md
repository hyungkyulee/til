# Orientation Handling on Rotation

## Android

on event of rotation, the MainActivity will be triggered again and the ConfigurationChanged can be handled for a further work.

[MainActivity.cs]
```
...
public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
{
    base.OnConfigurationChanged (newConfig);

    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) 
    {
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) 
    {
        Xamarin.FormsMaps.Init(this, savedBundle);
    }
}
```

## Google Map initialisation in Xamarin Forms
### Do not change the location on rotation event.
> ref: https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/map/map
```
[.xaml]
...
<maps:Map MoveToLastRegionOnLayoutChange="false" />
...

[.xaml.cs]
...
Map.MoveToLastRegionOnLayoutChange = false;
...
```
