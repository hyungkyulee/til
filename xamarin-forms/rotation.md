# Rotation to handle in IOS and Android

## Rotation Callback handler in App Main
Rotation Output Value:
- LandscapeLeft
- PortraitUpsideDown
- LandscapeRight
- Portrait

### IOS

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


## Rotation Callback handler in Native Controller
### IOS
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
