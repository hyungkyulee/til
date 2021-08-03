# Build and Sign android app with Github Actions

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

## Encoding of Keystore
