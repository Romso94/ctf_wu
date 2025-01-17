username : natas11
password : UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk



- On arrive sur un formulaire qui contient du code php :

```php 
<? $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");
function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}
function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}
function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
$data = loadData($defaultdata);
if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}
saveData($data);?>
<? if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}?>
```
- On comprend rapidement l'objectif de ce code : Il recupere le cookie le décode grace au XOR et verifie si  `$data["showpassword"] == "yes"`. Il faut donc trouver le cookie qui une fois un XOR appliqué donne ce résultat.
- On sait que le XOR est reversible : 
		- Propriété du XOR :  
	    Si A ⊕ B = C, alors C ⊕ A = B (où ⊕ représente l'opération XOR)
- Dans notre cas :
    - Texte en clair ⊕ Clé = Texte chiffré
    - Donc : Texte chiffré ⊕ Texte en clair = Clé

On recupere donc notre cookie stocké en claire :
	 "showpassword"=>"no", "bgcolor"=>"#ffffff"
Ainsi que notre cookie chiffré : 
	"HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg="

Avec PHP on obtiens donc notre clé : 
```php 
	// Fonction pour trouver la clé XOR
function find_xor_key($plaintext, $ciphertext) {
    $key = '';
    for ($i = 0; $i < strlen($plaintext); $i++) {
        $key .= $plaintext[$i] ^ $ciphertext[$i];
    }
    return substr($key, 0, strlen($key) / 2);  // On prend la première moitié car la clé se répète
}
$key = find_xor_key($knownPlaintext, $knownCiphertext);

```

	- Clé XOR trouvée : eDWoeDWoeDWoeDWoeDWo

On a plus qu'a XOR la valeur que l'on veut envoyer : 
	{"showpassword":"yes","bgcolor":"#ffffff"}

On obtiens ce cookie : "HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5"

On le remplace dans notre navigateur et on rafraichit la page.
Well done mot de passe : **yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB**

Code php entier : 
```php 
<?php

// Données connues
$knownPlaintext = json_encode(array( "showpassword"=>"no", "bgcolor"=>"#ffffff"));
$knownCiphertext = base64_decode("HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=");

echo "Texte en clair connu : " . $knownPlaintext . "\n";
echo "Texte chiffré connu (après décodage base64) : " . bin2hex($knownCiphertext) . "\n\n";

// Fonction pour trouver la clé XOR
function find_xor_key($plaintext, $ciphertext) {
    $key = '';
    for ($i = 0; $i < strlen($plaintext); $i++) {
        $key .= $plaintext[$i] ^ $ciphertext[$i];
    }
    return substr($key, 0, strlen($key) / 2);  // On prend la première moitié car la clé se répète
}

$key = find_xor_key($knownPlaintext, $knownCiphertext);
echo "Clé XOR trouvée : " . $key . "\n\n";

// Fonction de chiffrement/déchiffrement XOR
function xor_encrypt($in, $key) {
    $text = $in;
    $outText = '';
    for($i = 0; $i < strlen($text); $i++) {
        $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }
    return $outText;
}
// Vérification : déchiffrons le texte chiffré connu avec la clé trouvée
$decrypted = xor_encrypt($knownCiphertext, $key);
echo "Vérification - Texte déchiffré : " . $decrypted . "\n";
if ($decrypted === $knownPlaintext) {
    echo "La clé est correcte ! Le déchiffrement fonctionne.\n\n";
} else {
    echo "Erreur : La clé n'est pas correcte.\n\n";
}

// Créons maintenant un nouveau cookie avec showpassword=yes
$newData = json_encode(array( "showpassword"=>"yes", "bgcolor"=>"#ffffff"));
echo "Nouvelles données à chiffrer : " . $newData . "\n";

$newEncrypted = xor_encrypt($newData, $key);
echo "Nouvelles données chiffrées : " . bin2hex($newEncrypted) . "\n";

$newCookie = base64_encode($newEncrypted);
echo "Nouveau cookie à utiliser : " . $newCookie . "\n\n";
?>
```
