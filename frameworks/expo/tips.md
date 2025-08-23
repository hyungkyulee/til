### Expo App Audit
- npx expo-doctor
- npx expo install --check // will check any coflict between packages


### Expo Build Environment
To reuse the same keystore stored in Expo for deploying your app to production on Android, follow these steps:

#### Log in to Expo
Ensure you are logged into your Expo account in your terminal:
```
eas login
```
> To change the EAS CLI login user, you can log out of the current session and log in with a different user. You can log out of the EAS CLI by running the command "eas logout". After logging out, you can then log in with the desired user account by running eas login and following the prompts. 

#### Fetch the Existing Keystore
Run the following command to fetch the existing keystore associated with your app:
```
eas credentials
```
This will download the keystore file and display the credentials (keystore password, alias, and alias password). Save these details securely.

#### Configure eas.json: 
Update your eas.json file to ensure it uses the existing keystore. Add the following configuration under the android build profile:
```
{
  "build": {
    "production": {
      "android": {
        "credentialsSource": "remote"
      }
    }
  }
}
```
This ensures Expo uses the keystore stored on their servers.

#### Ensure Correct android.package
Verify that the android.package field in your app.json matches the package name of your previous app (com.[appname]).

#### Build the App
Use the following command to build the app for production:
```
eas build -p android
```
> eas build --platform android --profile production

#### Submit
Submit to Play Store: Once the build is complete, download the APK/AAB and submit it to the Google Play Console.
