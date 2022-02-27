# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

## Exposed Services
Nmap scan results for each machine reveal the below services and OS details:  
~~~
nmap -sV 192.168.1  
~~~

[picture]

This scan identifies the services below as potential points of entry:  
  
Target 1  
OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)  
Apache httpd 2.4.10 ((Debian))  
The following vulnerabilities were identified on each target:  
- Target 1
- Weak server password
- Unsalted hashed passwords
- Unpatched WordPress software
  
 ## Exploitation
The Red Team was able to penetrate Target 1 and retrieve the following confidential data:


**flag1{b9bbcb33e11b80be759c4e844862482d}**
Tools used: dirb, wpscan  
- Using dirb we were able to find a wordpress page: 192.168.1.110/wordpress  
- From there we ran wpscan: 
~~~
wpscan –url http://192.168.1.110/wordpress
~~~
[picture]

-- We then found two login names: Steven and Michael  
[picture]  
- By sheer luck, we guessed the password without brute force:  
- ssh michael@192.168.1.110
- Password:  michael
-  Commands:
~~~
cd ../../var/www/html
grep -RE flag1
~~~

**flag2{fc3fd58dcdad9ab23faca6e9a36e581c}**
- Follow same steps as flag 1
~~~ 
cd ../../var/www/
grep -RE flag2
~~~


Flag 3,4 from MySQL:
Tools used: SSH, MySQL
ssh michael@192.168.1.110
Password: michael
cd /var/www/html/wordpress


