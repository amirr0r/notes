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

Top 20 mistakes web developers do:

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

Last OWASP Top 10:

**1.** 	Injection

**2.** 	Broken Authentication

**3.** 	Sensitive Data Exposure

**4.** 	XML External Entities (XXE)

**5.** 	Broken Access Control

**6.** 	Security Misconfiguration

**7.** 	Cross-Site Scripting (XSS)

**8.** 	Insecure Deserialization

**9.** 	Using Components with Known Vulnerabilities

**10.** Insufficient Logging & Monitoring

____

[Character encoding table](https://www.w3schools.com/tags/ref_urlencode.ASP)

> The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."