# Mobile App from Expo
## Android
- [play.google.com] create a Google Play Developer account on [the Google Play Console sign-up page](https://play.google.com/apps/publish/signup/) : 
  > A paid developer account is required
- [play.google.com] Open Google Play Console, expand Setup, and choose API access.
- [play.google.com] select Choose a project to link, -> agree -> then either link it to an existing project if you have one, or select Create new project and then click Link project.
  ![image](https://user-images.githubusercontent.com/59367560/189769876-52d3e557-e3a1-41b0-8a81-c2a0f0ba213f.png)

- [console.cloud.google.com] Create a service account to give an access to the service, not user
  > Google Cloud Console > IAM and admin > Service Accounts > : > Create Service Account
  ![image](https://user-images.githubusercontent.com/59367560/189771046-db5af4ed-3c13-4ef3-be6e-a352a47c8adb.png)

  > service account details with an app-name > set a role with 'Service Accounts' > 'Service Account User' > done
  > result > actions > manage keys > Add key > Create a new key > select 'JSON' > download a json file
  ![image](https://user-images.githubusercontent.com/59367560/189771858-7fbb9bb2-678a-4235-a053-f3479302a54e.png)
  
  ![image](https://user-images.githubusercontent.com/59367560/189772101-6fbbf730-cb0d-4878-9408-9eb51e162a33.png)

  ![image](https://user-images.githubusercontent.com/59367560/189772394-da4cac8a-f71b-4022-ad2a-eaabd75684b2.png)

- [play.google.com] done at popup window > manage play console permissions > account permissions
  - check on the belows
  ![image](https://user-images.githubusercontent.com/59367560/189772850-85b005ac-eacf-4a79-9ca8-d74e4e23983c.png)
  
  - invite user > send invitation to the created google service account

- [play.google.com] create an app on Google Play Console
- All done !! => ready to upload app by ```eas submit```

## deployment
1. EAS Submit - https://docs.expo.dev/submit/introduction/
2. Manually uploading your app for the first time 
   > Before using eas submit -p android for uploading your builds, you have to upload your app manually at least once. This is a limitation of the Google Play Store API.
3. install eas
   ```
   yarn global add eas-cli
   ```


## Expo build for Android
Step 1: Run expo build:android and choose apk-bundle option:
```
Run the following:

› npm install -g eas-cli
› eas build -p android https://docs.expo.dev/build/setup/

expo build:android will be discontinued on January 4, 2023 (113 days left).

📝  Android package Learn more: https://expo.fyi/android-package

? What would you like your Android package name to be? › 
```

install eas-cli
```
npm install -g eas-cli
...
main !2 ?2  eas --version                                                                     ✔  29s  01:11:06 
eas-cli/2.1.0 darwin-x64 node-v14.18.0

...
main !2 ?2  eas build -p android

```






