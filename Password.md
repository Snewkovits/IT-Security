# Linux jelszókezelés
`/etc/passwd` felépítése

A passwd fájl sorai `:` karakterrel elválasztott mezőkből állnak.

Példa:
```
adam:x:1000:1000:Adam Kovacs:/home/adam:/bin/bash
```
Mezők jelentése:

| Mező	       | Jelentés |
| -- | -- |
| adam	       | felhasználónév |
| x	           | jelszó helye (x → shadow fájlban van) |
| 1000	       | UID |
| 1000	       | elsődleges GID |
| Adam Kovacs  |megjegyzés (GECOS) |
| /home/adam   |home könyvtár |
| /bin/bash	   | shell |
| /etc/group   | felépítése |

Példa:
```
developers:x:1001:adam,bela
```
Mezők:

|Mező	    |Jelentés|
|--|--|
|developers	|csoport neve|
|x	        |csoportjelszó helye|
|1001	    |GID|
|adam,bela	|supplementary tagok|
|/etc/shadow |felépítése|

Példa:
```
adam:$6$abc123$.....:20220:7:90:14:5:20300:
```
Mezők:

|Pozíció	|Jelentés|
|--|--|
|1	    |felhasználónév|
|2	    |jelszó hash|
|3	    |utolsó jelszócsere|
|4	    |minimum nap|
|5	    |maximum nap|
|6	    |warning periódus|
|7	    |inactivity|
|8	    |account expiry|
|9	    |fenntartott|

## Felhasználó lekérdezési parancsok
### Aktuális felhasználó
```
whoami
```
UID és csoportok
```
id
```
Példa kimenet:
```
uid=1000(adam) gid=1000(adam) groups=1000(adam),27(sudo)
```
### Teljes passwd fájl
```bash
cat /etc/passwd
```
### Egy adott felhasználó
```bash
grep "^adam:" /etc/passwd
```
### Shadow információ
```bash
sudo grep "^adam:" /etc/shadow
```
## Felhasználó létrehozás
### Egyszerű felhasználó
```bash
sudo useradd adam
```
### Home könyvtárral
```bash
sudo useradd -m adam
```
### Bash shell megadása
```bash
sudo useradd -m -s /bin/bash adam
```
### Megjegyzés hozzáadása
```bash
sudo useradd -m -c "Adam Kovacs" adam
```
### Jelszó beállítása
```bash
sudo passwd adam
```
## Felhasználó módosítás
### Shell módosítása
```bash
sudo usermod -s /bin/zsh adam
```
### Home könyvtár módosítása
```bash
sudo usermod -d /home/newadam adam
```
### Csoport hozzáadása
```bash
sudo usermod -aG sudo adam
```
* `-a` → append
* `-G` → supplementary group
### Felhasználó zárolása
```bash
sudo usermod -L adam
```
### Felhasználó feloldása
```bash
sudo usermod -U adam
```
## Jelszókor kezelés (chage)
### Jelenlegi beállítások
```bash
sudo chage -l adam
```

Példa:

```
Last password change                                    : May 14, 2026
Password expires                                        : Aug 12, 2026
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 7
Maximum number of days between password change          : 90
Number of days of warning before password expires       : 14
```
### Minimum jelszócsere idő
7 nap minimum
```bash
sudo chage -m 7 adam
```
### Maximum jelszóéletkor
90 nap
```bash
sudo chage -M 90 adam
```
### Figyelmeztetési periódus
14 nappal lejárat előtt
```bash
sudo chage -W 14 adam
```
### Inaktivitási idő
5 nap türelmi idő
```bash
sudo chage -I 5 adam
```

Jelentése:

* lejárt jelszó után még 5 napig lehet belépni jelszócserére.
## Account lejárat
### Konkrét dátum
```bash
sudo chage -E 2026-12-31 adam
```
### Lejárat törlése
```bash
sudo chage -E -1 adam
```
## Példa teljes policy-re
### Biztonságos vállalati policy
```bash
sudo chage -m 7 -M 90 -W 14 -I 5 adam
```

Jelentés:

* minimum 7 napig nem cserélhető
* 90 nap után lejár
* 14 nappal előtte warning
* 5 nap grace period
## Technikai felhasználó létrehozása
### Interaktív login tiltása
```bash
sudo useradd -r -s /usr/sbin/nologin serviceuser
```
Vagy:
```bash
sudo useradd -r -s /bin/false serviceuser
```
## Fiók lejárat demonstráció
### Holnapi lejárat
```bash
sudo chage -E $(date -d tomorrow +%F) adam
```
## Shadow hash algoritmusok

Példa:

```
$6$salt$hash
```
Jelentés:

|Prefix	|Algoritmus|
|--|--|
|$1$	    |MD5|
|$5$	    |SHA-256|
|$6$	    |SHA-512|

## Felhasználó törlés
### Csak user
```bash
sudo userdel adam
```
### User + home törlés
```bash
sudo userdel -r adam
```
### Supplementary csoportok listázása
```bash
groups adam
```
### Bejelentkezési shell ellenőrzése
```bash
getent passwd adam
```
### Login tiltása shell cserével
```bash
sudo usermod -s /usr/sbin/nologin adam
```

Ezután:

* SSH login nem működik
* interaktív shell nem indul
## Fontos rendszerfelhasználók
|Felhasználó	|Szerep|
|--|--|
|root	    |superuser|
|daemon	    |háttérfolyamatok|
|www-data	|webszerver|
|nobody	    |minimális jogosultságú user|

A `root` UID-je mindig:

```
0
```

Ez ad teljes rendszerszintű jogosultságot.