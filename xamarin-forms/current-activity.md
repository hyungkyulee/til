# Application Activity on Xamarin Essential
## Overview
Standalone: CrossCurrentActivity.Current.Activity
Xamarin Essentials: Platform.CurrentActivity

> On Xamarin Essentials 1.5 or higher, Platform.CurrentActivity can be replaced with the CurrentActivity plugin.

### Initialisation
OnCreate at MainActivity.cs
```
Xamarin.Essentials.Platform.Init(this, savedInstanceState);
```
### Callers 




## Private Nuget
### Plugin.CurrentActivity

OnCreate at MainActivity.cs
```
CrossCurrentActivity.Current.Init(this, savedInstanceState);
```

Caller methods
```

```
