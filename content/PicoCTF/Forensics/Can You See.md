---
title: Can You See
draft: false
tags:
  - Forensic
  - Easy
  - Metadata
date: 2025-04-07
---
![[Can You See.png]]

## Solution
From the description, is hide and seek so is probably hiding the flag in the metadata section. Therefore, just use `exiftool` to look at it and proceed to find the flag.

### Step 1: Use `exiftool` to get the metadata of the challenge file
```bash
exiftool ukn_reality.jpg
```

![[Can You See 2.png]]

### Step 2: Decode the `attribution URL` from base 64
```
echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fNmE5ZjVhYzR9Cg==" | base64 -d
```


**Result**:
```text
picoCTF{ME74D47A_HIDD3N_6a9f5ac4}
```
