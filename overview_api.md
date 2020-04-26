# Application programming interface

An application programming interface (API) is a computing interface to a software component or a system, that defines how other components or systems can use it. It defines the kinds of calls or requests that can be made, how to make them, the data formats that should be used, the conventions to follow, etc. It can also provide extension mechanisms so that users can extend existing functionality in various ways and to varying degrees. An API can be entirely custom, specific to a component, or it can be designed based on an industry standard to ensure interoperability. Some APIs have to be documented, others are designed so that they can be "interrogated" to determine supported functionality. Since other components/systems rely only on the API, the system that provides the API can (ideally) change its internal details "behind" that API without affecting its users. 
Today, with the rise of REST and web services over HTTP, the term is often assumed to refer to APIs of such services when given no other context.

> REST (Representational state transfer) is a software architectural style that defines a set of constraints to be used for creating Web services. Web services that conform to the REST architectural style, called RESTful Web services, provide interoperability between computer systems on the Internet.

> SOAP (Simple Object Access Protocol) is a messaging protocol specification for exchanging structured information in the implementation of web services in computer networks. Its purpose is to provide extensibility, neutrality, verbosity and independence. It uses XML Information Set for its message format, and relies on application layer protocols, most often Hypertext Transfer Protocol (HTTP), although some legacy systems communicate over Simple Mail Transfer Protocol (SMTP), for message negotiation and transmission. 

### Libraries and frameworks

An API usually is related to a software library. The API describes and prescribes the "expected behavior" (a specification) while the library is an "actual implementation" of this set of rules.

A single API can have multiple implementations (or none, being abstract) in the form of different libraries that share the same programming interface.

The separation of the API from its implementation can allow programs written in one language to use a library written in another. For example, because Scala and Java compile to compatible bytecode, Scala developers can take advantage of any Java API.

API use can vary depending on the type of programming language involved. An API for a procedural language such as Lua could consist primarily of basic routines to execute code, manipulate data or handle errors while an API for an object-oriented language, such as Java, would provide a specification of classes and its class methods.

Language bindings are also APIs. By mapping the features and capabilities of one language to an interface implemented in another language, a language binding allows a library or service written in one language to be used when developing in another language. Tools such as SWIG and F2PY, a Fortran-to-Python interface generator, facilitate the creation of such interfaces.

An API can also be related to a software framework: a framework can be based on several libraries implementing several APIs, but unlike the normal use of an API, the access to the behavior built into the framework is mediated by extending its content with new classes plugged into the framework itself.

### Operating systems

An API can specify the interface between an application and the operating system. POSIX, for example, specifies a set of common APIs that aim to enable an application written for a POSIX conformant operating system to be compiled for another POSIX conformant operating system.

Linux and Berkeley Software Distribution are examples of operating systems that implement the POSIX APIs.

Microsoft has shown a strong commitment to a backward-compatible API, particularly within its Windows API (Win32) library, so older applications may run on newer versions of Windows using an executable-specific setting called "Compatibility Mode".

An API differs from an application binary interface (ABI) in that an API is source code based while an ABI is binary based. For instance, POSIX provides APIs while the Linux Standard Base provides an ABI.

### Remote APIs

Remote APIs allow developers to manipulate remote resources through protocols, specific standards for communication that allow different technologies to work together, regardless of language or platform. For example, the Java Database Connectivity API allows developers to query many different types of databases with the same set of functions, while the Java remote method invocation API uses the Java Remote Method Protocol to allow invocation of functions that operate remotely, but appear local to the developer.

Therefore, remote APIs are useful in maintaining the object abstraction in object-oriented programming; a method call, executed locally on a proxy object, invokes the corresponding method on the remote object, using the remoting protocol, and acquires the result to be used locally as a return value.

### Web APIs

Web APIs are the defined interfaces through which interactions happen between an enterprise and applications that use its assets, which also is a Service Level Agreement (SLA) to specify the functional provider and expose the service path or URL for its API users. An API approach is an architectural approach that revolves around providing a program interface to a set of services to different applications serving different types of consumers.

When used in the context of web development, an API is typically defined as a set of specifications, such as Hypertext Transfer Protocol (HTTP) request messages, along with a definition of the structure of response messages, usually in an Extensible Markup Language (XML) or JavaScript Object Notation (JSON) format. An example might be a shipping company API that can be added to an eCommerce-focused website to facilitate ordering shipping services and automatically include current shipping rates, without the site developer having to enter the shipper's rate table into a web database. While "web API" historically has been virtually synonymous with web service, the recent trend (so-called Web 2.0) has been moving away from Simple Object Access Protocol (SOAP) based web services and service-oriented architecture (SOA) towards more direct representational state transfer (REST) style web resources and resource-oriented architecture (ROA). Part of this trend is related to the Semantic Web movement toward Resource Description Framework (RDF), a concept to promote web-based ontology engineering technologies. Web APIs allow the combination of multiple APIs into new applications known as mashups. In the social media space, web APIs have allowed web communities to facilitate sharing content and data between communities and applications. In this way, content that is created in one place dynamically can be posted and updated to multiple locations on the web. For example, Twitter's REST API allows developers to access core Twitter data and the Search API provides methods for developers to interact with Twitter Search and trends data.

# REST

REST is acronym for REpresentational State Transfer. It is architectural style for distributed hypermedia systems and was first presented by Roy Fielding in 2000 in his famous [dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm).

### Guiding Principles of REST

1. Client–server – By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.

2. Stateless – Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.

3. Cacheable – Cache constraints require that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.

