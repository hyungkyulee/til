# Device Settings for Mobild Developement

## Andorid
### Developer Mode
- Enable Dev Mode
  > Settings > About phone > Build number > tab 7 times > it will enable developer mode

- Configure Develper options
  > Settings > Sytem > Developer options
  - Stay awake : On
  - USB Debugging : On > Allow USB debugging form computer
  - Bug report shortcut : On
  - Enable verbose vendor logging : On
  - Enable GPU debug layers : On

> you can see the device from your PC as an adb devices.
> ``` adb devices ```

## IOS
### Developer Connection
- Connect device with itunes/apple under the same AppleID
- allow the Trust of device on PC and Mobile

### List your device on signing/provisioing profiles
- UDID : can be checked on your connected device on Mac
  > connect a device > finder > select the device > click the device description on the top tab > toggle to show the details
   
- Common Error
> error MT1006: Could not install the application on the device: Your code signing/provisioning profiles are not correctly configured. Probably you have an entitlement not supported by your current provisioning profile, or your device is not part of the current provisioning profile.2
