---
title: Secret of the Polyglot
draft: false
tags:
  - Forensic
  - Easy
  - FileFormat
date: 2025-04-07
---
![[Secret of the Polyglot.png]]
## Solution
From the description, you will know that the challenge is related to file format. 

### Step 1: Use `open` to open the file but it seems likes is the 2nd part of the flag
![[Secret of the Polyglot2.png]]

### Step 2: Check the file format of the challenge file
![[Secret of the Polyglot3.png]]

### Step 3: Change the file extension to `.png`
```bash
mv flag2of2-final.pdf flag.png
```

### Step 4: Use `eog` to open the file
![[Secret of the Polyglot4.png]]


Result:
```text
picoCTF{f1u3n7_1n_pn9_&_pdf_53b741d6}
```
