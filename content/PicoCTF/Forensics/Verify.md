---
title: Verify
draft: false
tags:
  - Easy
  - Forensic
  - SHA-256
date: 2025-04-06
---
![[Verify.png]]
## Solution
From the description we can know that we need to find and decrypt a file that has the same checksum using `SHA-256` hash. Therefore, we need to connect to server using `ssh -p 65223 ctf-player@rhea.picoctf.net` and enter the password provided. Then, just find the file that has the same hash value as the checksum provided in the system and decrypt it using the `./decrypt.sh`

![[Verify2.png]]

Result:
```text
picoCTF{trust_but_verify_00011a60}
```
