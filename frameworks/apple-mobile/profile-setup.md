



- Apple Developer portal shows the certificate: this is the public certificate record
- Xcode debug signing needs a local code-signing identity: certificate + matching private key in your Keychain

> If private key is missing locally, auto signing fails even though team/account/cert look correct.
> 1. check in keychain access
>   - Open Keychain Access
>   - Select login keychain, category Certificates
>   - Find Apple Development: <user name> (<user account id>)
>   - Expand it:
>     - If you see a nested Private Key item underneath, you’re good
>     - If no nested private key, that certificate cannot sign
>  2. Check via terminal (identity must appear)
>     - Run: ```security find-identity -v -p codesigning```
>     - You should see an entry like: Apple Development: <user name> (<user account id>)
>     - If certificate exists in Keychain but does not appear here, private key is missing or unusable
>
> 2. Private key cannot be recovered (most common)
>    - Revoke old Apple Development certificate in Apple Developer portal
>    - Create a new Apple Development certificate from this Mac
>      - Easiest: Xcode -> Settings -> Accounts -> your Team -> Manage Certificates -> + -> Apple Development
>    - Let Xcode install it into login keychain
>    - Refresh signing assets:
>      - Xcode -> Settings -> Accounts -> Download Manual Profiles (or let auto signing recreate)
>    - Clean and rebuild:
>      - Product -> Clean Build Folder
>      - delete DerivedData if needed
