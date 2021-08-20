# Splash Screen in MvvmCross App

## Overview
MvvvmCross should not work properly without starting from MvxSplashScreenActivity.
MainActivity needs to be called from SplashActivity by intent filter.

## How to set a style of the splash loading screen

### Android
#### MainActivity

Define Activity for Main with App Label, Icon, and Theme
> Theme should be 'MainTheme' because app needs to be working under the main theme returning back from the customised splash theme.
> This is because we don't want the loading screen is a default background of the app

```
namespace TestApp.Droid
{
    [Activity(Label = "TestApp",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize
    )]
    public class MainActivity : MvxFormsAppCompatActivity<Setup, App, ... >
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            Forms.Init(this, savedInstanceState);
            ...
            
            LoadApplication(new App()); (or App.Current.xxxx with the above App)
            ...
        }
    }
}
```

#### SplashActivity

```
namespace TestApp.Droid
{
    [Activity(
        MainLauncher = true,
        Icon = "@mipmap/testapp_icon",
        RoundIcon = "@mipmap/testapp_icon_round",
        Theme = "@style/Theme.Splash",
        NoHistory = true
        )]
    public class SplashScreen : MvxSplashScreenActivity
    {
        private Intent _intent;
        
        public override Task InitializationComplete()
        {
            _intent = new Intent(this, typeof(MainActivity));
            _intent.SetData(Intent?.Data);
            
            return base.InitializationComplete();
        }

        protected override void OnCreate(Bundle bundle)
        {
            SetTheme(Resource.Style.Theme_Splash);
            base.OnCreate(bundle);
        }

        protected override Task RunAppStartAsync(Bundle bundle)
        {
            if (Mvx.IoCProvider.TryResolve(out IMvxAppStart startup))
            {
                StartActivity(_intent);
            }
            return Task.CompletedTask;
        }

        protected override void OnStop()
        {
            // Save on Heap usage
            Window?.SetBackgroundDrawable(null);
            base.OnStop();
        }
    ...       
```
> OnResume can be overrided
> if you want to create your own splash screen style layout, it can be inherited from ctor
  ```public SplashScreen() : base(Resource.Layout.SplashScreen) { }```
  
#### Resource in Android
add a custom style of splash theme at style.xml 
> project > android > resorces > values > styles.xml

```
...
<style name="Theme.Splash"
         parent="Theme.AppCompat.NoActionBar">
    <item name="windowNoTitle">true</item>
    <item name="colorPrimary">#ffff00</item>
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowAnimationStyle">@null</item>
</style>
...
```
> if name has '.', you can refer this name from resource.style with '_' (e.g. Resource.Style.Theme_Splash)
> this style will call the xml design file as a resource 

#### Splash Screen Design as a resource
create splash_screen.xml file on android project > resources > drawable folder
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" 
            android:opacity="opaque" >
  <item android:drawable="@color/launcher_background" />
  <item
    android:width="430dp"
    android:gravity="center">
    <bitmap
      android:gravity="fill_horizontal|fill_vertical"
      android:src="@mipmap/testapp_logo"
      android:mipMap="true"/>
  </item>
</layer-list>
```
> The android:opacity=”opaque” this is critical in preventing a flash of black in theme transitions.

#### Reference Color Style
any reference style can be defined at android project > resources > values > color.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="launcher_background">#FFFFFF</color>
    <color name="colorPrimary">#ffff00</color>
    ...
</resources>
```

[e.g. background image example]
> without 
```
<?xml version="1.0" encoding="UTF-8" ?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >
<item>
  <color android:color="@color/launcher_background" />
</item>
<item
  android:drawable="@drawable/splash_bg_v6"
  android:gravity="center"/>
</layer-list>
```

```
...
<style name="Theme.Splash"
     parent="Theme.AppCompat.NoActionBar">
    <item name="windowNoTitle">true</item>
    <item name="colorPrimary">#13AB5B</item>
    <item name="android:windowBackground">@drawable/splash_bg_v6</item>
    <item name="android:windowFullscreen">true</item>
    <item name="android:windowDisablePreview">false</item>
    <item name="android:windowAnimationStyle">@null</item>
    <item name="android:adjustViewBounds">true</item>
    <item name="android:scaleType">centerCrop</item>
</style>
<style name="datepicker" parent="Theme.AppCompat.Light.Dialog">
    <item name="colorAccent">#13AB5B</item>
</style>
...
```

