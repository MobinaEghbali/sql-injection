```html
<script>alert(1)</script>
```
## types of XSS attacks
|                                            - **REFLECTED XSS**                                            |                                                 STORED XSS<br>                                                  |                                                 DOM XSS<br>                                                 |
| :-------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------: |
| Attacker Input<br>↓<br>HTTP Request<br>↓<br>Server<br>↓<br>Immediate Response<br>↓<br>Browser<br>↓<br>XSS | Attacker Input<br>↓<br>Server<br>↓<br>Database<br> ↓<br>   Later Response   <br>↓<br>Victim Browser<br>↓<br>XSS | Attacker Input<br>↓<br>Source<br>↓<br>Client-side JavaScript<br>↓<br>Dangerous Sink<br>↓<br>DOM<br>↓<br>XSS |
## Where?
Search
Comment
Username
Profile
URL parameters
Forms
Headers

valid header -->block --->https://portswigger.net/web-security/cross-site-scripting/cheat-sheet


```url
page=homevoorivex<details open ontoggle=alert(origin)>
```