4. Uniform interface – By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform interface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self-descriptive messages; and, hypermedia as the engine of application state.

5. Layered system – The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot “see” beyond the immediate layer with which they are interacting.

6. Code on demand (optional) – REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts. This simplifies clients by reducing the number of features required to be pre-implemented.

### Resource
    
The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service, a collection of other resources, a non-virtual object (e.g. a person), and so on. REST uses a resource identifier to identify the particular resource involved in an interaction between components.
    
The state of the resource at any particular timestamp is known as resource representation. A representation consists of data, metadata describing the data and hypermedia links which can help the clients in transition to the next desired state.
    
The data format of a representation is known as a [media type](https://www.iana.org/assignments/media-types/media-types.xhtml). The media type identifies a specification that defines how a representation is to be processed. A truly RESTful API looks like hypertext. Every addressable unit of information carries an address, either explicitly (e.g., link and id attributes) or implicitly (e.g., derived from the media type definition and representation structure).

> Hypertext (or hypermedia) means the simultaneous presentation of information and controls such that the information becomes the affordance through which the user (or automaton) obtains choices and selects actions. Remember that hypertext does not need to be HTML (or XML or JSON) on a browser. Machines can follow links when they understand the data format and relationship types.

Further, resource representations shall be self-descriptive: the client does not need to know if a resource is employee or device. It should act on the basis of media-type associated with the resource. So in practice, you will end up creating lots of custom media-types – normally one media-type associated with one resource.

### Resource Methods

Another important thing associated with REST is resource methods to be used to perform the desired transition. A large number of people wrongly relate resource methods to HTTP GET/PUT/POST/DELETE methods.

Roy Fielding has never mentioned any recommendation around which method to be used in which condition. All he emphasizes is that it should be uniform interface. If you decide HTTP POST will be used for updating a resource – rather than most people recommend HTTP PUT – it’s alright and application interface will be RESTful.

Another thing which will help you while building RESTful APIs is that query based API results should be represented by a list of links with summary information, not by arrays of original resource representations because the query is not a substitute for identification of resources.

### REST and HTTP are not same !!

A lot of people prefer to compare HTTP with REST. REST and HTTP are not same.

> REST != HTTP

Though, because REST also intends to make the web (internet) more streamline and standard, he advocates using REST principles more strictly. And that’s from where people try to start comparing REST with web (HTTP). Roy fielding, in his dissertation, nowhere mentioned any implementation directive – including any protocol preference and HTTP. Till the time, you are honoring the 6 guiding principles of REST, you can call your interface RESTful.

In simplest words, in the REST architectural style, data and functionality are considered resources and are accessed using Uniform Resource Identifiers (URIs). The resources are acted upon by using a set of simple, well-defined operations. The clients and servers exchange representations of resources by using a standardized interface and protocol – typically HTTP.

Resources are decoupled from their representation so that their content can be accessed in a variety of formats, such as HTML, XML, plain text, PDF, JPEG, JSON, and others. Metadata about the resource is available and used, for example, to control caching, detect transmission errors, negotiate the appropriate representation format, and perform authentication or access control. And most importantly, every interaction with a resource is stateless.

All these principles help RESTful applications to be simple, lightweight, and fast.

# REST Architectural Constraints

### Architectural Constraints

REST defines 6 architectural constraints which make any web service – a true RESTful API.

1. Uniform interface

2. Client–server

3. Stateless

4. Cacheable

5. Layered system

6. Code on demand (optional)

### 1. Uniform interface

As the constraint name itself applies, you MUST decide APIs interface for resources inside the system which are exposed to API consumers and follow religiously. A resource in the system should have only one logical URI, and that should provide a way to fetch related or additional data. It’s always better to synonymize a resource with a web page.

Any single resource should not be too large and contain each and everything in its representation. Whenever relevant, a resource should contain links (HATEOAS) pointing to relative URIs to fetch related information.

Also, the resource representations across the system should follow specific guidelines such as naming conventions, link formats, or data format (XML or/and JSON).

All resources should be accessible through a common approach such as HTTP GET and similarly modified using a consistent approach.

> Once a developer becomes familiar with one of your APIs, he should be able to follow similar approach for other APIs.

### 2. Client–server

This essentially means that client application and server application MUST be able to evolve separately without any dependency on each other. A client should know only resource URIs, and that’s all. Today, this is normal practice in web development, so nothing fancy is required from your side. Keep it simple.

> Servers and clients may also be replaced and developed independently, as long as the interface between them is not altered.

### 3. Stateless

Roy fielding got inspiration from HTTP, so it reflects in this constraint. Make all client-server interactions stateless. The server will not store anything about the latest HTTP request the client made. It will treat every request as new. No session, no history.
       
If the client application needs to be a stateful application for the end-user, where user logs in once and do other authorized operations after that, then each request from the client should contain all the information necessary to service the request – including authentication and authorization details.

> No client context shall be stored on the server between requests. The client is responsible for managing the state of the application.

### 4. Cacheable

In today’s world, caching of data and responses is of utmost important wherever they are applicable/possible. The webpage you are reading here is also a cached version of the HTML page. Caching brings performance improvement for the client-side and better scope for scalability for a server because the load has reduced.
       
In REST, caching shall be applied to resources when applicable, and then these resources MUST declare themselves cacheable. Caching can be implemented on the server or client-side.

> Well-managed caching partially or completely eliminates some client-server interactions, further improving scalability and performance.

### 5. Layered system

REST allows you to use a layered system architecture where you deploy the APIs on server A, and store data on server B and authenticate requests in Server C, for example. A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.

### 6. Code on demand (optional)

Well, this constraint is optional. Most of the time, you will be sending the static representations of resources in the form of XML or JSON. But when you need to, you are free to return executable code to support a part of your application, e.g., clients may call your API to get a UI widget rendering code. It is permitted.

> All the above constraints help you build a truly RESTful API, and you should follow them. Still, at times, you may find yourself violating one or two constraints. Do not worry; you are still making a RESTful API – but not “truly RESTful.”

# REST Resource Naming Guide

In REST, primary data representation is called Resource. Having a strong and consistent REST resource naming strategy – will definitely prove one of the best design decisions in the long term.

> The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. “today’s weather in Los Angeles”), a collection of other resources, a non-virtual object (e.g., a person), and so on. In other words, any concept that might be the target of an author’s hypertext reference must fit within the definition of a resource. A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.

