# Xamarin Forms : IOS - Provisioning
> source: https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/
![image](https://user-images.githubusercontent.com/59367560/118698402-41e97480-b808-11eb-9dec-4702dcb6cf92.png)

## Simulator

### Enroll
- Join to a Developer portal in Apple : https://developer.apple.com/
> Create an AppleID or login with your AppleID

- Enroll in the Apple Developer Program

- Review and download Certificates and Profiles
> account > certificates, identifieres, and profiles 

### Develop
- Generate Development Certificate
> Xcode > preference > accounts > + > AppleID > Login
your Apple ID will be added the xcode account

- Manage Certificates and Profiles
> Manage Certificates > + > save it to keychain (Login) (the certificates will be registered in the Developer Portal > Account > Certificates
> Download Manual Profiles ( can be registered in Dev Portal, and it's installed in the system)

### Setup on project
Project folder > right button > properties > Bundle Signing
> the registered certificate can be linked with downloaded dev profiles. 
> For the distribution, distribution certificate and profiels can be registered in the system in pair

![image](https://user-images.githubusercontent.com/59367560/118696495-42810b80-b806-11eb-8945-d09b97b36ec2.png)


## Pysical Devices

### Manual provisioning with a device on visual studio
https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/manual-provisioning?tabs=macos

### Trouble-shooting
- have an entitlement not supported by your current provisioing profile
![image](https://user-images.githubusercontent.com/59367560/118716883-41a7a400-b81d-11eb-9607-74b342fe4b5c.png)

  > as entitlement has no permission, remove the custom entitlements field: Project options > Build > IOS Bundle Signing > Custom Entitlements > make it blank.
  ![image](https://user-images.githubusercontent.com/59367560/118717229-b4188400-b81d-11eb-9a3e-091286a8d2a5.png)


