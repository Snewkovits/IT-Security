# Tanúsítványok kezelése

## Privát kulcs generálása
```batch
openssl genrsa -out private.pem 2048
```
## Publikus kulcs előállítása
```batch
openssl rsa -in private.pem -pubout -out public.pem
```
## CSR (Certificate Signing Request) készítése
```batch
openssl req -new -key private.pem -out request.csr
```
## CSR tartalmának megtekintése
```batch
openssl req -in request.csr -text -noout
```
## Önaláírt tanúsítvány készítése
```batch
openssl req -x509 -new -key private.pem -out certificate.crt -days 365
```
## Tanúsítvány megtekintése
```batch
openssl x509 -in certificate.crt -text -noout
```
## Tanúsítvány érvényességének ellenőrzése
```batch
openssl x509 -in certificate.crt -noout -dates
```
## Tanúsítvány aláírása CA-val
```batch
openssl x509 -req -in request.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out certificate.crt -days 365
```
## Tanúsítvány ellenőrzése CA segítségével
```batch
openssl verify -CAfile ca.crt certificate.crt
```
## Tanúsítvány fingerprint
```batch
openssl x509 -in certificate.crt -fingerprint -sha256 -noout
```
## CSR aláírás ellenőrzése
```batch
openssl req -in request.csr -verify -noout
```
## Tanúsítvány és kulcs egyezőségének ellenőrzése
### Tanúsítvány modulus
```batch
openssl x509 -noout -modulus -in certificate.crt
```
### Privát kulcs modulus
```batch
openssl rsa -noout -modulus -in private.pem
```
## CRL létrehozása
```batch
openssl ca -gencrl -out crl.pem
```
## CRL megtekintése
```batch
openssl crl -in crl.pem -text -noout
```
## OCSP lekérdezés
```batch
openssl ocsp -issuer ca.crt -cert certificate.crt -url http://ocsp.server
```