A resource can be a singleton or a collection. For example, ```customers``` is a collection resource and ```customer``` is a singleton resource (in a banking domain). We can identify ```customers``` collection resource using the URI ```/customers```. We can identify a single ```customer``` resource using the URI ```/customers/{customerId}```.

A resource may contain sub-collection resources also. For example, sub-collection resource ```accounts``` of a particular ```customer``` can be identified using the URN ```/customers/{customerId}/accounts``` (in a banking domain). Similarly, a singleton resource ```account``` inside the sub-collection resource ```accounts``` can be identified as follows: ```/customers/{customerId}/accounts/{accountId}```.

REST APIs use Uniform Resource Identifiers (URIs) to address resources. REST API designers should create URIs that convey a REST API’s resource model to its potential client developers. When resources are named well, an API is intuitive and easy to use. If done poorly, that same API can feel difficult to use and understand.

The constraint of uniform interface is partially addressed by the combination of URIs and HTTP verbs and using them in line with the standards and conventions.

# REST Resource Naming Best Practices

### Use nouns to represent resources

RESTful URI should refer to a resource that is a thing (noun) instead of referring to an action (verb) because nouns have properties which verbs do not have – similar to resources have attributes. Some examples of a resource are:

- Users of the system

- User Accounts

- Network Devices etc.

and their resource URIs can be designed as below:

```
http://api.example.com/device-management/managed-devices 
http://api.example.com/device-management/managed-devices/{device-id} 
http://api.example.com/user-management/users/
http://api.example.com/user-management/users/{id}
```

For more clarity, let’s divide the resource archetypes into four categories (document, collection, store and controller) and then you should always target to put a resource into one archetype and then use it’s naming convention consistently. For uniformity’s sake, resist the temptation to design resources that are hybrids of more than one archetype.

#### 1. document

A document resource is a singular concept that is akin to an object instance or database record. In REST, you can view it as a single resource inside resource collection. A document’s state representation typically includes both fields with values and links to other related resources.

Use “singular” name to denote document resource archetype.

```
http://api.example.com/device-management/managed-devices/{device-id}
http://api.example.com/user-management/users/{id}
http://api.example.com/user-management/users/admin
```

#### 2. collection

A collection resource is a server-managed directory of resources. Clients may propose new resources to be added to a collection. However, it is up to the collection to choose to create a new resource or not. A collection resource chooses what it wants to contain and also decides the URIs of each contained resource.

Use “plural” name to denote collection resource archetype.

```
http://api.example.com/device-management/managed-devices
http://api.example.com/user-management/users
http://api.example.com/user-management/users/{id}/accounts
```

#### 3. store

A store is a client-managed resource repository. A store resource lets an API client put resources in, get them back out, and decide when to delete them. A store never generates new URIs. Instead, each stored resource has a URI that was chosen by a client when it was initially put into the store.

Use “plural” name to denote store resource archetype.

```
http://api.example.com/cart-management/users/{id}/carts
http://api.example.com/song-management/users/{id}/playlists
```

#### 4. controller

A controller resource models a procedural concept. Controller resources are like executable functions, with parameters and return values; inputs and outputs.

Use “verb” to denote controller archetype.

```
http://api.example.com/cart-management/users/{id}/cart/checkout
http://api.example.com/song-management/users/{id}/playlist/play
```

### Consistency is the key

#### 1. Use consistent resource naming conventions and URI formatting for minimum ambiguily and maximum readability and maintainability. You may implement below design hints to achieve consistency:

Use forward slash (/) to indicate hierarchical relationships

The forward slash (/) character is used in the path portion of the URI to indicate a hierarchical relationship between resources. e.g.

```
http://api.example.com/device-management
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices/{id}
http://api.example.com/device-management/managed-devices/{id}/scripts
http://api.example.com/device-management/managed-devices/{id}/scripts/{id}
```

#### 2. Do not use trailing forward slash (/) in URIs

As the last character within a URI’s path, a forward slash (/) adds no semantic value and may cause confusion. It’s better to drop them completely.

```
http://api.example.com/device-management/managed-devices/
http://api.example.com/device-management/managed-devices 	/*This is much better version*/
```

#### 3. Use hyphens (-) to improve the readability of URIs

To make your URIs easy for people to scan and interpret, use the hyphen (-) character to improve the readability of names in long path segments.

```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory-management/managedEntities/{id}/installScriptLocation  //Less readable
```

#### 4. Do not use underscores ( _ )

It’s possible to use an underscore in place of a hyphen to be used as separator – But depending on the application’s font, it’s possible that the underscore (_) character can either get partially obscured or completely hidden in some browsers or screens.

