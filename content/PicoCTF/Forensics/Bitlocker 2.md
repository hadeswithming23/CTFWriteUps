---
title: Bitlocker 2
draft: false
tags:
  - Forensic
  - Medium
  - Bitlocker
  - Volatility
date: 2025-04-10
---
![[Bitlocker v2.png]]
## Solution
From the description we can know that the RAM was being captured. Therefore, we could use an plugin in volatility 2.6 `dislocker` to get the memory dump of the `vdek` file.

### Step 1: Find the profile of the memory dump
```bash
vol2 -f memdump.mem imageinfo
```

```text
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : No suggestion (Instantiated with Win10x64_19041)
                     AS Layer1 : SkipDuplicatesAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/kali/Downloads/picoCTF2025/memdump.mem)
                      PAE type : No PAE
                           DTB : 0x1ad000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2025-03-10 02:58:56 UTC+0000
     Image local date and time : 2025-03-09 22:58:56 -0400

```


### Step 2: extract BitLocker encryption keys from RAM and possibly decrypt a BitLocker volume using `dislocker`
```bash
vol2 -f memdump.mem --profile=Win10x64_19041 bitlocker --dislocker dumps
```

![[Bitlocker v2 2.png]]
Choose the FVEK file which has the same version as the OS `4f79d4a00d5e9b25965b89581a6a599c`

### Step 3: Create folder for `mnt/dislocker` & `mnt/decrypted`
```bash
sudo mkdir mnt/dislocker mnt/decrypted
```

### Step 4: Decrypt the BitLokcer-encrypted disk image using the **Full Volume Encryption Key (FVEK)** extracted from your memory dump
```bash
sudo dislocker -V bitlocker-2.dd -k ../dumps/0x8087865bead0-Dislocker.fvek -- /mnt/dislocker
```

### Step 5: Mount the decrypted BitLocker volume (`dislocker-file`) to `/mnt/decrypted`
1. Setup loop device 
```bash
sudo losetup --find --show /mnt/dislocker/dislocker-file
```

2. **Mount the decrypted volume** as NTFS:
```bash
sudo mount -t ntfs-3g -o ro /dev/loop1 /mnt/decrypted
```

- `-t : types of the system`
- `-o : options, ro means read only`
### 🧐 **Why NTFS?**

When you **decrypt a BitLocker-encrypted drive** using tools like **Dislocker**, the decrypted data is typically **formatted with NTFS** (the default file system for Windows). Here's why you may need NTFS specifically in this context:

1. **BitLocker Uses NTFS**: BitLocker encryption is typically used on NTFS-formatted volumes in Windows. After the encryption is removed (decrypted), the **underlying file system is still NTFS**, but you can’t access it normally without mounting it correctly.
    
2. **Dislocker Creates a Virtual NTFS Volume**: When you use **Dislocker**, it creates a **virtual decrypted file** (the `dislocker-file`), which contains the original data formatted as **NTFS**. To access the data, you need to mount it as an NTFS file system, using the proper tools, like **ntfs-3g** on Linux.

### Step 6: Print the flag
```bash
cat /mnt/decrypted/flag.txt
```

**Result:**
```text
picoCTF{B1tl0ck3r_dr1v3_d3crypt3d_9029ae5b}
```
