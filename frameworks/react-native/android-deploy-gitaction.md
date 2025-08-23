# Build and Sign android app with Github Actions
> https://developer.android.com/studio/publish/app-signing#sign_release
> https://proandroiddev.com/how-to-securely-build-and-sign-your-android-app-with-github-actions-ad5323452ce


## Overall steps
1. Generate an upload key and keystore
2. Sign your app with your upload key
3. Configure Play App Signing
4. Upload your app to Google Play
5. Prepare & roll out release of your app

## Pre-requisite 
### Generate an upload key and keystore
- android studio > build > generate signed bundle/apk > android app bundle (enforcely recommended by google since Nov 2021)
- create new > new key store and key information > ok

## Gradle Config
setup signingConfigs on android > app > build.gradle
> use environment variables from github secret (see below)

```
 signingConfigs {
    release {
        def tempFilePath = System.getProperty("user.home") + "/dev/_temp/keystore/"
        def allFilesFromDir = new File(tempFilePath).listFiles()

        if (allFilesFromDir != null) {
            def keystoreFile = allFilesFromDir.first()
            keystoreFile.renameTo("keystore/[projectname]_keystore.jks")
        }

        storeFile = file("keystore/[projectname]_keystore.jks")
        storePassword System.getenv("SIGNING_STORE_PASSWORD")
        keyAlias System.getenv("SIGNING_KEY_ALIAS")
        keyPassword System.getenv("SIGNING_KEY_PASSWORD")
    }
}
```
> this will leave the keystore file in the local path and use to build on gradle file
> add keystore file on .gitignore

## Encoding of Keystore
encode the keystore file by Base64 encoding shceme and the outcome .txt file will be stored in Gibhub secret

in the folder which the keystore file is located
```
openssl base64 < [appname].keystore.jks | tr -d '\n' | tee [appname].keystore.base64.encoded.txt
```

## Github Secret configuration
project git repo > setting > secrets > New repository secret

Name : KEYSTORE
Value : [copy the content of encoded keystore ]
> ``` pbcopy < [appname].keystore.base64.encoded.txt ```

Name : SIGNING_STORE_PASSWORD
Name : SIGNING_KEY_ALIAS
Name : SIGNING_KEY_PASSWORD