To avoid this confusion, use hyphens (-) instead of underscores ( _ ).

```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory_management/managed_entities/{id}/install_script_location  //More error prone
```

#### 5. Use lowercase letters in URIs

When convenient, lowercase letters should be consistently preferred in URI paths.

[RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) defines URIs as case-sensitive except for the scheme and host components. e.g.

```
http://api.example.org/my-folder/my-doc  //1
HTTP://API.EXAMPLE.ORG/my-folder/my-doc  //2
http://api.example.org/My-Folder/my-doc  //3
```

In above examples, 1 and 2 are same but 3 is not as it uses My-Folder in capital letters.

#### 6. Do not use file extentions

File extensions look bad and do not add any advantage. Removing them decreases the length of URIs as well. No reason to keep them.

Apart from above reason, if you want to highlight the media type of API using file extenstion then you should rely on the media type, as communicated through the Content-Type header, to determine how to process the body’s content.

```
http://api.example.com/device-management/managed-devices.xml  /*Do not use it*/
http://api.example.com/device-management/managed-devices 	/*This is correct URI*/
```

### Never use CRUD function names in URIs

URIs should not be used to indicate that a CRUD function is performed. URIs should be used to uniquely identify resources and not any action upon them. HTTP request methods should be used to indicate which CRUD function is performed.

```
HTTP GET http://api.example.com/device-management/managed-devices  //Get all devices
HTTP POST http://api.example.com/device-management/managed-devices  //Create new Device

HTTP GET http://api.example.com/device-management/managed-devices/{id}  //Get device for given Id
HTTP PUT http://api.example.com/device-management/managed-devices/{id}  //Update device for given Id
HTTP DELETE http://api.example.com/device-management/managed-devices/{id}  //Delete device for given Id
```

### Use query component to filter URI collection

Many times, you will come across requirements where you will need a collection of resources sorted, filtered or limited based on some certain resource attribute. For this, do not create new APIs – rather enable sorting, filtering and pagination capabilities in resource collection API and pass the input parameters as query parameters. e.g.

```
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices?region=USA
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ&sort=installation-date
```

# Caching REST API Response

Caching is the ability to store copies of frequently accessed data in several places along the request-response path. When a consumer requests a resource representation, the request goes through a cache or a series of caches (local cache, proxy cache, or reverse proxy) toward the service hosting the resource. If any of the caches along the request path has a fresh copy of the requested representation, it uses that copy to satisfy the request. If none of the caches can satisfy the request, the request travels all the way to the service (or origin server as it is formally known).

Using HTTP headers, an origin server indicates whether a response can be cached and, if so, by whom, and for how long. Caches along the response path can take a copy of a response, but only if the caching metadata allows them to do so.

Optimizing the network using caching improves the overall quality-of-service in the following ways:

- Reduce bandwidth

- Reduce latency

- Reduce load on servers

- Hide network failures

### Caching in REST APIs

Being cacheable is one of architectural constraints of REST. GET requests should be cachable by default – until special condition arises. Usually, browsers treat all GET requests cacheable. POST requests are not cacheable by default but can be made cacheable if either an ```Expires``` header or a ```Cache-Control``` header with a directive, to explicitly allows caching, is added to the response. Responses to ```PUT``` and ```DELETE``` requests are not cacheable at all.

There are two main HTTP response headers that we can use to control caching behavior:

#### Expires

The Expires HTTP header specifies an absolute expiry time for a cached representation. Beyond that time, a cached representation is considered stale and must be re-validated with the origin server. To indicate that a representation never expires, a service can include a time up to one year in the future.

```Expires: Fri, 20 May 2016 19:20:49 IST```

#### Cache-Control

The header value comprises one or more comma-separated directives. These directives determine whether a response is cacheable, and if so, by whom, and for how long e.g. ```max-age``` or ```s-maxage``` directives.

```Cache-Control: max-age=3600```

Cacheable responses (whether to a GET or to a POST request) should also include a validator — either an ETag or a Last-Modified header.

#### ETag
 
An ETag value is an opaque string token that a server associates with a resource to uniquely identify the state of the resource over its lifetime. When the resource changes, the ETag changes accordingly.

```ETag: "abcd1234567n34jv"```

#### Last-Modified

Whereas a response’s Date header indicates when the response was generated, the Last-Modified header indicates when the associated resource last changed. The Last-Modified value cannot be less than the Date value.

```Last-Modified: Fri, 10 May 2016 09:17:49 IST```

# REST Resource Representation Compression

REST APIs can return the resource representations in several formats such as XML, JSON, HTML, or even plain text. All such forms can be compressed to a lesser number of bytes to save bandwidth over the network. Different protocols use different techniques to enable compression and notify the clients about the compression scheme – so that the client can decompress it before consuming the representations.

> Compression, like encryption, is something that happens to a representation in transit and must be undone before the client can use the representation.

HTTP is most widely used protocol for REST – so I am taking example of HTTP specific response compression.

### Compression Related Request/Response Headers

#### Accept-Encoding

While requesting resource representations – along with an HTTP request, the client sends an Accept-Encoding header that says what kind of compression algorithms the client understands.

The two standard values for ```Accept-Encoding``` are compress and gzip.

A sample request with ```accept-encoding``` header looks like this :

```
GET        /employees         HTTP/1.1
Host:     www.domain.com
Accept:     text/html
Accept-Encoding:     gzip,compress
```

Other possible usage of accept-encoding may be:

