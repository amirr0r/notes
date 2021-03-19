Web applications typically have **front end** components (i.e., the website interface, or "what the user sees") that run on the client-side (browser) and other **back end** components (web application source code) that run on the server-side (back end server/databases).

Websites have static pages while web apps have dynamic content based on user interaction (it can change in real-time) .

- Static pages are also known as [Web 1.0](https://en.wikipedia.org/wiki/Web_2.0#Web_1.0)
- Web applications are known as [Web 2.0](https://en.wikipedia.org/wiki/Web_2.0#Web_2.0)

Web applications are platform-independent and can run in a browser on any operating system. They do not have to be installed on a user's system because these web applications and their functionality are executed remotely on the remote server and hence do not consume any space on the end user's hard drive.

All users accessing a web application use the same version, while users using a mobile app need to install the updated version on their own.

> `WordPress`, `OpenCart` and `Joomla` are open source  Content Management Systems (CMS) 
>  `Wix`, `Shopify` and `DotNetNuke` are closed source CMS

[OWASP Web Security Testing Guide](https://github.com/OWASP/wstg/tree/master/document/4-Web_Application_Security_Testing#web-application-security-testing) is a widely used set of methods for testing web applications.

> [password spraying attacks](https://us-cert.cisa.gov/ncas/current-activity/2019/08/08/acsc-releases-advisory-password-spraying-attacks) against a VPN or email portal.

___

Some Web attacks:

- [Broken Access Control](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control)
- [Indirect Object Reference (IDOR)](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)
- [File Inclusion](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
- [Unrestricted File Upload](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
- [SQL injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)

___

- Using `Many Servers - Many Databases` Infrastructure may require tools like [load balancers](https://en.wikipedia.org/wiki/Load_balancing_(computing))  to function appropriately

- There are web application models available such as [serverless](https://aws.amazon.com/lambda/serverless-architectures-learn-more/) web applications or web applications that utilize [microservices](https://aws.amazon.com/microservices).

> **Microservices** are independent components of the web application, which in most cases are programmed for one task only. Data is stored separately from the respective microservices They can be written in different programming languages and still interact. (Read: [AWS microservices paper](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf))

> **Serverless** is a way to build and deploy applications and services without having to manage infrastructure. Cloud providers such as AWS, GCP, Azure, among others, offer serverless architectures. (Read [Serverless on AWS](https://aws.amazon.com/serverless/))

> In many cases, an individual web application's vulnerability may not necessarily be caused by a programming error but by a design error in its architecture.

___

## Top 20 mistakes web developers do

**1.** 	Permitting Invalid Data to Enter the Database

**2.** 	Focusing on the System as a Whole

**3.** 	Establishing Personally Developed Security Methods

**4.** 	Treating Security to be Your Last Step

**5.** 	Developing Plain Text Password Storage

**6.** 	Creating Weak Passwords

**7.** 	Storing Unencrypted Data in the Database

**8.** 	Depending Excessively on the Client Side

**9.** 	Being Too Optimistic

**10.** Permitting Variables via the URL Path Name

**11.** Trusting third-party code

**12.** Hard-coding backdoor accounts

**13.** Unverified SQL injections

**14.** Remote file inclusions

**15.** Insecure data handling

**16.** Failing to encrypt data properly

**17.** Not using a secure cryptographic system

**18.** Ignoring layer 8

**19.** Review user actions

**20.** Web Application Firewall misconfigurations

## Last OWASP Top 10:

ID      | Vuln                                 | Description |
------- | ------------------------------------ | ----------- |
**1.**  | Injection                            | execute commands on the back end server ([Example](https://www.exploit-db.com/exploits/45274))|
**2.**  | Broken Authentication                | allow attackers to bypass authentication functions ([Example](https://www.exploit-db.com/exploits/47388)) |
**3.**  | Sensitive Data Exposure              | |
**4.**  | XML External Entities (XXE)          | |
**5.**  | Broken Access Control                | allow attackers to access pages and features they should not have access to|
**6.**  | Security Misconfiguration            | |
**7.**  | Cross-Site Scripting (XSS)           | injecting JavaScript code to be executed on the client-side |
**8.**  | Insecure Deserialization             | |
**9.**  | Using Components with Known Vulns    | |
**10.** | Insufficient Logging & Monitoring    | |

![Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

Tree types of XSS:

- **Reflected**: Occurs when user input is displayed on the page after processing (e.g., search result or error message).
- **Stored**: Occurs when user input is stored in the back end database and then displayed upon retrieval (e.g., posts or comments).
- **DOM**: Occurs when user input is directly shown in the browser and is written to an HTML DOM object (e.g., vulnerable username or page title).

____

[Character encoding table](https://www.w3schools.com/tags/ref_urlencode.ASP)

> The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."

____

**Sanitization**: Removing special characters and non-standard characters from user input before displaying it or storing it.

![Web stacks](https://en.wikipedia.org/wiki/Solution_stack)

___

**Successful responses**
`200 OK` 	The request has succeeded

**Redirection messages** 	
`301 Moved Permanently` 	The URL of the requested resource has been changed permanently
`302 Found` 	The URL of the requested resource has been changed temporarily

**Client error responses** 	
`400 Bad Request` 	The server could not understand the request due to invalid syntax
`401 Unauthorized` 	Unauthenticated attempt to access page
`403 Forbidden` 	The client does not have access rights to the content
`404 Not Found` 	The server can not find the requested resource
`405 Method Not Allowed` 	The request method is known by the server but has been disabled and cannot be used
`408 Request Timeout` 	This response is sent on an idle connection by some servers, even without any previous request by the client

**Server error responses** 	
`500 Internal Server Error` 	The server has encountered a situation it doesn't know how to handle
`502 Bad Gateway` 	The server, while working as a gateway to get a response needed to handle the request, received an invalid response
`504 Gateway Timeout` 	The server is acting as a gateway and cannot get a response in time


An HTTP server can return five types of response codes:

Type   | Description
------ | -------
`1xx`  | Usually provides information and continues processing the request.
`2xx`  | Positive response codes returned when a request succeeds.
`3xx`  | Returned when the server redirects the client.
`4xx`  | This class of codes signifies improper requests from the client. For example, requesting a resource that doesn't exist or requesting a bad format.
`5xx`  | Returned when there is some problem with the HTTP server itself.


___

`IIS` (Internet Information Services) is the third most common web server on the internet, hosting around **15%** of all internet web sites.

> Apache **40%** and nginx **30%**

___

## Non-relational (NoSQL)

A non-relational database does not use tables, rows, columns, primary keys, relationships, or schemas. Instead, a `NoSQL` database stores data using various storage models, depending on the type of data stored.

Due to the lack of a defined structure for the database, `NoSQL` databases are very scalable and flexible. When dealing with datasets that are not very well defined and structured, a `NoSQL` database would be the best choice for storing our data.

There are 4 common storage models for `NoSQL` databases:

- Key-Value
- Document-Based
- Wide-Column
- Graph

Each of the above models has a different way of storing data. For example, the `Key-Value` model usually stores data in `JSON` or `XML`, and has a key for each pair, storing all of its data as its value.

Example:

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
\
```


Type                | Description
------------------- | -----------
`MongoDB`           | The most common NoSQL database. It is free and open-source, uses the Document-Based model, and stores data in JSON objects
`ElasticSearch`     | Another free and open-source NoSQL database. It is optimized for storing and analyzing huge datasets. As its name suggests, searching for data within this database is very fast and efficient
`Apache Cassandra`  | Also free and open-source. It is very scalable and is optimized for gracefully handling faulty values

> Plus `Redis`, `Neo4j`, `CouchDB`, and `Amazon DynamoDB`

___

## APIs

- `SOAP` ([Simple Objects Access](https://en.wikipedia.org/wiki/SOAP)) standard shares data through XML, where the request is made in XML through an HTTP request, and the response is also returned in XML. 

- `REST` ([Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer))  standard shares data through the URL path 'i.e. `search/users/1`', and usually returns the output in JSON format 'i.e. userid `1`'

`REST` uses various `HTTP` methods to perform different actions on the web application:

- `GET` request to retrieve data
- `POST` request to create data
- `PUT` request to change existing data
- `DELETE` request to remove data

___

The Common Vulnerability Scoring System (`CVSS`) is an open-source industry standard for assessing the severity of security vulnerabilities. This scoring system is often used as a standard measurement for organizations and governments that need to produce accurate and consistent severity scores for their systems' vulnerabilities.

[CVSS Scoring System](https://www.first.org/cvss/user-guide)

___

[HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

![OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)

>  URL's should be kept to below 2,000 characters.

___

> The `OPTIONS` method is useful to enumerate the list of methods allowed by the server.
> The `PUT` method can be used to overwrite any existing file or create a new one. This method can be used to perform other dangerous operations such as uploading web shells and executing code on the server.