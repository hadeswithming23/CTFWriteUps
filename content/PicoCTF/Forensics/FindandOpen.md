---
title: FindandOpen
draft: false
tags:
  - Forensic
  - Easy
  - pcap
  - Networking
date: 2025-04-11
---
![[FindAndOpen.png]]
## Solution
From the description, there is one zip file which contains the flag, and the `pcap` file contains the password to unzip the file.

### Step 1: Analyze the `pcap` file to find the password
![[FindAndOpen2.png]]

```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ echo "aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8" | base64 -d

he secret: picoCTF{R34DING_LOKd_ 
```

### Step 2: Unzip the file with the password
```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ unzip flag.zip                          
Archive:  flag.zip
[flag.zip] flag password: 
password incorrect--reenter: 
 extracting: flag                    

┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ cat flag 
picoCTF{R34DING_LOKd_fil56_succ3ss_419835ef}
```
