---
title: Flags are stephic
draft: false
tags:
  - Forensic
  - Medium
date: 2025-04-07
---
![[Pasted image 20250407205512.png]]
## Solution
It's a webpage, so the first thing needs to do is view the page source `CTRL + U` to find is there any things can be found. In addition, the hidden data might be encoded in LSB using `stepic`.

### 🕵️‍♂️ Stepic (Python steganography tool)

**Stepic** is a **Python library** used for **hiding messages inside image files**, particularly **PNG** images. It works by modifying the **least significant bits (LSBs)** of the image’s pixel data to embed the message — a common technique in steganography.

### Step 1: View the source code of the web page
![[Pasted image 20250407205937.png]]
![[Pasted image 20250407205930.png]]
And I found there's one row which is having the tag `important` . Next, the next step would be downloading it and analyses the image. 

### Step 2: Find the hidden information using `stepic`
```bash
stepic -d -i upz.png
```

**Result:**
```text
picoCTF{fl4g_h45_fl4g9a81822b}
```
