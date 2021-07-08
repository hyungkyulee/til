# A First Azure Function API in Rider

## Setup Infrastructure
### install azure devkit in rider on market place
Rider > Preferences > Plugins > Marketplace > Search 'Azure toolkit for Rider'
![image](https://user-images.githubusercontent.com/59367560/124996978-ca82d500-e041-11eb-8d38-94552706230a.png)

### install azure function tools (aka. cli)
> ref: https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=macos%2Ccsharp%2Cbash

#### Core tools
```
brew tap azure/functions
brew install azure-functions-core-tools@3
# if upgrading on a machine that has 2.x installed
brew link --overwrite azure-functions-core-tools@3
```

### Azurite
> Azure Mocking Emulation Server for Developers to run the Azure Function in local
```
❯ npm install -g azurite
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142

added 235 packages, and audited 236 packages in 10s

3 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New minor version of npm available! 7.11.2 -> 7.19.1
npm notice Changelog: https://github.com/npm/cli/releases/tag/v7.19.1
npm notice Run npm install -g npm@7.19.1 to update!
npm notice

❯ mkdir ~/github/azurite
❯ azurite -s -l ~/github/azurite -d ~/github/azurite\debug.log
Azurite Blob service is starting at http://127.0.0.1:10000
Azurite Blob service is successfully listening at http://127.0.0.1:10000
Azurite Queue service is starting at http://127.0.0.1:10001
Azurite Queue service is successfully listening at http://127.0.0.1:10001
Azurite Table service is starting at http://127.0.0.1:10002
Azurite Table service is successfully listening at http://127.0.0.1:10002
```

### New Project
- create project on Azure Functions
![image](https://user-images.githubusercontent.com/59367560/124997506-a673c380-e042-11eb-9c54-2256d4ad03a8.png)

### First Function Handler File
- Project > Add > HTTP Trigger
![image](https://user-images.githubusercontent.com/59367560/124997671-f8b4e480-e042-11eb-9ebc-0c9e018dc93e.png)

- Endpoint detail
  - methods : get (or post, etc)
  - Route : endpoint path
  ```
  public static async Task<IActionResult> RunAsync(
      [HttpTrigger(AuthorizationLevel.Function, "post", Route = "v1/invoices")]
      HttpRequest req, ILogger log)
  {
    ...
  ```
  
### Execute
- build
- Run 
```
❯ func start
Function Runtime Version: 3.0.15828.0

Can't determine project language from files. Please use one of [--csharp, --javascript, --typescript, --java, --python, --powershell, --custom]
Can't determine project language from files. Please use one of [--csharp, --javascript, --typescript, --java, --python, --powershell, --custom]
[2021-07-08T22:02:38.005Z] Cannot create directory for shared memory usage: /dev/shm/AzureFunctions
[2021-07-08T22:02:38.005Z] System.IO.FileSystem: Access to the path '/dev/shm/AzureFunctions' is denied. Operation not permitted.

Functions:

        CreateInvoiceFunction: [POST] http://localhost:7071/api/v1/invoices

For detailed output, run func with --verbose flag.
[2021-07-08T22:02:45.104Z] Host lock lease acquired by instance ID '000000000000000000000000E85C0300'.

```