```
Accept-Encoding: compress, gzip
Accept-Encoding:
Accept-Encoding: *
Accept-Encoding: compress;q=0.5, gzip;q=1.0
Accept-Encoding: gzip;q=1.0, identity; q=0.5, *;q=0
```

If an ```Accept-Encoding``` field is present in a request, and if the server cannot send a response which is acceptable according to the ```Accept-Encoding``` header, then the server SHOULD send an error response with the **406 (Not Acceptable)** status code.

#### Content-Encoding

If the server understands one of the compression algorithms from Accept-Encoding, it can use that algorithm to compress the representation before serving it. When successfully compressed, server lets know the client of encoding scheme by another HTTP header i.e. ```Content-Encoding```.

```
200 OK
Content-Type:     text/html
Content-Encoding:     gzip
```

If the content-coding of an entity in a request message is not acceptable to the origin server, the server SHOULD respond with a status code of **415 (Unsupported Media Type)**. If multiple encodings have been applied to an entity, the content encodings MUST be listed in the order in which they were applied.

Please note that the original media-type for request and response are not impacted whether compression is requested or not.

Compression can save a lot of bandwidth, with minimal cost without additional complexity. Also, you may know that most web browsers automatically request compressed representations from website host servers – using the above headers.

# REST – Content Negotiation

Generally, resources can have multiple presentations, mostly because there may be multiple different clients expecting different representations. Asking for a suitable presentation by a client, is referred as content negotiation.

> HTTP has provisions for several mechanisms for “content negotiation” — the process of selecting the best representation for a given response when there are multiple representations available.   - [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec12.html)

### Server-driven Vs Agent-driven Content Negotiation

If the selection of the best representation for a response is made by an algorithm located at the server, it is called server-driven negotiation. If that selection is made at agent or client side, its called agent-driven content negotiation.

Practically, you will NOT find much usage of server side negotiations because in that way, you have to make lots of assumptions about client expectations. Few things like client context or how client will use the resource representation is almost impossible to determine. Apart from that this approach makes the server side code complex, unnecessarily.

So, most REST API implementations rely on agent driven content negotiations. Agent driven content negotiation rely on usage of HTTP request headers or resource URI patterns.

#### 1. Content negotiation using HTTP headers

At server side, an incoming request may have an entity attached to it. To determine it’s type, server uses the HTTP request header ```Content-Type```. Some common examples of content types are “text/plain”, “application/xml”, “text/html”, “application/json”, “image/gif”, and “image/jpeg”.

```Content-Type: application/json```

Similarly, to determine what type of representation is desired at client side, HTTP header ```ACCEPT``` is used. It will have one of the values as mentioned for ```Content-Type``` above.

```Accept: application/json```

Generally, if no ```Accept``` header is present in the request, the server can send pre-configured default representation type.

> Implementing ```Accept``` header based content negotiation is most used and recommened way.

#### 2. Content negotiation using URL patterns

Another way to pass content type information to server, client may use specific extension in resource URIs. For example, a client can ask for details using:

```
http://rest.api.com/v1/employees/20423.xml
http://rest.api.com/v1/employees/20423.json
```

In above case, first request URI will return a XML response whether second request URI will return a JSON response.

### Defining preferences

