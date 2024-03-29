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

### updated at 2022-10
- xcode installation at apple store
- launch xcode and install the dev components (e.g. ios, macos, watch, etc)
- select dev tool path
<img width="824" alt="image" src="https://user-images.githubusercontent.com/59367560/195796321-8773f333-39dc-4399-be6e-fbc48c75913f.png">


[by Brew]
```
brew install cocoapods

// move to the xcode project folder with 'Podfile'
pod install
```

[manual]
```
// 1) install cocoapods 
sudo gem install cocoapods

// 2) to setup the CocoaPods master repository
pod setup

// 3) creat a xcode project
// 4) create podfile 
pod init

// 5) add a project dependency on 'podfile' or initial setup
//    uncomment 'platform : ...' and 'user_frameworks!'(on Swift)

// 6) install the dependencies added
pod install
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
3) JDK installation
   ```
   brew tap homebrew/cask-versions
   brew install --cask zulu11
   ```
   > ./android/gradlew --version
   > Android Gradle v.7.x requires above jdk 11 : https://reactnative.dev/docs/next/environment-setup 
   > ```
   > # export PATH="/usr/local/opt/openjdk@8/bin:$PATH" // disable jdk1.8, replace it to zulu11 
   > 
   > export JAVA_HOME="/Library/Java/JavaVirtualMachines/zulu-11.jdk/Contents/Home"
   > export PATH="$JAVA_HOME/bin:$PATH"

   > export ANDROID_HOME=/Users/$USER/Library/Android/sdk
   > export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

   > export JAVA_8_HOME=$(/usr/libexec/java_home -v1.8)
   > export JAVA_11_HOME=$(/usr/libexec/java_home -v11)

   > alias java8='export JAVA_HOME=$JAVA_8_HOME'
   > alias java11='export JAVA_HOME=$JAVA_11_HOME'

   > # default to Java 11
   > java11
   > ```
   
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

### IOS iphone device connection
- ios/[project name].xcodeproj on xcode
- product > destination > select your iphone connected by cable
- automatically manage signing
- Signing Certificate : assign your personal apple id to the profile
- Go to your Iphone Settings -> General -> Device Management and "trust" yourself as developer.
- trust

[rn project]
- npm install -g ios-deploy
- react-native run-ios --device "[device name]"
> if "device name" is not described, it will pick the first device from the list

[for test-flight/production release]
- enroll at apple developer program : https://developer.apple.com/programs/enroll/
- do two-factor auth with your apple 
- trust this browser
- [phone] download and enroll with the 'apple developer' app on iphone app store
- click 'continue enrollment on the web' link
- fill up the detail and confirm your personal info
- select your entity type : indivisual/sole proprietor
- review and accept
- complete your purchase

## React-Native Project Dev and Build

- project cloning and build/run
```
git clone [proj]
npm i

[ios]
react-native run-ios
...

[android]
react-native run-android
```

[output]
- metro local server
![image](https://user-images.githubusercontent.com/59367560/126879977-78c2b046-8da8-40d5-a6ce-713ac897e959.png)

- emulator to run

## Debugging Tools
### Flipper
https://fbflipper.com/docs/getting-started/
- download desktop app
- install flipper by brew
- Open Flipper. gear icon > Settings > enable the "Enable physical iOS devices" option.
- On "IDB binary location", alert sign (⚠️) means you do not have an idb client
- install the idb companion and idb client.
  > https://fbidb.io/docs/installation/
- Flipper > Gear Icon > Settings > update path with "/opt/homebrew/bin/idb"

## Native App Build/Dev
### Android
#### open android project by Android Studio
Android Studio > Open 'exist project' > select 'android' folder in a React-Native project > open

#### Clean / Rebuild Android project by manually

- clean native build folders (as seen above)
```
[android]
$ ./android/gradlew clean -p ./android/

[ios]
$ rm -rf ios/build
```

- reset including cache
If you are sure the module exists, try these steps:
 1. Clear watchman watches: watchman watch-del-all
 2. Delete node_modules and run yarn install
 3. Reset Metro's cache: yarn start --reset-cache
 4. Remove the cache: rm -rf /tmp/metro-*
   

## Common Errors or Issues
### JDK error
```
Value '/Library/Java/JavaVirtualMachines/jdk1.8.0_212.jdk/Contents/Home' given for org.gradle.java.home Gradle property is invalid (Java home supplied is invalid)
```
> android project > open the file : 'gradle.properties'
  delete the line saying of any mis-matching java version : e.g. org.gradle.java.home=/Library/Java/JavaVirtualMachines/jdk1.8.0_212.jdk/Contents/Home

### Gradle Sync Fail
```
Error running android: Gradle project sync failed.
```
Case 1) Check internet connection

Case 2) Check if you're installed the relevant Android SDK for your project.
> Settings > Appearance & Behavior > System Settings > Android SDK > add the proper API Level for your project

Case 3) Reset Gradle
> File > Invalidate caches / Restart > Shutdown Android Studio > Rename/remove .gradle folder in the user home directory
> Restart Android Studio (It will download gradle metadata and data)

Case 4) Change the Gradle Path to your project local
> Preference > Build, Execution, Deployment > Build Tools > Gradle > Gradle user home > change the default path to your project '.gradle'
