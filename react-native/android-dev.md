# Android App Development

## Dev Environment Preparation

### Android Studio Installation

- download Android Studio for Mac : https://developer.android.com/studio
- install and Down/Install a set of components
  > Welcome Wizard > Standard > Darcula theme > Verify/Finish
- Configure Android SDK on system Path 
  > Edit the Android SDK tools path on shell script
  ``` code ~/.zshrc ```
  ```
  ...
  export ANDROID_HOME=/Users/$USER/Library/Android/sdk
  export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
  ```
  > check the SDK location on Android Studio > Configure > SDK Manager > Android SDK > SDK Platforms Tab > 'Android SDK Location'
  > Exit/restart the shell for the path to be applied to the system

#### Other ways to setup a Android SDK Development
[To use Homebrew]
1) Install homebrew
   ```
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
2) Install adb and Starting adb
   ```
   brew cask install android-platform-tools
   adb devices
   ```

[To install minimum SDK]
1) Do the basic steps
   ```
   rm -rf ~/.android-sdk-macosx/
   cd ~/Downloads/
   unzip tools_r*-macosx.zip
   mkdir ~/.android-sdk-macosx
   mv tools/ ~/.android-sdk-macosx/tools
   ```

2) Run the SDK Manager
   ```
   sh ~/.android-sdk-macosx/tools/android
   ```

### Android Emulator setup
- Android Studio > Configure > AVD Manager > Add or Edit emulator
- emulator device status
```
adb devices
```


## Project Dev and Build
### React-Native Project Initialisation
### npm package build environment
- Install homebrew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
...
brew update
brew doctor
```

- Install Node
```
brew install node
node -v
brew install watchman
```

- Install Java JRE and JDK
```
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```

- project cloning and build/run
```
git clone [proj]
npm i
react-native run-android
```
