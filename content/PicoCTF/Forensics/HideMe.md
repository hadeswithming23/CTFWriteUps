---
title: HideMe
draft: false
tags:
  - Forensic
  - Steganography
  - Easy
date: 2025-04-11
---
![[HideMe.png]]
## Solution 
Based on the challenge file, it is a `.png` which means we can use `zsteg` to check whether there is hidden data.

### Step 1: Extract LSB data from the `flag.png`
```bash
└─$ zsteg flag.png
[?] 3180 bytes of extra data after image end (IEND), offset = 0x9b3b
extradata:0         .. file: Zip archive data, at least v1.0 to extract, compression method=store
    00000000: 50 4b 03 04 0a 00 00 00  00 00 3c 10 70 56 00 00  |PK........<.pV..|
    00000010: 00 00 00 00 00 00 00 00  00 00 07 00 1c 00 73 65  |..............se|
    00000020: 63 72 65 74 2f 55 54 09  00 03 93 78 12 64 93 78  |cret/UT....x.d.x|
    00000030: 12 64 75 78 0b 00 01 04  00 00 00 00 04 00 00 00  |.dux............|
    00000040: 00 50 4b 03 04 14 00 00  00 08 00 3c 10 70 56 75  |.PK........<.pVu|
    00000050: 61 47 c7 2a 0b 00 00 c7  0b 00 00 0f 00 1c 00 73  |aG.*...........s|
    00000060: 65 63 72 65 74 2f 66 6c  61 67 2e 70 6e 67 55 54  |ecret/flag.pngUT|
    00000070: 09 00 03 93 78 12 64 93  78 12 64 75 78 0b 00 01  |....x.d.x.dux...|
    00000080: 04 00 00 00 00 04 00 00  00 00 cd 56 57 3c db 8f  |...........VW<..|
    00000090: 16 ff a9 52 ad 51 45 ed  22 66 8b 98 0d 4a ed 68  |...R.QE."f...J.h|
    000000a0: ec 99 9a 6d cc 34 66 43  cc 88 59 a3 43 69 5d 1f  |...m.4fC..Y.Ci].|
    000000b0: 54 cd 52 5b aa 8a 18 b1  7a cd 96 52 b5 47 a3 f6  |T.R[....z..R.G..|
    000000c0: 9e a1 34 25 6e fe 8f f7  e1 be df f3 70 be 67 7d  |..4%n.......p.g}|
    000000d0: df be e7 7c ce 73 4b 73  18 eb 15 fe 2b 00 00 b0  |...|.sKs....+...|
    000000e0: 1a 19 42 ad 01 e0 82 3d  2d 56 62 a7 39 e0 c8 4f  |..B....=-Vb.9..O|
    000000f0: d2 96 06 17 51 ba 66 ba  00 50 f3 8a f9 d4 95 81  |....Q.f..P......|
```
It shows that the flag file is stored in zip under /secret/flag.png

### Step 2: Unzip and open the image file
```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ unzip flag.png                       
Archive:  flag.png
warning [flag.png]:  39739 extra bytes at beginning or within zipfile
  (attempting to process anyway)
   creating: secret/
  inflating: secret/flag.png         
```

```bash
eog secret/flag.png
```

![[HideMe 2.png]]

**Result:**
```text
picoCTF{Hiddinng_An_imag3_within_@n_ima9e_96539bea}
```
