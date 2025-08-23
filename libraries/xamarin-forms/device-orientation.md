# Orientation on Rotation

## Rotation to handle by callback

### Rotation handler in Main App
#### Callback handler in IOS
Rotation Output Value:
- LandscapeLeft
- PortraitUpsideDown
- LandscapeRight
- Portrait

[AppDelegate.cs]
```
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    InitAppearance();

    ...
    
    Foundation.NSNotificationCenter.DefaultCenter.AddObserver(new NSString("UIDeviceOrientationDidChangeNotification"), DeviceRotatedCallback);

    return base.FinishedLaunching(app, options); 
}

...

private void DeviceRotatedCallback(NSNotification obj)
{
    Console.WriteLine($">>>> rotation: {UIDevice.CurrentDevice.Orientation}");
}
```

#### Event handler in Android
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

### Rotation Callback handler in Native Controller
#### IOS
[ios project > controls > customFeature.cs]
```
public class CustomFeature : ICustomFeature
{
  ...
  // [constructor]
  public CustomFeature()
  {
    ...
    UIDevice.CurrentDevice.BeginGeneratingDeviceOrientationNotifications();
    _orientationObserver = UIDevice.Notifications.ObserveOrientationDidChange(DataPickerRotationCallback);
    ...
  }
  ...
  
  // [callback handler]
  private void DataPickerRotationCallback(object sender, NSNotificationEventArgs e)
  {
      Console.WriteLine($">>>> rotation: {UIDevice.CurrentDevice.Orientation}");
  }
  
}
```


## Google Map rotation property in Xamarin Forms
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
