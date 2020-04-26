# Confidentiality, Integrity, and Availability

### Confidentiality

Confidentiality means that someone gains access to information who shouldn't have access to it.

### Integrity

Integrity refers to ensuring the authenticity of information—that information is not altered, and that the source of the information is genuine.

### Availability

Availability means that information is accessible by authorized users.

# Introduction to cross-site scripting 

### What is cross-site scripting and why should I care?

Cross-site scripting (XSS) is a security bug that can affect websites. If present in your website, this bug can allow an attacker to add their own malicious JavaScript code onto the HTML pages displayed to your users. Once executed by the victim's browser, this code could then perform actions such as completely changing the behavior or appearance of the website, stealing private data, or performing actions on behalf of the user. 

### Preventing XSS

#### What can I do to prevent XSS?

A common technique for preventing XSS vulnerabilities is "escaping". The purpose of character and string escaping is to make sure that every part of a string is interpreted as a string primitive, not as a control character or code.

For example, ```&lt;``` is the HTML encoding for the ```<``` character. If you include: 

```html
<script>alert('testing')</script>
```

in the HTML of a page, the script will execute. But if you include: 

```html
&lt;script&gt;alert('testing')&lt;/script&gt;
```

in the HTML of a page, it will print out the text ```<script>alert('testing')</script>```, but it will not actually execute the script. By escaping the \<script> tags, we prevented the script from executing. Technically, what we did here is "encoding" not "escaping", but "escaping" conveys the basic concept (and we'll see later that in the case of JavaScript, "escaping" actually is the correct term).

The following can help minimize the chances that your website will contain XSS vulnerabilities: 

- Using a template system with context-aware auto-escaping

- Manually escaping user input (if it’s not possible to use a template system with context-aware auto-escaping)

- Understanding common browser behaviors that lead to XSS

- Learning the best practices for your technology

# What is the SQL Injection Vulnerability & How to Prevent it?

### Explanation of SQL Injection Vulnerability

As the name suggests, an SQL injection vulnerability allows an attacker to inject malicious input into an SQL statement. To fully understand the issue, we first have to understand how server-side scripting languages handle SQL queries.

For example, let's say functionality in the web application generates a string with the following SQL statement:

```
$statement = "SELECT * FROM users WHERE username = 'bob' AND password = 'mysecretpw'";
```

This SQL statement is passed to a function that sends the string to the connected database where it is parsed, executed and returns a result.

As you might have noticed the statement contains some new, special characters:

- ```*```  (asterisk) is an instruction for the SQL database to return all columns for the selected database row

- ```=```  (equals) is an instruction for the SQL database to only return values that match the searched string

- ```'```  (single quote mark) is used to tell the SQL database where the search string starts or ends

Now consider the following example in which a website user is able to change the values of '$user' and '$password', such as in a login form:

```
$statement = "SELECT * FROM users WHERE username = '$user' AND password
= '$password'";
```

An attacker can easily insert any special SQL syntax inside the statement, if the input is not sanitized by the application:

```
$statement = "SELECT * FROM users WHERE username = 'admin'; -- ' AND password
= 'anything'";
= 'anything'";
```

What is happening here? (admin'; --) is the attacker's input, which contains two new, special characters:


- ```;``` (semicolon) is used to instruct the SQL parser that the current statement has ended (not necessary in most cases)

- ```--``` (double hyphen) instructs the SQL parser that the rest of the line (shown in light grey above) is a comment and should not be executed

This SQL injection effectively removes the password verification, and returns a dataset for an existing user – 'admin' in this case. The attacker can now log in with an administrator account, without having to specify a password.

### The Different Types of SQL Injection Vulnerability

Attackers can exfiltrate data from servers by exploiting SQL Injection vulnerabilities in various ways. Common methods include retrieving data based on: errors, conditions (true/false) and timing . Let’s look at the variants.

#### Error-Based SQL Injection

When exploiting an error-based SQL Injection vulnerability, attackers can retrieve information such as table names and content from visible database errors.

Error-Based SQL Injection Example

```
https://example.com/index.php?id=1+and(select 1 FROM(select count(*),concat((select (select concat(database())) FROM information_schema.tables LIMIT 0,1),floor(rand(0)*2))x FROM information_schema.tables GROUP BY x)a)
```

This Request Returned an Error

```
Duplicate entry 'database1' for key 'group_key'
```

The same method works for table names and content. Disabling error messages on production systems helps to prevent attackers from gathering such information.

#### Boolean-Based SQL Injection

Sometimes there is no visible error message on the page when an SQL query fails, making it difficult for an attacker to get information from the vulnerable application. However there is still a  way to extract information.

When an SQL query fails, sometimes some parts of the web page disappear or change, or the entire website can fail to load. These indications allow attackers to determine whether the input parameter is vulnerable and whether it allows extraction of data.

Attackers can test for this by inserting a condition into an SQL query:

```
https://example.com/index.php?id=1+AND+1=1
```

If the page loads as usual, it might indicate that it is vulnerable to an SQL Injection. To be sure, an attacker typically tries to provoke a false result using something like this:

```
https://example.com/index.php?id=1+AND+1=2
```

Since the condition is false, if no result is returned or the page does not work as usual (missing text or a white page is displayed, for example), it might indicate that the page is vulnerable to an SQL injection.

Here is an example of how to extract data in this way:

```
https://example.com/index.php?id=1+AND+IF(version()+LIKE+'5%',true,false)
```

With this request, the page should load as usual if the database version is 5.X. But, it will behave differently (display an empty page, for example) if the version is different, indicating whether it it is vulnerable to an SQL injection.

#### Time-Based SQL Injection

In some cases, even though a vulnerable SQL query does not have any visible effect on the output of the page, it may still be possible to extract information from an underlying database.

Hackers determine this by instructing the database to wait (sleep) a stated amount of time before responding. If the page is not vulnerable, it will load quickly; if it is vulnerable it will take longer than usual to load. This enables hackers to extract data, even though there are no visible changes on the page. The SQL syntax can be similar to the one used in the Boolean-Based SQL Injection Vulnerability.

But to set a measurable sleep time, the 'true' function is changed to something that takes some time to execute, such as 'sleep(3)' which instructs the database to sleep for three seconds:

```
https://example.com/index.php?id=1+AND+IF(version()+LIKE+'5%',sleep(3),false)
```

If the page takes longer than usual to load it is safe to assume that the database version is 5.X.

### Impacts of SQL Injection Vulnerability

There are a number of things an attacker can do when exploiting an SQL injection on a vulnerable website. Usually, it depends on the privileges of the user the web application uses to connect to the database server. By exploiting an SQL injection vulnerability, an attacker can:

- Add, delete, edit or read content in the database

- Read source code from files on the database server

- Write files to the database server

It all depends on the capabilities of the attacker, but the exploitation of an SQL injection vulnerability can even lead to a complete takeover of the database and web server. You can learn more useful tips on how to test the impact of an SQL injection vulnerability on your website by referring to the [SQL injection cheat sheet](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/).

A good way to prevent damage is to restrict access as much as possible (for example, do not connect to the database using the sa or root account). It is also sensible to have different databases for different purposes (for example, separating the database for the shop system and the support forum of your website).

# Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin. A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, or port) from its own.

An example of a cross-origin request: the front-end JavaScript code served from ```https://domain-a.com``` uses ```XMLHttpRequest``` to make a request for ```https://domain-b.com/data.json```.

For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts. For example, ```XMLHttpRequest``` and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request resources from the same origin the application was loaded from, unless the response from other origins includes the right CORS headers.

> The Fetch API provides an interface for fetching resources (including across the network). It will seem familiar to anyone who has used XMLHttpRequest, but the new API provides a more powerful and flexible feature set.

> The same-origin policy is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

![](https://media.prod.mdn.mozit.cloud/attachments/2016/10/28/14295/a21a85eaccd405d608395b4ca8d82538/CORS_principle.png)

The CORS mechanism supports secure cross-origin requests and data transfers between browsers and servers. Modern browsers use CORS in APIs such as XMLHttpRequest or Fetch to mitigate the risks of cross-origin HTTP requests.

# XML external entity (XXE) injection

### What is XML external entity injection?

XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any backend or external systems that the application itself can access.

In some situations, an attacker can escalate an XXE attack to compromise the underlying server or other backend infrastructure, by leveraging the XXE vulnerability to perform server-side request forgery (SSRF) attacks. 

### How do XXE vulnerabilities arise?

Some applications use the XML format to transmit data between the browser and the server. Applications that do this virtually always use a standard library or platform API to process the XML data on the server. XXE vulnerabilities arise because the XML specification contains various potentially dangerous features, and standard parsers support these features even if they are not normally used by the application.

XML external entities are a type of custom XML entity whose defined values are loaded from outside of the DTD in which they are declared. External entities are particularly interesting from a security perspective because they allow an entity to be defined based on the contents of a file path or URL. 

### What are the types of XXE attacks?

There are various types of XXE attacks:

- Exploiting XXE to retrieve files, where an external entity is defined containing the contents of a file, and returned in the application's response.

- Exploiting XXE to perform SSRF attacks, where an external entity is defined based on a URL to a back-end system.

- Exploiting blind XXE exfiltrate data out-of-band, where sensitive data is transmitted from the application server to a system that the attacker controls.

- Exploiting blind XXE to retrieve data via error messages, where the attacker can trigger a parsing error message containing sensitive data.

### How to prevent XXE vulnerabilities

Virtually all XXE vulnerabilities arise because the application's XML parsing library supports potentially dangerous XML features that the application does not need or intend to use. The easiest and most effective way to prevent XXE attacks is to disable those features.

Generally, it is sufficient to disable resolution of external entities and disable support for ```XInclude```. This can usually be done via configuration options or by programmatically overriding default behavior. Consult the documentation for your XML parsing library or API for details about how to disable unnecessary capabilities. 

# JSON Web Token (JWT) explained

JSON Web Token is a standard used to create access tokens for an application.

It works this way: the server generates a token that certifies the user identity, and sends it to the client.

The client will send the token back to the server for every subsequent request, so the server knows the request comes from a particular identity.

This architecture proves to be very effective in modern Web Apps, where after the user is authenticated we perform API requests either to a REST or a GraphQL API.

Who uses JWT? Google, for example. If you use the Google APIs, you will use JWT.

A JWT is cryptographically signed (but not encrypted, hence using HTTPS is mandatory when storing user data in the JWT), so there is a guarantee we can trust it when we receive it, as no middleman can intercept and modify it, or the data it holds, without invalidating it.

That said, JWTs are often criticized for their overuse, and especially for them being used when less problematic solutions can be used.

You need to form your opinion around the subject. I’m not advocating for a technology over another, just presenting all the opportunities and tools you have at your disposal.

What are they good for? Mainly API authentication, and server-to-server authorization.

### API authentication

This is probably the only sensible way to use JWT.

A common scenario is: you sign up for a service and download a JWT from the service dashboard. This is what you will use from now on to authenticate all your requests to the server.

Another use case, which is the opposite, is sending the JWT when you manage the API and clients connect to you, and you want your users to send subsequent requests by just passing the token.

In this case, the client needs to store the token somewhere. Where is the best place? In an **HttpOnly cookie**. The other methods are all prone to XSS attacks and as such they should be avoided. An HttpOnly cookie is not accessible from JavaScript, and is automatically sent to the origin server upon every request, so it perfectly suits the use case.

### Don’t use JWTs as session tokens

You should not use JWTs for sessions. Use a regular server-side session mechanism, as it’s much more efficient and less prone to data exposure.

# What is OAuth? Definition and How it Works

### What is OAuth?

OAuth is an open-standard authorization protocol or framework that provides applications the ability for “secure designated access.” For example, you can tell Facebook that it’s OK for ESPN.com to access your profile or post updates to your timeline without having to give ESPN your Facebook password. This minimizes risk in a major way: In the event ESPN suffers a breach, your Facebook password remains safe.

OAuth doesn’t share password data but instead uses authorization tokens to prove an identity between consumers and service providers. OAuth is an authentication protocol that allows you to approve one application interacting with another on your behalf without giving away your password.

### OAuth Explained

OAuth is about authorization and not authentication. Authorization is asking for permission to do stuff. Authentication is about proving you are the correct person because you know things. OAuth doesn’t pass authentication data between consumers and service providers – but instead acts as an authorization token of sorts.

The common analogy I’ve seen used while researching OAuth is the valet key to your car. The valet key allows the valet to start and move the car but doesn’t give them access to the trunk or the glove box.

![](https://blogvaronis2.wpengine.com/wp-content/uploads/2012/04/oauth-explained-960x584.png)

An OAuth token is like that valet key. As a user, you get to tell the consumers what they can use and what they can’t use from each service provider. You can give each consumer a different valet key. They never have the full key or any of the private data that gives them access to the full key.

### OAuth 1.0 vs. OAuth 2.0

OAuth 2.0 is a complete redesign from OAuth 1.0, and the two are not compatible. If you create a new application today, use OAuth 2.0. This blog only applies to OAuth 2.0, since OAuth 1.0 is deprecated.

OAuth 2.0 is faster and easier to implement. OAuth 1.0 used complicated cryptographic requirements, only supported three flows, and did not scale.

OAuth 2.0, on the other hand, has six flows for different types of applications and requirements, and enables signed secrets over HTTPS. OAuth tokens no longer need to be encrypted on the endpoints in 2.0 since they are encrypted in transit.

# Basic access authentication

In the context of an HTTP transaction, **basic access authentication** is a method for an HTTP user agent (e.g. a web browser) to provide a user name and password when making a request. In basic HTTP authentication, a request contains a header field in the form of ```Authorization: Basic <credentials>```, where credentials is the base64 encoding of id and password joined by a single colon :.

It is specified in RFC 7617 from 2015, which obsoletes [RFC 2617](https://tools.ietf.org/html/rfc2617) from 1999.

### Protocol

#### Server side

When the server wants the user agent to authenticate itself towards the server, the server must respond appropriately to unauthenticated requests.

To unauthenticated requests, the server should return a response whose header contains a HTTP 401 Unauthorized status and a WWW-Authenticate field.

The WWW-Authenticate field for basic authentication is constructed as following: 

```
WWW-Authenticate: Basic realm="User Visible Realm"
```

The server may choose to include the charset parameter from \[rfc:7617 RFC] \[rfc:7617 7617]:

```
WWW-Authenticate: Basic realm="User Visible Realm", charset="UTF-8"
```

This parameter indicates that the server expects the client to use UTF-8 for encoding username and password (see below).

#### Client side

When the user agent wants to send authentication credentials to the server, it may use the Authorization field.

The Authorization field is constructed as follows:

1. The username and password are combined with a single colon (:). This means that the username itself cannot contain a colon.

2. The resulting string is encoded into an octet sequence. The character set to use for this encoding is by default unspecified, as long as it is compatible with US-ASCII, but the server may suggest use of UTF-8 by sending the charset parameter.

3. The resulting string is encoded using a variant of Base64.

4. The authorization method and a space (e.g. "Basic ") is then prepended to the encoded string.

For example, if the browser uses Aladdin as the username and OpenSesame as the password, then the field's value is the base64-encoding of Aladdin:OpenSesame, or QWxhZGRpbjpPcGVuU2VzYW1l. Then the Authorization header will appear as:

```
Authorization: Basic QWxhZGRpbjpPcGVuU2VzYW1l
```

# Transport Layer Security

The security of any connection using Transport Layer Security (TLS) is heavily dependent upon the cipher suites and security parameters selected. This article's goal is to help you make these decisions to ensure the confidentiality and integrity communication between client and server. The Mozilla Operations Security (OpSec) team maintains a wiki entry with reference configurations for servers.

The Transport Layer Security (TLS) protocol is the standard for enabling two networked applications or devices to exchange information privately and robustly. Applications that use TLS can choose their security parameters, which can have a substantial impact on the security and reliability of data. This article provides an overview of TLS and the kinds of decisions you need to make when securing your content.

### HTTP over TLS

TLS provides three primary services that help ensure the safety and security of data exchanged with it:

#### Authentication

Authentication lets each party to the communication verify that the other party is who they claim to be.

#### Encryption

Data is encrypted while being transmitted between the user agent and the server, in order to prevent it from being read and interpreted by unauthorized parties.

#### Integrity

TLS ensures that between encrypting, transmitting, and decrypting the data, no information is lost, damaged, tampered with, or falsified.

A TLS connection starts with a handshake phase where a client and server agree on a shared secret and important parameters, like cipher suites, are negotiated. Once parameters and a data exchange mode where application data, such HTTP, is exchanged.

# TCP/IP Security

TCP/IP is widely used throughout the world to provide network communications.  TCP/IP communications are composed of four layers that work together.  When a user wants to transfer data across networks, the data is passed from the highest layer through intermediate layers to the lowest layer, with each layer adding information.  At each layer, the logical units are typically composed of a header and a payload.  The payload consists of the information passed down from the previous layer, while the header contains layer-specific information such as addresses.  At the application layer, the payload is the actual application data.  The lowest layer sends the accumulated data through the physical network; the data is then passed up through the layers to its destination.  Essentially, the data produced by a layer is encapsulated in a larger container by the layer below it.  The four TCP/IP layers, from highest to lowest, are shown below.

- **Application Layer.**  This layer sends and receives data for particular applications, such as Domain Name System (DNS), HyperText Transfer Protocol (HTTP), and Simple Mail Transfer Protocol (SMTP).

- **Transport Layer.**  This layer provides connection-oriented or connectionless services for transporting application layer services between networks.  The transport layer can optionally assure the reliability of communications.  Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) are commonly used transport layer protocols.

- **Network Layer.**  This layer routes packets across networks.  Internet Protocol (IP) is the fundamental network layer protocol for TCP/IP.  Other commonly used protocols at the network layer are Internet Control Message Protocol (ICMP) and Internet Group Management Protocol (IGMP).

- **Data Link Layer.**  This layer handles communications on the physical network components.  The best-known data link layer protocol is Ethernet.

Security controls exist for network communications at each layer of the TCP/IP model.  As previously explained, data is passed from the highest to the lowest layer, with each layer adding more information.  Because of this, a security control at a higher layer cannot provide protection for lower layers, because the lower layers perform functions of which the higher layers are not aware.  Security controls that are available at each layer include:

# Cross-site request forgery (CSRF)

### Understanding the Attack

At its most fundamental level, CSRF is a vulnerability where an attacker forces a victim to make an HTTP request on the attacker’s behalf. It’s an attack that occurs entirely on the client’s side (e.g. web browser) where countermeasures trust that the victim is sending an application information that can be trusted. There are three components that enable the attack to occur: Improper usage of unsafe HTTP verbs, web browser cookie handling, and Cross-Site Scripting (XSS).

The HTTP specification separates verbs into two categories safe and unsafe. Safe verbs (GET, HEAD, and OPTIONS) are intended for use in read only operations. Requests that use them are designed to return information about the resource that is requested and should have no side effects on the server. Unsafe verbs (POST, PUT, PATCH, and DELETE) are intended for modification, creation, or deletion of a resource. Unfortunately, it’s possible for an HTTP verb to be ignored or it’s intended behavior to not be strictly enforced.

A major reason for incorrect verb use is due to the historically poor browser support of the HTTP specification. Up until the prevalence of XML HTTP Request (XHR), it was simply not possible to use a verb other than GET or POST without relying on specific framework and library hacks. This limitation resulted in the practical irrelevance of distinguishing between HTTP verbs. This alone is not sufficient to create the condition of CSRF but it contributes and makes protection against it significantly more difficult. The biggest factor that contributes to the vulnerability of CSRF is the handling of cookies by the browser.

HTTP was originally designed to be a stateless protocol with a single request corresponding to a single response and no state carried between requests. To support complex web applications, cookies were created as a solution to keep state between related HTTP requests. Cookies reside at the global level of the browser and are shared across instances, windows, and tabs. Users depend on web browsers to automatically transmit cookies for every request. Since cookies are accessible/modifiable in the browser and lack anti-tamper protection, the storage of state has shifted to server managed sessions. In this model, a unique identifier is generated on the server and stored in the cookie. Each browser request sends the cookie with it and the server is able to look up the identifier to determine if it is a valid session. When the session ends, the server forgets the identifier and invalidates any further requests that send it.

The issue lies in how cookies are managed by browsers. A cookie is composed of a handful of attributes, but the most important one we care about here is the Domain attribute. The intended functionality of the Domain attribute is to scope the cookie to a specific host that match the domain attribute of the cookie. This was designed as a security mechanism to avoid sensitive information, such as session identifiers, from being stolen by malicious websites where attackers could perform session fixation attacks. The flaw here is that the domain attribute does not depend on the Same Origin Policy (SOP); it simply compares the domain value of the cookie to the host in the request. This allows requests originating from a different origin to also carry along with it any cookies for that host. This is safe behavior if and only if safe and unsafe verbs are used correctly; example: a safe request (GET) should never change state but as we’ve seen correct use cannot necessarily be trusted. If you don’t know about the SOP, then you should read more about it: https://en.wikipedia.org/wiki/Same-origin_policy.

The last component to focus on is Cross-Site Scripting (XSS). XSS is the ability for attacker controlled JavaScript or HTML to be rendered in the DOM by a victim. If XSS exists in the application it’s essentially game over when it comes to stopping CSRF attacks. The main countermeasure that we’re going to discuss in this post, and most applications depend on, can be bypassed if XSS is possible.

# Clickjacking

### What is clickjacking

Clickjacking is an attack that tricks a user into clicking a webpage element which is invisible or disguised as another element. This can cause users to unwittingly download malware, visit malicious web pages, provide credentials or sensitive information, transfer money, or purchase products online.

Typically, clickjacking is performed by displaying an invisible page or HTML element, inside an iframe, on top of the page the user sees. The user believes they are clicking the visible page but in fact they are clicking an invisible element in the additional page transposed on top of it.

The invisible page could be a malicious page, or a legitimate page the user did not intend to visit – for example, a page on the user’s banking site that authorizes the transfer of money.

There are several variations of the clickjacking attack, such as:

- Likejacking – a technique in which the Facebook “Like” button is manipulated, causing users to “like” a page they actually did not intend to like.

- Cursorjacking – a UI redressing technique that changes the cursor for the position the user perceives to another position. Cursorjacking relies on vulnerabilities in Flash and the Firefox browser, which have now been fixed.

###  Clickjacking attack example

1. The attacker creates an attractive page which promises to give the user a free trip to Tahiti.

2. In the background the attacker checks if the user is logged into his banking site and if so, loads the screen that enables transfer of funds, using query parameters to insert the attacker’s bank details into the form.

3. The bank transfer page is displayed in an invisible iframe above the free gift page, with the “Confirm Transfer” button exactly aligned over the “Receive Gift” button visible to the user.

4. The user visits the page and clicks the “Book My Free Trip” button.

5. In reality the user is clicking on the invisible iframe, and has clicked the “Confirm Transfer” button. Funds are transferred to the attacker.

6. The user is redirected to a page with information about the free gift (not knowing what happened in the background).

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/Clickjacking.png)

This example illustrates that, in a clickjacking attack, the malicious action (on the bank website, in this case) cannot be traced back to the attacker because the user performed it while being legitimately signed into their own account.

### Clickjacking mitigation

There are two general ways to defend against clickjacking:

- Client-side methods – the most common is called Frame Busting. Client-side methods can be effective in some cases, but are considered not to be a best practice, because they can be easily bypassed.

- Server-side methods – the most common is X-Frame-Options. Server-side methods are recommended by security experts as an effective way to defend against clickjacking.

#### Using the SAMEORIGIN option to defend against clickjacking

X-Frame-Options allows content publishers to prevent their own content from being used in an invisible frame by attackers.

The DENY option is the most secure, preventing any use of the current page in a frame. More commonly, SAMEORIGIN is used, as it does enable the use of frames, but limits them to the current domain.

# Server-side request forgery (SSRF)

### What is SSRF?

Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing.

In typical SSRF examples, the attacker might cause the server to make a connection back to itself, or to other web-based services within the organization's infrastructure, or to external third-party systems. 

### What is the impact of SSRF attacks?

A successful SSRF attack can often result in unauthorized actions or access to data within the organization, either in the vulnerable application itself or on other back-end systems that the application can communicate with. In some situations, the SSRF vulnerability might allow an attacker to perform arbitrary command execution.

An SSRF exploit that causes connections to external third-party systems might result in malicious onward attacks that appear to originate from the organization hosting the vulnerable application, leading to potential legal liabilities and reputational damage. 

### Common SSRF attacks

SSRF attacks often exploit trust relationships to escalate an attack from the vulnerable application and perform unauthorized actions. These trust relationships might exist in relation to the server itself, or in relation to other back-end systems within the same organization. 

### SSRF attacks against the server itself

In an SSRF attack against the server itself, the attacker induces the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface. This will typically involve supplying a URL with a hostname like ```127.0.0.1``` (a reserved IP address that points to the loopback adapter) or ```localhost``` (a commonly used name for the same adapter).

For example, consider a shopping application that lets the user view whether an item is in stock in a particular store. To provide the stock information, the application must query various back-end REST APIs, dependent on the product and store in question. The function is implemented by passing the URL to the relevant back-end API endpoint via a front-end HTTP request. So when a user views the stock status for an item, their browser makes a request like this: 

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1 
```

This causes the server to make a request to the specified URL, retrieve the stock status, and return this to the user.

In this situation, an attacker can modify the request to specify a URL local to the server itself. For example: 

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin 
```

Here, the server will fetch the contents of the ```/admin``` URL and return it to the user.

Now of course, the attacker could just visit the ```/admin``` URL directly. But the administrative functionality is ordinarily accessible only to suitable authenticated users. So an attacker who simply visits the URL directly won't see anything of interest. However, when the request to the ```/admin``` URL comes from the local machine itself, the normal access controls are bypassed. The application grants full access to the administrative functionality, because the request appears to originate from a trusted location. 

Why do applications behave in this way, and implicitly trust requests that come from the local machine? This can arise for various reasons:

- The access control check might be implemented in a different component that sits in front of the application server. When a connection is made back to the server itself, the check is bypassed.

- For disaster recovery purposes, the application might allow administrative access without logging in, to any user coming from the local machine. This provides a way for an administrator to recover the system in the event they lose their credentials. The assumption here is that only a fully trusted user would be coming directly from the server itself.

- The administrative interface might be listening on a different port number than the main application, and so might not be reachable directly by users.

These kind of trust relationships, where requests originating from the local machine are handled differently than ordinary requests, is often what makes SSRF into a critical vulnerability. 
