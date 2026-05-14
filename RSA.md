# Aszimmetrikus eljárások, RSA
## RSA privát kulcs generálása
### 2048 bites kulcs
```batch
openssl genrsa -out private.pem 2048
```
## Publikus kulcs előállítása
```batch
openssl rsa -in private.pem -pubout -out public.pem
```
## Privát kulcs megtekintése
```batch
cat private.pem
```
## Publikus kulcs megtekintése
```batch
cat public.pem
```
## Fájl titkosítása publikus kulccsal
```batch
openssl rsautl -encrypt -pubin -inkey public.pem -in plain.txt -out encrypted.bin
```
## Visszafejtés privát kulccsal
```batch
openssl rsautl -decrypt -inkey private.pem -in encrypted.bin -out decrypted.txt
```
## SHA-256 hash készítése
```batch
openssl dgst -sha256 plain.txt
```
## Digitális aláírás készítése
```batch
openssl dgst -sha256 -sign private.pem -out signature.bin plain.txt
```
## Aláírás ellenőrzése
```batch
openssl dgst -sha256 -verify public.pem -signature signature.bin plain.txt
```
## Publikus kulcs adatai
```batch
openssl rsa -pubin -in public.pem -text -noout
```
## Privát kulcs adatai
```batch
openssl rsa -in private.pem -text -noout
```
## Titkosított privát kulcs generálása
```batch
openssl genrsa -aes256 -out private.pem 2048
```
## PEM → DER konvertálás
### Publikus kulcs
```batch
openssl rsa -pubin -in public.pem -outform DER -out public.der
```
### Privát kulcs
```batch
openssl rsa -in private.pem -outform DER -out private.der
```