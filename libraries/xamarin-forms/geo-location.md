# Geo-Location with Xamarin.Essecials

## Permission
### Android
#### Permission Configuration on Project setting
- add the below at Android Proj > Properties > AssemblyInfo.cs
  ```
  [assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
  [assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
  [assembly: UsesFeature("android.hardware.location", Required = false)]
  [assembly: UsesFeature("android.hardware.location.gps", Required = false)]
  [assembly: UsesFeature("android.hardware.location.network", Required = false)]
  ```

(OR)
- add the below at Android Proj > Properties > AndroidManifest.xml
  ```
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  <uses-feature android:name="android.hardware.location" android:required="false" />
  <uses-feature android:name="android.hardware.location.gps" android:required="false" />
  <uses-feature android:name="android.hardware.location.network" android:required="false" />
  ```
  
(OR)
- right-click on the Android project > open the project's properties. 
- Under Android Manifest find the Required permissions: 
  area and check the ACCESS_COARSE_LOCATION and ACCESS_FINE_LOCATION permissions. 
  This will automatically update the AndroidManifest.xml file.
  
#### API initialisation on Runtime permission
Android Proj > MainActivity
```
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}

...
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
> InRequestPremission will handle runtime permissions by this Xamarin.Essential handler

### iOS
- add key/string dictionary at iOS proj > info.plist
  ```
  <key>NSLocationWhenInUseUsageDescription</key>
  <string>Fill in a reason why your app needs access to location.</string>
  ```
  
## How to use
```
CancellationTokenSource cts;

async Task GetCurrentLocation()
{
    try
    {
        var request = new GeolocationRequest(GeolocationAccuracy.Medium, TimeSpan.FromSeconds(10));
        cts = new CancellationTokenSource();
        var location = await Geolocation.GetLocationAsync(request, cts.Token);

        if (location != null)
        {
            Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
        }
    }
    catch (FeatureNotSupportedException fnsEx)
    {
        // Handle not supported on device exception
    }
    catch (FeatureNotEnabledException fneEx)
    {
        // Handle not enabled on device exception
    }
    catch (PermissionException pEx)
    {
        // Handle permission exception
    }
    catch (Exception ex)
    {
        // Unable to get location
    }
}

protected override void OnDisappearing()
{
    if (cts != null && !cts.IsCancellationRequested)
        cts.Cancel();
    base.OnDisappearing();
}
```
