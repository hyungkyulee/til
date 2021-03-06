# Xamarin Forms : IOS - Provisioning
> source: https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/
![image](https://user-images.githubusercontent.com/59367560/118698402-41e97480-b808-11eb-9dec-4702dcb6cf92.png)

## Dev Environment
### Visual Studio
- visual studio for mac installation
- sign-in onto the enterprise membership (if applied)

### Developer Profile
- = developer certificate (your identity) + device (associated key) + profile for development (provisioning profile)
> apple will check the certificate to control 'access/deploy to device' from you

#### Provisioning Profile
- Apple dev portal > Certificates, identifieres & profiles > Register an App ID(application ID to deploy)
> An App ID is a reverse-DNS style string that uniquely identifies an application : e.g. com.[DomainName].*

#### Development Certificate
- manage certificates, identifiers, and profiles : https://developer.apple.com/
> add your appleId as a member of organisation (if it's under the enrolling a enterprise programme)


- create certificate via visual studio : 
  [Certificate on VS]
  - Visual Studio > Preferences > Apple Developer Account
  - Add Account > login to organisation
  > you can see your linked appleId and member status

  - View Detail > Create Certificate > select 'Apple Development or iOS Development'
  > two versions of developer profile will be located : 1) Apple Developer Portal (public key), 2) local machine (private key)

  [on Apple Dev Poral]
  - portal > C, I & P > + > Development > iOS App Development > Select App ID to use > Select Certificates to include > Profile Name > Generate
  > (Optional) Download it into Local Machine

  [xcode]
  > another option will be via xcode, but don't mix up the process

- Visual Studio > Prefereneces > Apple Developer Account > view Details > Download All Profiles
> The new provisioning profile will now be available in Visual Studio and ready to use.

#### Add a Device
- connect iphone(ipad) via USB cable
- Xcode > Window > Devices and Simulators > copy UDID (identifier string)
- Apple dev portal(https://developer.apple.com/) > Certificates, identifieres & profiles > Devices > + > Register a Device


## Deploy to a device
- Visual Studio > IOS project > Info.plist 
> check if 'Bundle Identifier' has the same App ID

- Manual downloading/installation of provisioning profile
- Visual Studio > IOS Bundle Signing > Signing Identity = Developer (Automatic)
> you can see the list of a set of available Provisioning Profiles

### Simulator

### Setup on project
Project folder > right button > properties > Bundle Signing
> the registered certificate can be linked with downloaded dev profiles. 
> For the distribution, distribution certificate and profiels can be registered in the system in pair

![image](https://user-images.githubusercontent.com/59367560/118696495-42810b80-b806-11eb-8945-d09b97b36ec2.png)


### Pysical Devices

### Manual provisioning with a device on visual studio
https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/manual-provisioning?tabs=macos

### Device Configuration on Project settings of Rider
> Rider > Hightlight IOS Project on Explorer > F4 (or mouse right-click) > Properties or Edit(Manual setting)
e.g.) 
![image](https://user-images.githubusercontent.com/59367560/125324107-77f23300-e337-11eb-8a0f-57e7fe84307f.png)

### install the beta test app
- release beta app via vs app center
- email will be sent to the registered developers
- if you can see a non-registered device message, click the 'add device' button 
![image](https://user-images.githubusercontent.com/59367560/137393740-7923017d-0055-44b8-a811-2e04009a66d5.png)

- profile will be downloaded
- install the app via settings > downloaded profile (will be located under 'software update') > install
- will be redirected to vs app center page and you can install the beta version


## Trouble-shooting
- have an entitlement not supported by your current provisioing profile
![image](https://user-images.githubusercontent.com/59367560/125322280-82abc880-e335-11eb-94c2-5158b63eebbb.png)

  > as entitlement has no permission, remove the custom entitlements field: Project options > Build > IOS Bundle Signing > Custom Entitlements > make it blank.
  ![image](https://user-images.githubusercontent.com/59367560/118717229-b4188400-b81d-11eb-9a3e-091286a8d2a5.png)

- non-listed device error
![image](https://user-images.githubusercontent.com/59367560/125324631-ffd83d00-e337-11eb-89c3-ac4b7c0276f1.png)

  > device is not listed on the provisioning group for the project.
 
