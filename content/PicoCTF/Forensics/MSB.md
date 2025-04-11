---
title: MSB
draft: false
tags:
  - Forensic
  - Steganography
  - Easy
  - sigBits
date: 2025-04-11
---
![[Pasted image 20250411162755.png]]
## Solution
From the description, we can know that the data are probably encrypted in MSB instead of LSB. Therefore, we can use a tool called `sigBits` that is able to extract data from LSB or MSB.

The `sigBits` tool is a Python-based steganography decoder designed to extract hidden information from image files by analyzing specific bits within each pixel's color channels. It's particularly useful in Capture The Flag (CTF) challenges where data may be concealed in the least or most significant bits of an image.

---

### Step 1: Extract hidden information using `sigBits`
```bash
python3 sigBits.py -t=msb Ninja-and-Prince-Genji-Ukiyoe-Utagawa-Kunisada.flag.png

Done, check the output file!
```


### Step 2: Find the flag
```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ cat outputSB.txt | grep -o -E "picoCTF.{0,50}"
picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_06326238}"Thou h
```

**Result:**
```text
picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_06326238}
```
