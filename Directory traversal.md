# Directory Traversal

Directory traversal (also known as file path traversal) is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application.

## IDENTIFYING THE VULNERABILITY
```
1 Check wether the Parameter that u identified  is fetching the results from a systems disk location or not.
2 Generally try to enumerate endpoints having query strings(?). 
3 Check wether the system commands are being executed on the Endpoint or not. 

```
## Examples
```
> https://insecure-website.com/loadImage?filename=../../../etc/passwd.
Above **?filename** is the parameter that has the vuln and **../../../etc/passwd** is the system command that we used to check wether the application is executing the arbitary code that the user has supplied.
This causes the application to read from the following file path:
/var/www/images/../../../etc/passwd.

```
## Common obstacles to exploiting file path traversal vulnerabilities
- If the usual ../../../ doesnt work then u may use nested traversal techniques like ....//....//....//etc/passwd
- If the backend is using some kind of Filtering then u can use various non-standard encodings like ..%c0%af or ..%252f 
   - ..%252f..%252f..%252fetc/passwd 
- If an application requires that the user-supplied filename must start with the expected base folder, such as /var/www/images, then it might be possible to include the required base folder followed by suitable traversal sequences. 
  - For Example: filename=/var/www/images/../../../etc/passwd 
- If an application requires that the user-supplied filename must end with an expected file extension, such as .png, then it might be possible to use a null byte to effectively terminate the file path before the required extension. 
  - For example: filename=../../../etc/passwd%00.png
```

Learn More 
1. https://portswigger.net/web-security/file-path-traversal
2. https://hackerone.com/reports/229622
3. https://hackerone.com/reports/311216
