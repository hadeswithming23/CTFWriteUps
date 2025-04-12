---
title: Sleuthkit Apprentice
draft: false
tags:
  - Forensic
  - Autopsy
  - Disk
  - Easy
---
![[Sleuthkit Apprentice.png]]
## Solution 
Analyze the disk using `Autopsy`. Especially looking into the `bash history`.

### Step 1: Analyze on the bash history
![[Sleuthkit Apprentice2.png]]
It seems like he file is being saved in `/myfolder/flag.txt` and being encoded in flag.uni.txt.

### Step 2: Export and decode the file in UTF-16
![[Sleuthkit Apprentice3.png]]

**Result:**
```text
picoCTF{by73_5urf3r_adac6cb4}
```