# Xamarin Forms : IOS - Provisioning

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

