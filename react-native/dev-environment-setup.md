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
