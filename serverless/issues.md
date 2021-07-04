# Common Issues with serverless practices

### issue : service name violation
- message
``` Serverless: Configuration warning at 'service.name': should match pattern "^[a-zA-Z][0-9a-zA-Z-]+$" ```
- reason
> I was using a '.' on the service nameing.
- solution
> change the service name from ``` service: inventory.apis ``` to ``` service: inventory-apis ```
