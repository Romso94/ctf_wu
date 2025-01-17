
---

# Description du challenge : 
## Le pivot

### 150

Conçu par calygael

Un pivot est une donnée que l'on exploite pour passer d'une information A à une information B. Exemple : Utiliser une adresse mail pour retrouver un compte inscrit

J'ai découvert ce compte [Gravatar](https://gravatar.com/atomicprince3790861275) et j'aimerais pivoter à l'aide de celui-ci.

Aide-moi à retrouver son plat préféré ! Format : `CTFREI{ice_cream}`

[https://gravatar.com/atomicprince3790861275](https://gravatar.com/atomicprince3790861275)

---

## Première Etape 

J'ai cliqué sur le lien et je suis tombé sur ce profil : 

![[Pasted image 20240930233042.png]]

# Trouver l'email 

> J'ai un pseudo donc je lance sherlock :

```powershell
PS C:\Users\Romai> python3 -m sherlock_project Maxou94306
[*] Checking username Maxou94306 on:

[+] 8tracks: https://8tracks.com/Maxou94306
[+] Amino: https://aminoapps.com/u/Maxou94306
[+] Fiverr: https://www.fiverr.com/Maxou94306
[+] GitHub: https://www.github.com/Maxou94306
[+] HackTheBox: https://forum.hackthebox.eu/profile/Maxou94306
[+] HackenProof (Hackers): https://hackenproof.com/hackers/Maxou94306
[+] HudsonRock: https://cavalier.hudsonrock.com/api/json/v2/osint-tools/search-by-username?username=Maxou94306
[+] Kick: https://kick.com/Maxou94306
[+] LibraryThing: https://www.librarything.com/profile/Maxou94306
[+] Lichess: https://lichess.org/@/Maxou94306
[+] NitroType: https://www.nitrotype.com/racer/Maxou94306
[+] ProductHunt: https://www.producthunt.com/@Maxou94306
[+] TLDR Legal: https://tldrlegal.com/users/Maxou94306/

[*] Search completed with 13 results
```

> Je cherche son email donc je vais sur son profil [Github](https://github.com/Maxou94/Maxou94306/ )
> Il y a un fichier README.md avec 2 commits
> Je vais sur le commit et je rajoute  [.patch a l'url](https://github.com/Maxou94/Maxou94306/commit/b3d85d3f41a379e94ad3af480e68187e9eac2c39.patch) 

![[Pasted image 20240930233505.png]]
> Son compte est mal configuré j'ai accès a son mail 

# Epios 

J'ai son mail je vais donc sur epios pour trouver des informations supplémentaires 

> Apres une recherche j'ai des informations sur : 
> - [Google map](https://www.google.com/maps/contrib/115416143820529650554)
> - [Google Calendar](https://calendar.google.com/calendar/u/0/embed?src=maxou94306@gmail.com)

> En cliquant sur le google calendar on a une info : 

![[Pasted image 20240930233702.png]]

le flag : **CTFREI{avocado_toast}**