### IOS
> ref: https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/images-icons/launch-screens?tabs=macos
> ref: 

#### Assets and storyboard
- If the original project has not been including Asset Catalog and Launch Screen Storyboard, 
Add this on project of solution.
VS for mac > Project(IOS) > Right Button > Add > Asset Catalog, and Add > Launch Screen
> Name the file LaunchScreen or another name of your choosing.

- Add images on Assets.xcassets
Add > Image set > copy images to universal 

![image](https://user-images.githubusercontent.com/59367560/128366023-4cb1ca48-f5ef-4667-ad2f-2c1ff6e0bc85.png)

#### Storyboard configuration
Configure the Project to use the appropriate Storyboard for its Launch Screen
info.plist > Application tab (bottom of tab) > Launch Images > set source and launch screen
> source will diplay the image set name of the assets, and Lunch Screen is listing storyboard name
> 
![image](https://user-images.githubusercontent.com/59367560/128366113-a6130d3e-ecb1-4f63-beba-c830c4d43536.png)

#### Storyboard Editing
double-click will lead you to xcode ios designer tool so that you can design the splash(launch) screen.

- Select the storyboard on the left pane
- Eidt the screen: Vew controller Scene > Vew controller > Vew
![image](https://user-images.githubusercontent.com/59367560/128367052-5be3d432-8637-4c87-9fe5-a78d601171c0.png)

- select one of the reference device for you on the bottom selector
- edit the screen with bg color and image and text, etc
- add constraint to set the design to be applied to the other size devices as well
  > Add new constraints > Add on constraints widget > Save on xcode
  
![image](https://user-images.githubusercontent.com/59367560/128367141-23f49d4d-6a37-4857-8c4e-6728e2f850ba.png)


## Disable rotation on splash screen
### android
- set portrait only on the layer-list of the splash screen xml design file
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content">
  <item>
    <color android:color="@color/launcher_background" />
  </item>
  <item>
    <bitmap
      android:gravity="fill"
      android:scaleType="fitCenter"
      android:src="@mipmap/splash_location_logo_bg"
      android:mipMap="true"/>
  </item>
</layer-list>
```
### ios
- set portrait only on info.plist
```
...
<key>UISupportedInterfaceOrientations</key>
	<array>
		<string>UIInterfaceOrientationPortrait</string>
		<string>UIInterfaceOrientationPortraitUpsideDown</string>
	</array>
...
```

- then mask again a rotation rule for app on appDelegate.cs

[other way]
https://heartbeat.fritz.ai/force-an-orientation-on-a-single-page-in-xamarin-forms-b9c0c5295367


## Appendix
### Launch Images and Launch Screen configuration
[VS] : iOS project > Info.plist > doulbe click to open
![image](https://user-images.githubusercontent.com/59367560/130222653-7fe8184b-8897-4506-8201-93e1beebb7b0.png)
> you can select the LaunchScreen from the LaunchScreen.Storyboard
> You can select the LaunchScreen Image set from the Image Asset

on Application tab, it should be linked with the above setting
![image](https://user-images.githubusercontent.com/59367560/130222846-33539cc5-d02a-4dd9-8f16-979f7acfe081.png)

on xxx.xcassests (Launch Image Assets folder), it will be displayed on the above info.plist dropdown list
![image](https://user-images.githubusercontent.com/59367560/130223166-2212686f-43fc-494b-a762-6956f95abb79.png)
> you have to arrange the different resolution images on LaunchImages

![image](https://user-images.githubusercontent.com/59367560/130223412-68e7c545-deed-42be-8758-a009368155b1.png)

#### Some issues with a clipping Top/Bottom 
> This is related with the release binary which is not connecting to the relevant LaunchImages.
> I've tried it to LaunchScreen, though. It's not successful and I changed the configuration to LaunchImages rather than LaunchScreen.
> -> Why?
