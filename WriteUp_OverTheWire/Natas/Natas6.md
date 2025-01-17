username : natas6
password:  0RoJwHdSKWFTYR5WuiAewauSuNaBXned

- On a un formulaire et du code php 
```php 
<?      include "includes/secret.inc";          if(array_key_exists("submit", $_POST)) {           if($secret == $_POST['secret']) {           print "Access granted. The password for natas7 is <censored>";       } else {           print "Wrong secret";       }       }   ?>
```

- On va donc chercher le fichier /includes/secret.inc
- On analyse le code source : `<? $secret = "FOEIUWGHFEEUHOFUOIU"; ?>`

-> Mot de passe  : **bmg8SvU1LizuWjx3y7xkNERkHxGre0GS**