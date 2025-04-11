---
title: mob psycho
draft: false
tags:
  - Forensic
  - APK
  - Easy
date: 2025-04-10
---
![[Mob psycho.png]]
## Solution
Unzip the APKs file, then search the flag using commands like `strings` and `cat`.

### Step 1: Unzip the file 
```bash
unzip mobpsycho.apk
```

### Step 2: Find the flag file
```bash
strings mobpsycho.apk | grep flag
```
- Run `strings` because the APK file is in binary format, executing `strings` will convert the binary form of it to readable text

**Result:**
**res/color/flag.txt**

### Step 3: Print the `flag.txt` file
```bash
cat res/color/flag.txt
```

```text
7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f38356462643231357d
```

###  Step 4: Decode using `xxd`
```bash
cat res/color/flag.txt | xxd -r -p
```

**`xxd -r -p`**: This takes the output (which is a **plain hexadecimal string**) from `cat` and:
- **`-r`** reverses the hexadecimal format back to its binary form.
    
- **`-p`** ensures that `xxd` treats the input as a **plain hex string** (without formatting), so it directly converts the hex into its original binary data.


**Result:**
```text
picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_85dbd215}
```