If is possible to have multiple values in ```Accept``` header. Client may want to give multiple values in accept header when client is not sure about if its desired representation is present or supported by server at that time. \[[RFC 2296](https://tools.ietf.org/html/rfc2296)]

For example,

```Accept: application/json,application/xml;q=0.9,*/*;q=0.8```

Above ```Accept``` header allows you to ask the server a JSON format. If it can’t, perhaps it could return XML format (the second level). If it’s still not possible, let it return what it can.

The preference order is defined through the q parameter with values from 0 to 1. When nothing is specified, the implicit value is 1.

# HATEOAS Driven REST APIs

**HATEOAS (Hypermedia as the Engine of Application State)** is a constraint of the REST application architecture that keeps the RESTful style architecture unique from most other network application architectures. The term “hypermedia” refers to any content that contains links to other forms of media such as images, movies, and text.

REST architectural style lets you use hypermedia links in the response contents so that the client can dynamically navigate to the appropriate resource by traversing the hypermedia links. Above is conceptually the same as a web user browsing through web pages by clicking the relevant hyperlinks to achieve a final goal.

For example, below given JSON response may be from an API like ```HTTP GET http://api.domain.com/management/departments/10```

```json
{
    "departmentId": 10,
    "departmentName": "Administration",
    "locationId": 1700,
    "managerId": 200,
    "links": [
        {
            "href": "10/employees",
            "rel": "employees",
            "type" : "GET"
        }
    ]
}
```

In the preceding example, the response returned by the server contains hypermedia links to employee resources ```10/employees```, which can be traversed by the client to read employees belonging to the department.

The advantage of the above approach is that hypermedia links returned from the server drive the application’s state and not the other way around.

There is no universally accepted format for representing links between two resources in JSON. You may choose to adopt the above format, or you may choose to send links in HTTP response headers.

```
HTTP/1.1 200 OK
...
Link: <10/employees>; rel="employees"
```

Both are absolutely valid solutions.

### HATEOAS Implementation

In the real world, when you visit a website – you hit its homepage. It presents some snapshots and links to other sections of websites. You click on them, and then you get more information along with more related links that are relevant to the context.

Similar to a human’s interaction with a website, a REST client hits an initial API URI and uses the server-provided links to dynamically discover available actions and access the resources it needs. The client need not have prior knowledge of the service or the different steps involved in a workflow. Additionally, the clients no longer have to hard code the URI structures for different resources. HATEOAS allows the server to make URI changes as the API evolves without breaking the clients.

Above API interaction is possible using HATEOAS only.

Each REST framework provides it’s own way on creating the HATEOAS links using framework capabilities e.g. in this RESTEasy HATEOAS tutorial, links are part of resource model classes which is transferred as resource state to the client.

### HATEOAS References

The following are the two popular formats for specifying JSON REST API hypermedia links:

#### RFC 5988 (web linking)

[RFC 5988](https://tools.ietf.org/html/rfc5988) puts forward a framework for building links that defines the relation between resources on the web. Each link in RFC 5988 contains the following properties:

**Target URI:** Each link should contain a target Internationalized Resource Identifiers (IRIs). This is represented by the ```href``` attribute.

**Link relation type:** The link relation type describes how the current context (source) is related to the target resource. This is represented by the ```rel``` attribute.

**Attributes for target IRI:** The attributes for a link include ```hreflang```, ```media```, ```title```, and ```type```, and any extension link parameters.

#### JSON Hypermedia API Language (HAL)

JSON HAL is a promising proposal that sets the conventions for expressing hypermedia controls, such as links, with JSON or XML. It is in the draft stage at this time.

The two associated MIME types are

```
media type: application/hal+xml 
media type: application/hal+json
```

Each link in HAL may contain the following properties:

**Target URI:** It indicates the target resource URI. This is represented by the ```href``` attribute.

**Link relation:** The link relation type describes how the current context is related to the target resource. This is represented by the ```rel``` attribute.

**Type:** This indicates the expected resource media type. This is represented by the ```type``` attribute.

There is no right or wrong in choosing a hypermedia link format for your application. You should pick up a format that meets most of your use case requirements and stick to it.

# Idempotent REST APIs

In the context of REST APIs, when making multiple identical requests has the same effect as making a single request – then that REST API is called **idempotent**.

When you design REST APIs, you must realize that API consumers can make mistakes. They can write client code in such a way that there can be duplicate requests as well. These duplicate requests may be unintentional as well as intentional some time (e.g. due to timeout or network issues). You have to design fault-tolerant APIs in such a way that duplicate requests do not leave the system unstable.

> An idempotent HTTP method is an HTTP method that can be called many times without different outcomes. It would not matter if the method is called only once, or ten times over. The result should be the same. It essentially means that the result of a successfully performed request is independent of the number of times it is executed. For example, in arithmetic, adding zero to a number is idempotent operation.

If you follow REST principles in designing API, you will have automatically **idempotent REST APIs** for GET, PUT, DELETE, HEAD, OPTIONS and TRACE HTTP methods. Only ```POST``` APIs will not be idempotent.

1. ```POST``` is NOT idempotent.
2. ```GET```, ```PUT```, ```DELETE```, ```HEAD```, ```OPTIONS``` and ```TRACE``` are idempotent.

Let’s analyze how above HTTP methods end up being idempotent – any why POST is not.

### HTTP POST

Generally – not necessarily – ```POST``` APIs are used to create a new resource on server. So when you invoke the same POST request N times, you will have N new resources on the server. So, **POST is not idempotent**.

### HTTP GET, HEAD, OPTIONS and TRACE

```GET```, ```HEAD```, ```OPTIONS``` and ```TRACE``` methods NEVER change the resource state on server. They are purely for retrieving the resource representation or meta data at that point of time. So invoking multiple requests will not have any write operation on server, so **GET, HEAD, OPTIONS and TRACE are idempotent**.

### HTTP PUT

Generally – not necessarily – ```PUT``` APIs are used to update the resource state. If you invoke a ```PUT``` API N times, the very first request will update the resource; then rest N-1 requests will just overwrite the same resource state again and again – effectively not changing anything. Hence, **PUT is idempotent**.

### HTTP DELETE

When you invoke N similar ```DELETE``` requests, first request will delete the resource and response will be ```200 (OK)``` or ```204 (No Content)```. Other N-1 requests will return ```404 (Not Found)```. Clearly, the response is different from first request, but there is no change of state for any resource on server side because original resource is already deleted. So, **DELETE is idempotent**.

Please keep in mind if some systems may have DELETE APIs like this:

```DELETE /item/last```

In the above case, calling operation N times will delete N resources – hence ```DELETE``` is not idempotent in this case. In this case, a good suggestion might be to change above API to POST – because POST is not idempotent.

```POST /item/last```

Now, this is closer to HTTP spec – hence more REST compliant.

# REST API Security Essentials

Security isn’t an afterthought. It has to be an integral part of any development project and also for REST APIs. There are multiple ways to secure a RESTful API e.g. basic auth, OAuth etc. but one thing is sure that RESTful APIs should be stateless – so request authentication/authorization should not depend on cookies or sessions. Instead, each API request should come with some sort authentication credentials which must be validated on the server for each and every request.

### REST Security Design Principles

The paper “The Protection of Information in Computer Systems” by Jerome Saltzer and Michael Schroeder, put forth eight design principles for securing information in computer systems, as described in the following sections:

1. **Least Privilege:** An entity should only have the required set of permissions to perform the actions for which they are authorized, and no more. Permissions can be added as needed and should be revoked when no longer in use.
2. **Fail-Safe Defaults:** A user’s default access level to any resource in the system should be “denied” unless they’ve been granted a “permit” explicitly.
3. **Economy of Mechanism:** The design should be as simple as possible. All the component interfaces and the interactions between them should be simple enough to understand.
4. **Complete Mediation:** A system should validate access rights to all its resources to ensure that they’re allowed and should not rely on the cached permission matrix. If the access level to a given resource is being revoked, but that isn’t reflected in the permission matrix, it would violate the security.
5. **Open Design:** This principle highlights the importance of building a system in an open manner—with no secret, confidential algorithms.
6. **Separation of Privilege:** Granting permissions to an entity should not be purely based on a single condition, a combination of conditions based on the type of resource is a better idea.
7. **Least Common Mechanism:** It concerns the risk of sharing state among different components. If one can corrupt the shared state, it can then corrupt all the other components that depend on it.
8. **Psychological Acceptability:** It states that security mechanisms should not make the resource more difficult to access than if the security mechanisms were not present. In short, security should not make worse the user experience.

### Best Practices to Secure REST APIs

Below given points may serve as a checklist for designing the security mechanism for REST APIs.

#### Keep it Simple

Secure an API/System – just how secure it needs to be. Every time you make the solution more complex “unnecessarily,” you are also likely to leave a hole.

#### Always Use HTTPS

By always using SSL, the authentication credentials can be simplified to a randomly generated access token that is delivered in the username field of HTTP Basic Auth. It’s relatively simple to use, and you get a lot of security features for free.

If you use HTTP 2, to improve performance – you can even send multiple requests over a single connection, that way you avoid the complete TCP and SSL handshake overhead on later requests.

#### Use Password Hash

Passwords must always be hashed to protect the system (or minimize the damage) even if it is compromised in some hacking attempts. There are many such hashing algorithms which can prove really effective for password security e.g. PBKDF2, bcrypt and scrypt algorithms.

#### Never expose information on URLs

Usernames, passwords, session tokens, and API keys should not appear in the URL, as this can be captured in web server logs, which makes them easily exploitable.

```https://api.domain.com/user-management/users/{id}/someAction?apiKey=abcd123456789  //Very BAD !!```

The above URL exposes the API key. So, never use this form of security.

#### Consider OAuth

Though basic auth is good enough for most of the APIs and if implemented correctly, it’s secure as well – yet you may want to consider OAuth as well. The OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.

#### Consider Adding Timestamp in Request

Along with other request parameters, you may add a request timestamp as an HTTP custom header in API requests. The server will compare the current timestamp to the request timestamp and only accepts the request if it is within a reasonable timeframe (1-2 minutes, perhaps).

This will prevent very basic replay attacks from people who are trying to brute force your system without changing this timestamp.

#### Input Parameter Validation

Validate request parameters on the very first step, before it reaches to application logic. Put strong validation checks and reject the request immediately if validation fails. In API response, send relevant error messages and example of correct input format to improve user experience.

# REST API Versioning

To manage this complexity, version your API. Versioning helps you iterate faster when the needed changes are identified.

> Change in an API is inevitable as your knowledge and experience of a system improve. Managing the impact of this change can be quite a challenge when it threatens to break existing client integration.

### When to Version

APIs only need to be up-versioned when a breaking change is made. Breaking changes include:

- a change in the format of the response data for one or more calls
- a change in the response type (i.e. changing an integer to a float)
- removing any part of the API.

**Breaking changes** should always result in a change to the major version number for an API or content [response type](https://www.iana.org/assignments/media-types/media-types.xhtml).

**Non-breaking changes**, such as adding new endpoints or new response parameters, do not require a change to the major version number. However, it can be helpful to track the minor versions of APIs when changes are made to support customers who may be receiving cached versions of data or may be experiencing other API issues.

### How to Version

REST doesn’t provide for any specific versioning guidelines but the more commonly used approaches fall into three categories:

### URI Versioning

Using the URI is the most straightforward approach (and most commonly used as well) though it does violate the principle that a URI should refer to a unique resource. You are also guaranteed to break client integration when a version is updated.

e.g.

```
http://api.example.com/v1
http://apiv1.example.com
```

The version need not be numeric, nor specified using the “v[x]” syntax. Alternatives include dates, project names, seasons or other identifiers that are meaningful enough to the team producing the APIs and flexible enough to change as the versions change.

### Versioning using Custom Request Header

A custom header (e.g. Accept-version) allows you to preserve your URIs between versions though it is effectively a duplicate of the content negotiation behavior implemented by the existing Accept header.

e.g.

```
Accept-version: v1
Accept-version: v2
```

### Versioning using Accept header

Content negotiation may let you preserve a clean set of URLs but you still have to deal with the complexity of serving different versions of content somewhere. This burden tends to be moved up the stack to your API controllers which become responsible for figuring out which version of a resource to send. The end result tends to be a more complex API as clients have to know which headers to specify before requesting a resource.

e.g.

```
Accept: application/vnd.example.v1+json
Accept: application/vnd.example+json;version=1.0
```

In the real world, an API is never going to be completely stable. So it’s important how this change is managed. A well documented and gradual deprecation of API can be an acceptable practice for most of the APIs.

# Statelessness

As per the REST (REpresentational “**State**” Transfer) architecture, the server does not store any state about the client session on the server side. This restriction is called Statelessness. Each request from the client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client. client is responsible for storing and handling all application state related information on client side.

It also means that the client is responsible for sending any state information to the server whenever it is needed. There should not be any session affinity or sticky sessions on server.

> Statelessness means that every HTTP request happens in complete isolation. When the client makes an HTTP request, it includes all information necessary for the server to fulfill that request. The server never relies on information from previous requests. If that information was important, the client would have sent it again in this request.

To enable clients to access these stateless APIs, its necessary that servers also should include every piece of information that the client may need to create the state on its side.

For becoming stateless, do not store even authentication/authorization details of client. Provide credentials with the request. Each request MUST stand alone and should not be affected by the previous conversation happened from the same client in past.

### Application State vs Resource State

Please do not confuse between application state and resource state. Both are completely different things.

**Application state** is server-side data which servers store to identify incoming client requests, their previous interaction details, and current context information.

**Resource state** is the current state of a resource on a server at any point of time – and it has nothing to do with the interaction between client and server. It is what you get as a response from the server as API response. You refer to it as resource representation.

**REST statelessness means being free on application state.**

### Advantages of Statelessness

There are some very noticeable advantages for having **REST APIs stateless**.

1. Statelessness helps in scaling the APIs to millions of concurrent users by deploying it to multiple servers. Any server can handle any request because there is no session related dependency.
2. Being stateless makes REST APIs less complex – by removing all server-side state synchronization logic.
3. A stateless API is also easy to cache as well. A specific software can decide whether or not to cache the result of an HTTP request just by looking at that one request. There’s no nagging uncertainty that state from a previous request might affect the cacheability of this one. It improves the performance of applications.
4. The server never loses track of “where” each client is in the application because the client sends all necessary information with each request.

Reference: Roy T. Fielding on [Stateless](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1_3)

# REST API – N+1 Problem

**N+1 problem** is mostly talked in context of ORMs. In this problem, the system needs to load N children of a parent entity where only parent entity was requested for. By default, ORMs are configured with lazy-loading enabled, so 1 query issued for the parent entity causes N more queries i.e. one each for N child entities.

This N+1 problem is often considered a major performance bottleneck and so shall be solved at the design level of application.

### N+1 Problem in REST APIs

Though mostly directly associated, yet the N+1 problem is not specific to ORMs only. This can be associated with the context of web APIs as well e.g. REST APIs.

In case of web APIs, N+1 problem is a situation where client applications are required to call the server N+1 times to fetch 1 collection resource + N client resources, mostly because of collection resource not had enough information about child resources to build its user interface completely.

For example, a REST API returning a collection of books as a resource.

```xml
<books uri="/books" size="100">
    <book uri="/books/1" id="1">
        <isbn>3434253561</isbn>
    </book>
    <book uri="/books/2" id="2">
        <isbn>3423423534</isbn>
    </book>
    <book uri="/books/3" id="3">
        <isbn>5352342344</isbn>
    </book>
    ...
    ...
</books>
```

Here ```/books``` resource return list of books with information including only it’s ```id``` and ```isbn```. This information is clearly not enough to build a client application UI which will want to typically show the books ```name``` in UI rather than ISBN. It may be that they want to show other information such as author and publication year as well.

In above scenario, client application MUST make N more requests for each individual book resource at ```/books/{id}```. So in the total client will end up invoking REST APIs N+1 times.

Above scenario is only for example. Idea is that insufficient information in collection resources may lead to N+1 problem in REST APIs.

### How to Solve N+1 Problem

The good thing about the previously discussed problem is that we know what exactly is the issue. And this makes the solution pretty easy. **Include more information in individual resources inside collection resource**.

You may consult with API consumers, do market research for similar applications and their user interfaces or simply put yourself in the client’s shoe.

Moreover, you may evolve your APIs over the time as your understanding around client requirements improve. This is possible using API versioning.

# ‘q’ Parameter in HTTP ‘Accept’ Header

A REST API can return the resource representation in many formats – to be more specific **MIME-types**. A client application or browser can request for any supported MIME type in HTTP Accept header. Technically, ```Accept``` header can have multiple values in form of comma separated values.

For example, an ```Accept``` header requesting for ```text/html``` or ```application/xml``` formats can be set as:

```Accept : text/html,application/xml```

### The ‘q’ Parameter

Sometimes client may want to set their preferences when requesting multiple ```MIME``` types. To set this preference, ```q``` parameter (relative quality factor) is used.

**Value of ```q``` parameter can be from 0 to 1**. 0 is lowest value (i.e. least preferred) and 1 is highest (i.e. most preferred).

A sample usage can be:

```Accept : text/html, application/xml;q=0.9, */*;q=0.8```

In above example, client is indicating the server that it will prefer to have the response in ```text/html``` format, first. It server does not support ```text/html``` format for requested resource than it shall send ```application/xml``` format. If none of both formats are available, then send the response in whatever format it support (```*/*```).

- One of the benefit of ‘q’ parameter is to minimize the client-server interactions, which could have happened due to failed content negotiations.
- It also allows clients to receive content types of which they may not be aware, an asterisk “*” may be used in place of either the second half of MIME type value or both halves.

Here’s how the HTTP spec defines it:

> Each media-range MAY be followed by one or more accept-params, beginning with the “q” parameter for indicating a relative quality factor. The first “q” parameter (if any) separates the media-range parameter(s) from the accept-params. Quality factors allow the user or user agent to indicate the relative degree of preference for that media-range, using the ‘q’ value scale from 0 to 1. The default value is 1.

If there are two MIME types for given same ```q``` value, then more specific type, between both, wins.

For example if both ```application/xml``` and ```*/*``` had a preference of 0.9 then ```application/xml``` will be served by the server.

> If no ```Accept``` header field is present, then it is assumed that the client accepts all media types. If an ```Accept``` header field is present, and if the server cannot send a response which is acceptable according to the combined ```Accept``` field value, then the server SHOULD send a ```406 (not acceptable)``` response.
