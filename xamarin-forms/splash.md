# Splash Screen in MvvmCross App

## Overview
MvvvmCross should not work properly without starting from MvxSplashScreenActivity.
MainActivity needs to be called from SplashActivity by intent filter.

## How to set a style of the splash loading screen

### MainActivity

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

### SplashActivity

```
namespace TestApp.Droid
{
  [Activity(
    MainLauncher = true,
    Icon = "@mipmap/quartix_icon",
    RoundIcon = "@mipmap/quartix_icon_round",
    Theme = "@style/Theme.Splash",
    NoHistory = true
    )]
    public class SplashScreen : MvxSplashScreenActivity
    {
        public override Task InitializationComplete()
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.SetData(Intent?.Data);
            StartActivity(intent);
            return base.InitializationComplete();
        }
```

### Resource in Android
add a custom style of splash theme at style.xml
```
...
<style name="Theme.Splash"
         parent="Theme.AppCompat.NoActionBar">
    <item name="windowNoTitle">true</item>
    <item name="colorPrimary">#ffff00</item>
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowAnimationStyle">@null</item>
```

### Splash Screen Design as a resource
create splash_screen.xml file on android project > resources > drawable folder
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android"
            android:opacity="opaque">
  <item android:drawable="@color/loading_bg" />

  <item>
    <bitmap android:src="@drawable/test_logo"
            android:tileMode="disabled"
            android:gravity="center" />
  </item>
</layer-list>
```
> The android:opacity=”opaque” this is critical in preventing a flash of black in theme transitions.

### Reference Color Style
any reference style can be defined at android project > resources > values > color.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="launcher_background">#FFFFFF</color>
    <color name="colorPrimary">#ffff00</color>
    ...
</resources>
```

###
