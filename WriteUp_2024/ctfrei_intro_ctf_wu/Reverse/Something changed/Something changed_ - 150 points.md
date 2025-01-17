
---

# Description du challenge : 

## Something changed?

### 150

Conçu par Oblivios

Omg... du Python ?!

[http://files.ctfrei.fr/keygenme1](http://files.ctfrei.fr/keygenme1)


---

> J'installe le fichier on a pas d'extension 

> Je sais que c'est du code python compiler

# Début  : 

> J'installe [pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor) pour extraire le code 

```bash
[Oct 03, 2024 - 15:57:17 (CEST)] exegol-EthicalHacking /workspace # python3 pyinstxtractor.py keygenme1
```

> J'inspecte le résultat : 

```bash
[Oct 03, 2024 - 15:58:12 (CEST)] exegol-EthicalHacking keygenme1_extracted # ls
base_library.zip  lib-dynload           libz.so.1                pyimod03_ctypes.pyc
keygenme1.pyc     liblzma.so.5          pyiboot01_bootstrap.pyc  PYZ-00.pyz
libbz2.so.1.0     libpython3.7m.so.1.0  pyimod01_archive.pyc     PYZ-00.pyz_extracted
libcrypto.so.3    libssl.so.3           pyimod02_importers.pyc   struct.pyc
```

> Le fichier keygenme1.pyc m'interesse je le décompile avec uncompyle6

```bash
[Oct 03, 2024 - 15:59:20 (CEST)] exegol-EthicalHacking keygenme1_extracted # uncompyle6 keygenme1.pyc > keygenme1.py
```

> J'ai ce script maintenant donc le keygenme1.py :


```bash
[Oct 03, 2024 - 15:59:27 (CEST)] exegol-EthicalHacking keygenme1_extracted # cat keygenme1.py
# uncompyle6 version 3.9.2
# Python bytecode version base 3.7.0 (3394)
# Decompiled from: Python 3.11.9 (main, May  4 2024, 16:56:25) [GCC 12.2.0]
# Embedded file name: keygenme1.py


def xor_data(data, key):
    return "".join((chr(ord(c) ^ key) for c in data))


def generate_key(input_str):
    xor_key = 42
    if len(input_str) != 8:
        return "Mauvaise longueur de clé..."
    expected_sum = 824
    input_sum = sum((ord(c) for c in input_str))
    if input_sum == expected_sum:
        encrypted_flag = [
         105, 126, 108, 120, 111, 99, 81, 90, 83, 94,
         66, 26, 68, 117, 72, 83, 94, 25, 73, 26, 78, 25, 117,
         73, 30, 68, 117, 72, 25, 117, 66, 30, 88, 78, 117,
         94, 66, 26, 87]
        flag = "".join((chr(b ^ xor_key) for b in encrypted_flag))
        return flag
    return "Clé incorrecte :("


user_input = input("Entre la clé: ")
print(generate_key(user_input))

# okay decompiling keygenme1.pyc
```

> J'ai le flag, un scrypte python pour le recuprer : 

```python
import random

import string

  

def generate_valid_key():

while True:

key = ''.join(random.choices(string.ascii_letters + string.digits, k=8))

if sum(ord(c) for c in key) == 824:

return key

  

# Générer et afficher une clé valide

valid_key = generate_valid_key()

print(f"Clé valide générée : {valid_key}")

  

# Vérifier la clé

def check_key(key):

return len(key) == 8 and sum(ord(c) for c in key) == 824

  

print(f"Vérification de la clé : {check_key(valid_key)}")

  

# Fonction pour décrypter le flag (copiée du code original)

def decrypt_flag(encrypted_flag, xor_key):

return "".join(chr(b ^ xor_key) for b in encrypted_flag)

  

# Décrypter et afficher le flag

encrypted_flag = [105, 126, 108, 120, 111, 99, 81, 90, 83, 94, 66, 26, 68, 117, 72, 83, 94, 25, 73, 26, 78, 25, 117, 73, 30, 68, 117, 72, 25, 117, 66, 30, 88, 78, 117, 94, 66, 26, 87]

xor_key = 42

flag = decrypt_flag(encrypted_flag, xor_key)

print(f"Flag décrypté : {flag}")

```

# Résultat :

> Clé valide générée : hbjqyo9r
> Vérification de la clé : True
   Flag décrypté : CTFREI{pyth0n_byt3c0d3_c4n_b3_h4rd_th0}


