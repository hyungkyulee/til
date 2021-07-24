# Mobile App Development on RN

## React-Native Development Environment
### Basic setup
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

- Install Java JRE and JDK (ver. 1.8)
```
brew tap AdoptOpenJDK/openjdk
brew install --cask adoptopenjdk8

...
java -version                      ✔  1m 36s  20:34:05
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.292-b10, mixed mode)
```
> Optional if the default java version is not set to 1.8 : ``` echo 'export PATH="/usr/local/opt/openjdk@8/bin:$PATH"' >> ~/.zshrc ```

- install react-native-cli
```
npm install -g react-native-cli
```

### IOS Build/Dev environment
- setup XCODE
  - Xcode > Preferences > Locations tab
  - Selecting the most recent version from the Command Line Tools dropdown
   
- install cocoapods to MAC OS
  > CocoaPods manages library dependencies for your Xcode projects
```
sudo gem install cocoapods
```

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

#### Android Emulator setup
- Android Studio > Configure > AVD Manager > Add or Edit emulator
- emulator device status
```
adb devices
```

## React-Native Project Dev and Build

- project cloning and build/run
```
git clone [proj]
npm i
react-native run-android
```
