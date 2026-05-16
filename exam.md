# 1. Feladat
Készítsen egy 205394 bájt méretű állományt a /root/205394.dat néven.
```bash
dd if=/dev/zero of=/root/205394.dat bs=1 count=205394
```

2. Feladat
Fejtse vissza a /root/205396.enc állomány tartalmát a 11111111111111112222222222222222 kulcs és a a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0 inicializáló vektor használatűval. a fájl aes-128-cbc algoritmussal titkosították. Az eredményt a következő állományban tárolja: /root/205396.txt.
```bash
openssl enc -d -aes-128-cbc \
-K 11111111111111112222222222222222 \
-iv a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0 \
-in /root/205396.enc \
-out /root/205396.txt
```

3. Feladat
Állítsa át a /root/205397.txt állomány jogosultságait úgy, hogy az csak olvasható legyen (bárki is férjen hozzá).
```bash
chmod 444 /root/205397.txt
```

4. Feladat
Biztosítsa, hogy a /root/205398.key állományhoz egyetlen felhasználónak se legyen hozzáférése. Távolítson el minden jogosultsággal kapcsolatos bejegyzést a fájlról!
```bash
chmod 000 /root/205398.key
```

5. Feladat
Készítsen egy új user19 nevű felhasználót és zárolja a jelszavát!
```bash
useradd user19
passwd -l user19
```

6. Feladat
Készítsen egy 1024 bites RSA privát kulcsot és mentse el PKCS8 formátumban a következő állományba: /root/205400.pem.
```bash
Példa:
openssl genpkey -algorithm RSA \
-pkeyopt rsa_keygen_bits:1024 \
-out /root/205400.pem

PKCS#1
openssl genrsa -out key.pem 2048

PKCS#8
openssl genpkey -algorithm RSA -out key.pem

SEC1
openssl ecparam -genkey -name prime256v1 -out ec.pem

DER
openssl pkey -in key.pem -outform DER -out key.der
```

7. Feladat
Távolítsa el azon állományokat a /root/d20541 katakológusból melyek nem a root felhasználó tulajdonában vannak. (Egyebet ne változtasson!)
```bash
find /root/d20541 ! -user root -type f -delete
```

8.Feladat
Állítsa át a /root/205402.txt fájl attribútumait úgy, hogy a tulajdonosa az operator felhasználó legyen. (Mást ne változtasson.)
```bash
chown operator /root/205402.txt
```

9. Feladat
Készítsen egy új /dev/sdc1 partíciót a /dev/sdc lemezen. Formázza a partíciót ext2 fájlrendszerrel és csatolja csak olvashatóként a /mnt katalógusra.
```bash
fdisk /dev/sdc

// fdisk alatt
n
p
1
(üres)
(üres)
w

mkfs.ext2 /dev/sdc1
mount -o ro /dev/sdc1 /mnt
```

10. Feladat
Írja alá a /root/205403.txt állományt az alábbi privát kulcs segítségével. Használjon SHA 256 hash függvényt. Az aláírás a következő állományba kerüljön: /root/205403.sign!

----BEGIN PRIVATE KEY-----
MIIBOAIBAAJBAIdFbS0LphK2C2S0K2MOzjcqhoAeuGpzz+mgxQMwv+fjR6X1doom
QYY6nL7IHGCkJ4yci6CJe8bWba7TLJ48E8sCAwEAAQJAK+oiE2mgtJpdAIhtPW9l
CwvHNnjCreyDJvmGfA6rfoAQx45i+dSV88cJk1RfGzLi3yLaU5ncWnSrnnbYyMRI
4QIhAPfAB26Bpd3QynH7RLfyYV4xJI+6Xrbs8aS6ovvpcQr3AiEAi8aPO/ZQW+hM
iPj2Jsg9IWjgQz7RrpyQiJ2Gl2s9FM0CIEHtuAyQM7NzfGwYkZDhz0dhjHky/0Fu
jF9pyzV+SbbBAh84yHFn6qi6raRqALn/B1nOOMzihKKqTPBqj5Qd73LhAiB73AUL
XGVOJYu/fi44I3KSrYJ+gTJWFEcTwtInHZRzlg==
-----END PRIVATE KEY-----

```bash
cat > /root/key.pem << 'EOF'
-----BEGIN PRIVATE KEY-----
MIIBOAIBAAJBAIdFbS0LphK2C2S0K2MOzjcqhoAeuGpzz+mgxQMwv+fjR6X1doom
QYY6nL7IHGCkJ4yci6CJe8bWba7TLJ48E8sCAwEAAQJAK+oiE2mgtJpdAIhtPW9l
CwvHNnjCreyDJvmGfA6rfoAQx45i+dSV88cJk1RfGzLi3yLaU5ncWnSrnnbYyMRI
4QIhAPfAB26Bpd3QynH7RLfyYV4xJI+6Xrbs8aS6ovvpcQr3AiEAi8aPO/ZQW+hM
iPj2Jsg9IWjgQz7RrpyQiJ2Gl2s9FM0CIEHtuAyQM7NzfGwYkZDhz0dhjHky/0Fu
jF9pyzV+SbbBAh84yHFn6qi6raRqALn/B1nOOMzihKKqTPBqj5Qd73LhAiB73AUL
XGVOJYu/fi44I3KSrYJ+gTJWFEcTwtInHZRzlg==
-----END PRIVATE KEY-----
EOF

openssl dgst -sha256 -sign /root/key.pem \
-out /root/205403.sign \
/root/205403.txt
```

11. Feladat
Állítsa át a user205404 felhasználói fiók beállításait úgy, hogy az 2025-01-01 dátummal járjon le.
```bash
chage -E 2025-01-01 user205404
```

12. Feladat
Állítson ki tanusítványt a /root/205406.csr állományban található tanusítványaláírási kérés alapján a /root/205404.crt tanusítványban szereplő szervezet nevében felhasználva annak /root/205405.key állományban tárolt privát kulcsát. A kiállított tanusítványt a következő állományban tárolja: /root/205406.crt.
```bash
openssl x509 -req \
-in /root/205406.csr \
-CA /root/205404.crt \
-CAkey /root/205405.key \
-CAcreateserial \
-out /root/205406.crt
```

13. Feladat
Adjon olvasási jogot az operator felhasználónak a /root/205407.txt fájlhoz ACL bejegyzés segítségével. =Más az állományban és attribútumain ne változtasson.)
```bash
setfacl -m u:operator:r /root/205407.txt
```

14. Feladat
Készítsen önaláírt tanusítványt a self.my-factory.eu (CN) részére és mentse a következő állományba: /root/self.my-factory.eu.crt.
```bash
openssl req -x509 -newkey rsa:2048 -nodes \
-keyout /root/self.my-factory.eu.key \
-out /root/self.my-factory.eu.crt \
-days 365 \
-subj "/CN=self.my-factory.eu"
```

15. Feladat
A /root/205409 fájl egy LUKS formátumú titkosított fájlrendszert tartalmaz. A jelszó ccfvk. Másolja a fájlrendszer tartalmát a következő katalógusba: /root
```bash
losetup -fP /root/205409
losetup -a
// Ha loop0 lett a device
cryptsetup luksOpen /dev/loop0 crypt205409
ccfvk
mount /dev/mapper/crypt205409 /mnt
cp -a /mnt/. /root/

// Opcionális takarítás
umount /mnt
cryptsetup luksClose crypt205409
losetup -d /dev/loop0
```
