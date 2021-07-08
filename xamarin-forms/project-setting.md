# Project Setting for Development

## Dev Environment 
### Android setup on Mac (3-Ways)

#### Android Studio (recommended)
* Install the Android Studio at https://developer.android.com/studio
* Add the following lines to the end of ~/.bashrc or ~/.zshrc
 ```
 export ANDROID_HOME=/Users/$USER/Library/Android/sdk
 export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
 ```
 > Android SDK Path can be referred at 'Ppen' android studio > 'Configure' > 'SDK Manager' > 'Android SDK Location'

#### Android Debug Bridge (ADB) only installation on Brew 
- Install homebrew
  ```
  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
- Install adb and Starting adb
  ```
  brew cask install android-platform-tools
  adb devices
  ```

#### Install minimum SDK
- Do the basic steps
  ```
  rm -rf ~/.android-sdk-macosx/
  cd ~/Downloads/
  unzip tools_r*-macosx.zip
  mkdir ~/.android-sdk-macosx
  mv tools/ ~/.android-sdk-macosx/tools
  ```
- Run the SDK Manager
  ```
  sh ~/.android-sdk-macosx/tools/android
  ```

### IOS Setup on Mac
- Update XCODE as a latest version (via apple store)
- Install emulator to test
  > xcode > preference > components > simulators
  
- Install Visual Studio
  > e.g. vs for mac 2019 : https://docs.microsoft.com/en-us/visualstudio/mac/installation?view=vsmac-2019
- Set the apple SDK Path
  > Visual Studio for Mac > Preferences > Projects > SDK Locations > Apple > Apple SDK > select a application of xcode
  > e.g. Location: /Applications/Xcode.app
