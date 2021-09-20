# Https Dev Certificate on Mac for .NET project

## Security Issue on localhost
- reason : 
When https certificates (Dev) is installed to your keychain, the certificates are correlated to your password at the time they are installed. 
In case of password changes, error shows 'the https certificate is invalid' due to a mis-matching of passowrd.

- solution : 
  - command + space > keychain access > Certificates(tab) > select 'localhost' item > delete 'localhost' by mouse right-button
  - create a new certificate : ``` dotnet dev-certs https --trust ```

> ref: https://rimdev.io/solving-dotnet-core-https-development-certificate-issues-on-macos/
