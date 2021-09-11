# Signing and Deployment to Google Play Store
> [Bundle Build] : more benefits as a single release to cover every devices apks.

## Building a self signed Bundle
1) Open the project on Android Studio with the 'android' folder of the react-native app directory
2) in Android Studio, Go to Build > Generate signed bundle / APK
3) Select Bundle and click Next
4) Key store path : /Users/albert/_proj/navien/navienapp/android/app/navienapp.keystore
5) Key store Password : hxPxxxxxxx
6) Key alias : key0
7) Key Password : hxPxxxxxxx
8) Encryped key export path : /Users/albert/_proj/navien/navienapp/keystores
9) Next -> Release -> Next
10) on Top Menu, Build -> Build Bundle(s)/APK(s) -> Build Bundle(s)
11) you can see the bundle outcome in the location: project folder -> android/app/build/outputs/bundle/release/app-release.aab 

> The keystore file has to be in the android/app folder.
> check a 'signingConfig' in the android/app/build.gradle file.
  ```gradle
  buildTypes {
    release {
        // other settings
        signingConfig signingConfigs.release
    }
  }

  signingConfigs {
    release {
        //if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
            storeFile file("xxx.keystore")
            storePassword "****"
            keyAlias "key0"
            keyPassword "****"
       // }
    }
  }
  ```
> if you're using a variable to set key related informations, please put them on android/gradle.properties

## Building a self signed APK
1) Open the project on Android Studio with the 'android' folder of the react-native app directory
2) in Android Studio, Go to Build > Generate signed bundle / APK
3) Select APK and click Next
4) Under Key store path click Create new
5) Choose a path like *[any personal path in your local build system]*/keystores/android.jks
6) Choose passwords for the keystore and key
7) Enter the certificate information (note: this won’t be displayed in the app, just the certificate)
8) if you get an error: `“Your app is using permissions that require a privacy policy”`, add the tag and permission code to the manifest element of the file: android/app/src/main/AndroidManifest.xml
``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
+  xmlns:tools="http://schemas.android.com/tools"
  package="com.navienapp">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>
+   <uses-permission tools:node="remove" android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
:
```
> in order to avoid an error for CAMERA and RECORD_AUDIO permission, the privacy policy url on the 'store listing' should be set
9) Click OK and click Next. Select both the V1 and the V2 signature version and click Finish
10) A build will create an 'release' directory and both of apk files: `app-x86-release.apk` and `app-armeabi-v7a-release.apk` inside of the folder 'android/app/release'

## Errors
### Out of memory
Out of memory: Java heap space. Configure Gradle memory settings using '-Xmx' JVM option (e.g. '-Xmx2048m'.) Please fix the project's Gradle settings.
> add 'org.gradle.jvmargs=-Xmx2000m' into android/gradle.properties file

### Keystore file not set 
Keystore file not set for signing config release
> please see the above to set buildType and signingConfig for keystore


## Release on Play Store
### 1. Setup 'Play Console'
in order to test the app with a group or a Google Play user
1) Create Google account (e.g. 'username@gmail.com')
2) Create Google Play Console with payment ($25)
3) Sign-in to 'Play Console'
4) select 'setting' -> 'Manage email lists' -> 'Create new list'
5) Type a name to identify your list of testers.
     > *Users need a Google Account (@gmail.com) or a Google Apps account to join a test.*
6) select 'create new list'

### 2. Release the app
in order to test the app with a group or a Google Play user
Create New App with the Title in https://play.google.com/apps/publish 
1) 'Store listing' : set-up with the below mandatory fields
    - Short/Full Description
    - Graphic Assets (Icon: 512 x 512)
    - Screenshots (two representative images: 320~3840px per any side)
    - Application Type
    - Category
    - Contact Detail (Email)
    - Privacy Policy
   Save Draft and then check any missing field to turn the check box on 'Store listing' menu to green 
2) 'App Release' : Select 'Manage' on 'Internal Test'-> 'Create Release' -> Continue -> Accept on the T&Cs -> Release Name -> upload all apk files from the 'android/app/release' -> save
3) 'Content Rating' : Continue -> Select 'Utility' -> tick all 'No' for the questionnaires to ask to include any harmful contents -> save -> calculating rating -> apply rating
4) 'Pricing & Distribution' : Select 'Free' option -> No Ads -> Select simply the all country availability option -> tick the 'marketing opt-out', 'content guideline', and 'US exports law' -> save
5) 'App content' : select '18+' target age -> check 'Your app doesn't appeal to children' -> Email address -> Submit to be reviewed by Google

> All completed shows 'Ready to publish' message on the App main dashboard

6) Select 'App' -> Go to 'App Release' menu again -> Review -> Roll out
7) Select the tester registered, and add more testers if necessarily.


## Lost Keystore
### Create a new Keystore
- android studio > Build > Generate Signed Bundle/APK
- Generate Signed Bundle or APK dialog > select Android App Bundle or APK > click Next.
- Key store path, click Create new.
- New Key Store window > provide the following information for your keystore and key, as shown :
![image](https://user-images.githubusercontent.com/59367560/132947586-8c0b4489-9af2-4505-b418-70a50892244d.png)

> It must be different from any previous keys. Alternatively, you can use the following command line to generate a new key:
> ``` keytool -genkeypair -alias [newalias] -keyalg RSA -keysize 2048 -validity 9125 -keystore [nameofkeystore].jks ```

