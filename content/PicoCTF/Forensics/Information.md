---
title: Information
draft: false
tags:
  - Forensic
  - Easy
  - Metadata
date: 2025-04-07
---
![[Information.png]]

## Solution
The description states **files can always be changes** which might refer to the metadata of the file. Therefore, can use `exiftool` to start the analysis first.

### Step 1: Use `exiftool` on that `cat.jpg`
![[Information2.png]]
From the License, it seems like base64, let's decode it.

### Step 2: Decode the `License` field from base 64
```bash 
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
```

**Result:**
```text
picoCTF{the_m3tadata_1s_modified}
```
