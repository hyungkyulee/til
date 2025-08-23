# Signing Key Configuration

## Debug (Development) Mode
1) Proj Property > No signing key
![image](https://user-images.githubusercontent.com/59367560/120312555-c7712800-c2d0-11eb-9f6d-0fe1cd70909a.png)

2) check the debug.keystore auto-generated at : /Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore
3) run keytool to set a keystore into your project : keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
   > ```zsh
   > ❯ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
Alias name: androiddebugkey
Creation date: 15-May-2021
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 23b94340
Valid from: Sat May 15 12:21:39 BST 2021 until: Mon May 08 12:21:39 BST 2051
Certificate fingerprints:
         MD5:  B***
         SHA1: 6***
         SHA256: D***
Signature algorithm name: SHA256withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions: 

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 43 BC B***
]
]
   > ```

> when you create a credential(e.g. API Key) you can use this keysotre

## API Key location
dd this API key to the AndroidManifest.XML file
```xaml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```
