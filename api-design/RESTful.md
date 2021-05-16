# RESTful API Design
## Client-Server Communication overview
- Socket Programming : launguage based API, dependency on a network situation
- RPC (Remote Procedure Call) : encapsulation of network handlers, resource localisation by IDL(Interface Definition Language) 
- CORBA/RMI : JAVA interface / same approach as RPC
- SOAP : web-based approach, HTTP+XML, client message creation based on a server spec 
- REST : URI-based spec to represent resources

## RESTful API Concept
> REST (Representational State Transfer) is intended to evoke an image of how a well-designed web application behaves
 - a network of web pages
 - where a user progresses via an app
 - being transferred and rendered with a response
(* by Roy Fielding)

Definition and characteristics of REST
 - an architectural style (not a standard)
 - use a standard to implement this style
 - protocol agnostic

### 6 Constraints of RESTfulness
1) Uniform Interface - a single and technical interface only can be shared (e.g. uri, method, media type, etc)
   - Manipulation of resources via representations - URIs have to construct with resource id to update/delete
   - Self-descriptive Message - message must include enough info of how to handle the message (e.g. content-type: application/json, etc)
   - Hypermedia/Hypertext as the Engine of Application State - driven by a spec of how to consume and use API -> allow a self-documents of API
2) Client-Server - Separated client and server design (it can evolve independently)
3) Statelessness - a request contains all the necessary info to serve
4) Layered system - REST restricts knowledge to a single layer of a next one (cannot directly link to beyond layers) -> to reduce complexity
5) Cacheable - response message must explicitly say a cached or not info (e.g. ETAG, Cache-Expired, etc)
6) Code-on-demand (optional) - server can extend client functionality

### Level-3 API (aka. Richardson Maturity Model)
Level 3 API can meet a minimum requirement of RESTful-ness.
- Level 0 (The Swamp of POX) - HTTP Protocol, RPC-style implementation
- Level 1 (Resources) - each resource mapping with a URI, simpler response
- Level 2 (Verbs) - HTTP Method (GET, POST, PUT, PATCH, DELETE), correct status code according to a method
- Level 3 (Hypermedia) - Hypermedia(HATEOAS) supported, discoverability -> response (resource + link to drive application state) : self-documentation

### Resource Naming Guidelines
- noun only on URI, and other actions can be on query string
- structuring : [host]/[resource]/{id}
  > remove redundancies between [host] and [resource], plurals could be recommended on [resource], order of the pair between [resource] and {resource-id} so that it's predictable  
  > for multiple resources : [host]/[resource1]/{resource1-id}/[resource2]/{resource2-id}...
  > exceptional case : sometimes, RPC-style calls don't easily map to pluralised resource names

### Mothods
#### GET
- filter and searching : it would be efficient to use query string for this.
  > searching is totally relying on API capacity
- deferred execution : by building a dedicated Class of request or response and handing it over to parameter of repository, detach the dependency of repository from controller
