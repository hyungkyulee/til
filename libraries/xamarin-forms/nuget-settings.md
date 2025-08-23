# Nuget Paclage settings in Xamarin Project

### Any Nuget related unknown error : e.g. Couldn't Add NuGet Package
```
nuget locals all -clear
```

### Your project is not referencing the Mono.Android, Version=v11.x Framework Error
set the AndroidTargetSDK into v11.x 
install the AndroidSDK and Tools related with the TargetSDK version
> rider > tools > Androids > SDK Manager 
> Android Studio > Configure > SDK Manager 
> (* enable 'Show Package Details' to see which packages are missing)

