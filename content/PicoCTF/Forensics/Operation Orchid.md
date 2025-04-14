---
title: Operation Orchid
draft: false
tags:
  - Forensic
  - Autopsy
  - Easy
  - Openssl
date: 2025-04-13
---
![[Pasted image 20250413194055.png]]
## Solution
Analyze the file using `autopsy`, then found the `flag.txt.enc` which is being encoded in `aes-256` with the password `unbreakablepassword1234567`. Therefore, just use `openssl enc <hash type> -d -in <encoded file>` to get the flag.

![[Pasted image 20250413192824.png]]


```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ openssl enc -aes-256-cbc -d -in vol4-3.root.flag.txt.enc                            

enter AES-256-CBC decryption password:
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
bad decrypt
40872693757F0000:error:1C800064:Provider routines:ossl_cipher_unpadblock:bad decrypt:../providers/implementations/ciphers/ciphercommon_block.c:107:
picoCTF{h4un71ng_p457_5113beab} 
```

**Result:**
```text
picoCTF{h4un71ng_p457_5113beab}
```
