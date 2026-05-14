# Szimmetrikus kulcsú titkosítás, AES
## Fájl titkosítása AES-128-ECB módban
```batch
openssl aes-128-ecb -e -in plain.txt -out encrypted.bin
```
## Visszafejtés
```batch
openssl aes-128-ecb -d -in encrypted.bin -out decrypted.txt
```
## AES-128-CBC titkosítás
```batch
openssl aes-128-cbc -e -in plain.txt -out encrypted.bin
```
## AES-128-CBC visszafejtés
```batch
openssl aes-128-cbc -d -in encrypted.bin -out decrypted.txt
```
## AES-256-CBC használata
```batch
openssl aes-256-cbc -e -in plain.txt -out encrypted.bin
```
## AES-192-CBC használata
```batch
openssl aes-192-cbc -e -in plain.txt -out encrypted.bin
```
## Kulcs és IV manuális megadása
```batch
openssl aes-128-cbc -e -in plain.txt -out encrypted.bin -K 00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
```
## Manuális visszafejtés
```batch
openssl aes-128-cbc -d -in encrypted.bin -out decrypted.txt -K 00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
```
## Só használata
```batch
openssl aes-256-cbc -salt -e -in plain.txt -out encrypted.bin
```
## Só nélküli titkosítás
```batch
openssl aes-256-cbc -nosalt -e -in plain.txt -out encrypted.bin
```
## Titkosított fájl fejlécének vizsgálata
```batch
hexdump -C encrypted.bin | head
```
## Véletlen kulcs generálása
### 16 bájt
```batch
openssl rand -hex 16
```
### 32 bájt
```batch
openssl rand -hex 32
```
## Véletlen IV generálása
```batch
openssl rand -hex 16
```
## Hash készítése
### SHA-256
```batch
openssl dgst -sha256 plain.txt
```
## Base64 kimenet
```batch
openssl aes-256-cbc -a -e -in plain.txt -out encrypted.txt
```
## Base64 visszafejtés
```batch
openssl aes-256-cbc -a -d -in encrypted.txt -out decrypted.txt
```
## Kulcs és IV megjelenítése
```batch
openssl aes-256-cbc -P
```
## Jelszó alapú kulcsszármaztatás
```batch
openssl aes-256-cbc -pbkdf2 -e -in plain.txt -out encrypted.bin
```
## PBKDF2 visszafejtés
```batch
openssl aes-256-cbc -pbkdf2 -d -in encrypted.bin -out decrypted.txt
```