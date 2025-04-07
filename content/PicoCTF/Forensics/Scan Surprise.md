---
title: Scan Surprise
draft: false
tags:
  - Forensic
  - Easy
  - zbar
date: 2025-04-07
---
![[Scan Surprise.png]]
## Solution
From the description, we can know that the flag is being hidden in zip file in qr code format. Therefore, we can use a tool like `zbar-tool` to encode it.

### Step 1: Download the zip file
```bash
wget https://artifacts.picoctf.net/c_atlas/1/challenge.zip
```

### Step 2: Unzip the file
```bash
7z -x challenge.zip
```

### Step 3: Use `zbarimg` to decode the flag.png
```bash
zbarimg flag.png 
```

Download the tools if haven't installed
```bash
sudo apt install zbar-tools
```


Result:
```text
picoCTF{p33k_@_b00_3f7cf1ae}
```

