# Meet up at 2022-05-19
## Overview
- 2012 on production by facebook
- Microsoft has been joined graphql community and fully deploying their product on graphql since 2021
- banana cake pop
- hotchocolate + .net graphql

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

## Middleware
```
// [ ... ]  = middleware
public class Query
{
    [UsePaging]         // <- 1st middleware of pipeline
    [UseProjection]     // <- 2nd middleware of pipeline
    [UseFiltering]      // ...
    [UseSorting]
    public IQueryable<Asset> GetAssets(AssetContext context)
        => context.Assets;
}
```


middleware is a sort of pipeline


## Layer Service
GraphQL - Business Layer - Data Layer
e.g.
            GraphQL
   |           |                   |
Internal     REST                REST
   |           |                   |
Asset Srv    Price Change Srv    Price History Srv
   |           |                   |
Data         Data                Data



## Data Loader
GraphQL Execution Engine -> (Executes) -> Resolvers -> (BRC, Month) 
-> DataLoader -> (Resolve from Cache) -> Task Cache
              -> (create new)         -> 
              -> (Batch Execute) -> Price Change Service
              
- Improves Data Fetching (caching)
- Ensures Consistency (same object)


##

- microsoft driven reactive project 
  https://github.com/dotnet/reactive
  
  
## Defer
@defer and @stream allow the ...

아주 비싼 데이터 fetch 항목에 대해서 ```... @defer``` 사용하라
-> 별도의 분리된 fragment 를 만든다.
   해당 항목을 제외한 나머지를 먼저 처리하여 response
   이후 defer 설정된 항목을 history로 묶어서 response
   
   
slack.chillicream.com
github.com/chillicream/hotchocolate

bit.ly/michaelTwitter
bit.ly/michaelLinkedin

