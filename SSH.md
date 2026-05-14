# SSH és kulcsalapú hitelesítés
### SSH kapcsolat létrehozása
```bash
ssh user@192.168.1.10
```
## SSH kulcspár generálása
### ED25519 kulcs
```bash
ssh-keygen -t ed25519
```
### Publikus kulcs megtekintése
```bash
cat ~/.ssh/id_ed25519.pub
```
### Publikus kulcs másolása szerverre
```bash
ssh-copy-id user@192.168.1.10
```
## Manuális authorized_keys használat
### .ssh könyvtár létrehozása
```bash
mkdir -p ~/.ssh
```
### Jogosultságok
```bash
chmod 700 ~/.ssh
```
### Publikus kulcs hozzáadása
```bash
cat id_ed25519.pub >> ~/.ssh/authorized_keys
```
### authorized_keys jogosultság
```bash
chmod 600 ~/.ssh/authorized_keys
```
### Known hosts megtekintése
```bash
cat ~/.ssh/known_hosts
```
### SSH szerver konfiguráció
```bash
sudo nano /etc/ssh/sshd_config
```
### Root login tiltása
```bash
PermitRootLogin no
```
### Jelszavas autentikáció tiltása
```bash
PasswordAuthentication no
```
### SSH szolgáltatás újraindítása
```bash
sudo systemctl restart ssh
```