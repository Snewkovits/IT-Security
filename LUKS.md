# Linux fájlrendszer titkosítása

### 1. Virtuális lemezfájl létrehozása
```bash
dd if=/dev/zero of=virtual_disk.img bs=1M count=100
```
* if=/dev/zero → null bájtok forrása
* of=virtual_disk.img → létrehozott lemezkép
* bs=1M count=100 → 100 MB méret
### 2. Loop device csatolása
```bash
sudo losetup /dev/loop0 virtual_disk.img
```

Ellenőrzés:

```bash
losetup -a
```

### 3. Fájlrendszer készítése
```bash
sudo mkfs.ext4 /dev/loop0
```
### 4. Csatolási pont létrehozása
```bash
sudo mkdir /mnt/testdisk
```
### 5. Fájlrendszer mountolása
```bash
sudo mount /dev/loop0 /mnt/testdisk
```
### 6. Teszt fájl létrehozása
```bash
echo "Titkos adat" | sudo tee /mnt/testdisk/secret.txt
```
### 7. A nyers lemezkép tartalmának vizsgálata
```bash
strings virtual_disk.img
```

Vagy:

```bash
hexdump -C virtual_disk.img | less
```

A „Titkos adat” szöveg látható lesz, mert nincs titkosítás.

## LUKS titkosítás demonstráció
### 1. Új virtuális lemez létrehozása
```bash
dd if=/dev/zero of=encrypted_disk.img bs=1M count=100
```
### 2. LUKS inicializálás
```bash
sudo cryptsetup luksFormat encrypted_disk.img
```

Figyelmeztetés után:

```bash
YES
```

Ezután jelszót kér.

### 3. LUKS kötet megnyitása
```bash
sudo cryptsetup open encrypted_disk.img secure_disk
```
secure_disk → mapper név
Megjelenik:
/dev/mapper/secure_disk
### 4. Fájlrendszer létrehozása a titkosított köteten
```bash
sudo mkfs.ext4 /dev/mapper/secure_disk
```
### 5. Mountolás
```bash
sudo mount /dev/mapper/secure_disk /mnt/testdisk
```
### 6. Titkos adat létrehozása
```bash
echo "Nagyon titkos adat" | sudo tee /mnt/testdisk/secret.txt
```
### 7. Leválasztás
```bash
sudo umount /mnt/testdisk
```
### 8. LUKS kötet lezárása
```bash
sudo cryptsetup close secure_disk
```
### 9. Nyers tartalom vizsgálata
```bash
strings encrypted_disk.img
```

Vagy:
```bash
hexdump -C encrypted_disk.img | less
```

A szöveg már nem lesz olvasható, mert AES titkosítással tárolódik.

## KeySlot kezelés
### Új jelszó hozzáadása
```bash
sudo cryptsetup luksAddKey encrypted_disk.img
```

Először a meglévő jelszót kéri, majd az újat.

### KeySlot információk listázása
```bash
sudo cryptsetup luksDump encrypted_disk.img
```
Láthatók az aktív KeySlotok.

### Jelszó eltávolítása
```bash
sudo cryptsetup luksRemoveKey encrypted_disk.img
```
### Automatikus feloldás /etc/crypttab használatával
Kulcsfájl létrehozása
```bash
sudo dd if=/dev/urandom of=/root/luks-key.bin bs=4096 count=1
```

Jogosultság:

```bash
sudo chmod 600 /root/luks-key.bin
```
### Kulcs hozzáadása a LUKS kötethez
```bash
sudo cryptsetup luksAddKey encrypted_disk.img /root/luks-key.bin
```
`/etc/crypttab` példa
```bash
secure_disk /path/encrypted_disk.img /root/luks-key.bin luks
```

Formátum:

```
<mapper_név> <eszköz> <kulcsfájl> <opciók>
```
### Automatikus mount /etc/fstab

Példa:

```bash
/dev/mapper/secure_disk /mnt/testdisk ext4 defaults 0 2
```
### Pendrive alapú kulcsfájl példa
#### Pendrive mountolása
```bash
sudo mount /dev/sdb1 /media/usb
```
Kulcsfájl használata
```bash
sudo cryptsetup open encrypted_disk.img secure_disk --key-file /media/usb/luks-key.bin
```

Ha a pendrive nincs csatlakoztatva:

* a kulcsfájl nem érhető el,
* a kötet nem nyitható meg.
### Hasznos ellenőrző parancsok
#### Aktív mapper eszközök
```bash
ls /dev/mapper
```
### Csatolt fájlrendszerek
```bash
mount | grep secure_disk
```
### Block eszközök listázása
```bash
lsblk
```
### LUKS státusz
```bash
sudo cryptsetup status secure_disk
```

A parancs megmutatja:

* titkosítás típusát,
* cipher algoritmust,
* kulcsméretet,
* mapper állapotát.