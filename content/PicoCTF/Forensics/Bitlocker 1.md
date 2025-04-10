---
title: Bitlocker 1
draft: false
tags:
  - Forensic
  - Medium
  - Bitlocker
date: 2025-04-07
---
![[Bitlocker v1.png]]
## Solution
From the description we can know that we need to use password cracking tool like `john the ripper` to crack the password and decrypt it using `dislocker`.

### Step 1: Use `bitlocker2john` to generate the hash file

```bash
bitlocker2john -i bitlocker1.dd >> hash.txt
```
### ⚙️ Why does `bi2lockerjohn` generate a hash?
1. **BitLocker uses strong encryption (AES)** tied to passwords or recovery keys.
    
2. You can't brute-force the actual volume directly — instead, you need a **hash representation** of the encryption key derivation process.
    
3. `bi2lockerjohn` extracts this info and **generates a hash** in a format that **John the Ripper** understands (usually `$bitlocker$...` format).
    

This hash:
```hash
Encrypted device bitlocker-1.dd opened, size 100MB
Salt: 2b71884a0ef66f0b9de049a82a39d15b
RP Nonce: 00be8a46ead6da0106000000
RP MAC: a28f1a60db3e3fe4049a821c3aea5e4b
RP VMK: a1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58

UP Nonce: d04d9c58eed6da010a000000
UP MAC: 68156e51e53f0a01c076a32ba2b2999a
UP VMK: fffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d


User Password hash:
$bitlocker$0$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d
Hash type: User Password with MAC verification (slower solution, no false positives)
$bitlocker$1$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d
Hash type: Recovery Password fast attack
$bitlocker$2$16$2b71884a0ef66f0b9de049a82a39d15b$1048576$12$00be8a46ead6da0106000000$60$a28f1a60db3e3fe4049a821c3aea5e4ba1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58
Hash type: Recovery Password with MAC verification (slower solution, no false positives)
$bitlocker$3$16$2b71884a0ef66f0b9de049a82a39d15b$1048576$12$00be8a46ead6da0106000000$60$a28f1a60db3e3fe4049a821c3aea5e4ba1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58
```
- Represents the challenge of the key derivation function.
    
- Can be attacked offline using tools like `john` or `hashcat` with appropriate modules.

### Step 2: Use `john the ripper` to crack the `hast.txt` using `rocklist.txt`
![[Bitlocker v1 2.png]]

### Step 3: Create mount points for decrypted and final files.
```bash
sudo mkdir /mnt/dislocker /mnt/decrypted
```

### Step 4: Decrypt BitLocker volume using user password.
```bash
sudo dislocker -r -V "bitlocker-1.dd" -ujacqueline -- /mnt/dislocker
```

### Step 5: Check for the decrypted `dislocker-file`
```bash
sudo ls -lh /mnt/dislocker
```

### Step 6: Mount the loop device as a read-only filesystem.
```bash
sudo mount -o ro /mnt/dislocker /mnt/decrypted
```

### Step 7: Print the flag found in `mnt/decrypted`
```bash
cat /mnt/decrypted/flag.txt
```

**Result:**
```text
picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
```
