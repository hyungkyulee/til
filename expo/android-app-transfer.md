
Step 1: Transfer Android App to Another Google Play Developer Account
- Check Eligibility for Transfer
  - Ensure your app meets Google Play’s transfer requirements: https://support.google.com/googleplay/android-developer/answer/6230247?hl=en.
	- No active policy violations.
  - App must be published and not in a draft state.

- Prepare Necessary Information
  - Original Google Play Developer account name & email.
	- Target Google Play Developer account name & email.
	- App package name (e.g., com.example.app).
	- App signing key (Expo manages this, but you should download it).
	- Latest APK/AAB version code.
	- Active subscriptions (if any).
	- Firebase/Google API Services credentials (if used).

- Download and Secure Keystore from Expo
  > Expo -> select target project -> configuration -> credentials -> Build Credentials -> Android keystore -> three dots -> download
  By eas-cli
  ```
  eas credentials

  // Select Android → Keystore → Download.
  // Save the .jks file, keystore alias, and keystore password securely.
  ```
  > expo fetch:android:keystore is deprecated

- App Transfer
  - 
  

