# Tűzfalak
## Tűzfalszabályok megtekintése
### iptables szabályok listázása
```bash
sudo iptables -L
```
### Részletes lista
```bash
sudo iptables -L -v -n
```
## Port megnyitása
SSH (22/TCP)
```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
## Port tiltása
HTTP (80/TCP)
```bash
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```
### IP cím tiltása
```bash
sudo iptables -A INPUT -s 192.168.1.100 -j DROP
```
### Minden bejövő kapcsolat tiltása
```bash
sudo iptables -P INPUT DROP
```
### Meglévő kapcsolatok engedélyezése (stateful)
```bash
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED 
-j ACCEPT
```
### Új SSH kapcsolatok engedélyezése
```bash
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate 
NEW -j ACCEPT
```
### Loopback interfész engedélyezése
```bash
sudo iptables -A INPUT -i lo -j ACCEPT
```
### Szabály törlése
```bash
sudo iptables -D INPUT 1
```
### Szabályok mentése
#### Debian/Ubuntu
```bash
sudo iptables-save > rules.v4
```
## UFW használata
### UFW engedélyezése
```bash
sudo ufw enable
```
### Állapot lekérdezése
```bash
sudo ufw status
```
### SSH engedélyezése UFW-vel
```bash
sudo ufw allow 22/tcp
```
### HTTP engedélyezése
```bash
sudo ufw allow 80/tcp
```
### Port tiltása
```bash
sudo ufw deny 23/tcp
```
### Szabály törlése
```bash
sudo ufw delete allow 80/tcp
```
### UFW kikapcsolása
```bash
sudo ufw disable
```
## Firewalld használata
### Állapot
```bash
sudo firewall-cmd --state
```
### Szolgáltatás engedélyezése
#### SSH
```bash
sudo firewall-cmd --permanent --add-service=ssh
```
#### Port megnyitása
```bash
sudo firewall-cmd --permanent --add-port=8080/tcp
```
#### Konfiguráció újratöltése
```bash
sudo firewall-cmd --reload
```
#### Aktív szabályok listázása
```bash
sudo firewall-cmd --list-all
```