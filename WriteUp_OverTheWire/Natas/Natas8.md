username : natas8
password :  xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q


- On arrive on a un formulaire, on a un code php dans le code source :
```php  
<?      $encodedSecret = "3d3d516343746d4d6d6c315669563362";      function encodeSecret($secret) {       return bin2hex(strrev(base64_encode($secret)));   }      if(array_key_exists("submit", $_POST)) {       if(encodeSecret($_POST['secret']) == $encodedSecret) {       print "Access granted. The password for natas9 is <censored>";       } else {       print "Wrong secret";       }   }   ?>
```
- On comprend qu'il faut decouper le code pour decoder le secret 

```php 
<?php $encodedSecret = "3d3d516343746d4d6d6c315669563362"; 
$step1 = hex2bin($encodedSecret); 
$step2 = strrev($step1); 
$originalSecret = base64_decode($step2); 
echo $originalSecret; ?> ```

- On execute le code et on obtients le mot de passe : **ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t**

