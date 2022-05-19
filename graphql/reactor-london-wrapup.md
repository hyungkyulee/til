# Meet up at 2022-05-19
## Overview
- 2012 on production by facebook
- Microsoft has been joined graphql community and fully deploying their product on graphql since 2021


REST API
- cannot palleralise the fetching data.
- bad performance on network performance

GraphQL
- just empty json data on request with only key
- gragment : componentisation (e.g. function)
- field = dotnet's method
- no controller system



## What is GraphQL
- Query lang for your API
- Runtime to fulfill your queries
- one endpoint, one request
- No over- or under-fetching
- strong Type system (= not allow to violate the contract)


## Operation
Operation - GraphQL - REST
Read - Query - GET
write - Mutation - PUT, POST, PATCH, DELETE
Events - Subscription - N/A

## examples
https://github.com/michaelstaib/microsoft-reactor-examples/tree/main/exercises/Exercise1 
- graphql 의 fields 는 일종의 dotnet 의 method 로 이해할 수 있다. 
- annotation-based approach
```
public class Query
{
    // Resolver
    public string Hello(string name = "World")
        => $"Hello, {name}!";
}

type Query {
    // Field / Argument / Default Value / Non-Null
    Hello(string name = "World") => $"Hello, {name}!";
}
```
> child가 nullable 이어도 부모가 null 이면 contract을 위반 했다고 본다. 

## GraphQl Transport
...
WebSocket

## 




