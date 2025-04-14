---
title: Operation Oni
draft: false
tags:
  - Forensic
  - Easy
  - SSH
  - Disk
date: 2025-04-13
---




## 🧩 Challenge Files

- Download disk image
    
- **Remote Machine:**
    `ssh -i key_file -p 53924 ctf-player@saturn.picoctf.net`
---

## 🛠️ Steps to Solve

### 1. Unzip the Disk Image


```bash
gzip -d disk.img.gz
```

---

### 2. Identify Partition Offset

Used `fdisk` to view partition layout:

```bash
fdisk -lu disk.img
```

**Output:**

```text
Disk disk.img: 230 MiB, 241172480 bytes, 471040 sectors Units: sectors of 1 * 512 = 512 bytes  Device     Boot  Start    End    Sectors   Size  Id  Type disk.img1  *     2048     206847 204800    100M  83  Linux disk.img2        206848   471039 264192    129M  83  Linux
```

> Calculated offset for `disk.img2`:  
> `206848 * 512 = 105906176`

---

### 3. Mount the Disk Image
```
sudo mount -o loop,offset=105906176 disk.img /mnt
```
---

### 4. Search for SSH Keys

SSH keys are typically stored in `~/.ssh/` directories. Searching for `.pub` files:

```bash
find /mnt -name *.pub
```

> Initially, permission was denied for `/mnt/root/.ssh`, so permissions were changed:

```bash
sudo chmod 777 /mnt/root sudo chmod 777 /mnt/root/.ssh
```

---

### 5. Extract the Private Key

```bash
ls -l /mnt/root/.ssh
```

**Output:**
```text
-rw------- 1 root root 411 Oct  6 10:30 id_ed25519 
-rw-r--r-- 1 root root  96 Oct  6 10:30 id_ed25519.pub
```


Granted permission to read the private key:
```bash
sudo chmod 777 /mnt/root/.ssh/id_ed25519
```
---

### 6. Connect to the Remote Server via SSH

```bash
ssh -i /mnt/root/.ssh/id_ed25519 -p 53924 ctf-player@saturn.picoctf.net
```
---

### 7. Retrieve the Flag

```bash
ls -a cat flag.txt`
```

**Flag:**
```text
picoCTF{k3y_5l3u7h_75b85d71}
```