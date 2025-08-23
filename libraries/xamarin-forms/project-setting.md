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


## Trials and Errors

### Metadata file '.dll' could not be found
It was happening on VS build from the second time build. (Not Rider)
The cause is adding the output path with additional folder name (e.g. for me, netstandard2.1)
> it looks help me set the appendoutputpath option with false.
> ```
> <PropertyGroup>
>  <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
> </PropertyGroup>
> ```
> https://developercommunity.visualstudio.com/t/cant-remove-netstandard-folder-from-output-path/30940

### Build error on debug or release mode
build configuration is not set with an build mode.
> solution > options > build > configurations > configuration Mappings > check if any build option is unchecked
> ![image](https://user-images.githubusercontent.com/59367560/130784896-8e99a6bc-072b-4cf3-bd8e-c1c92bf58da8.png)
> https://stackoverflow.com/questions/1421862/metadata-file-dll-could-not-be-found
