---
title: Endianess-v2
draft: false
tags:
  - Forensic
  - Medium
  - LittleEndians
---
![[Endianness-v2.png]]
## Solution
From the description we can know that the file was a 32-bits system and its bytes is being organized in a weird way (little endian). Therefore, we need to check the file signature first.

### Step 1: Check the file signature
```bash
└─$ exiftool challengefile 
ExifTool Version Number         : 13.10
File Name                       : challengefile
Directory                       : .
File Size                       : 3.4 kB
File Modification Date/Time     : 2024:03:11 20:36:50-04:00
File Access Date/Time           : 2025:04:10 23:43:32-04:00
File Inode Change Date/Time     : 2025:04:10 23:43:20-04:00
File Permissions                : -rw-rw-r--
Warning                         : Processing JPEG-like data after unknown 1-byte header
```

So from the warning, we can know that the file signature should be JPEG

### Step 2: Check the file bytes
```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ xxd challengefile
00000000: e0ff d8ff 464a 1000 0100 4649 0100 0001  ....FJ....FI....
00000010: 0000 0100 4300 dbff 0606 0800 0805 0607  ....C...........
00000020: 0907 0707 0c0a 0809 0b0c 0d14 1219 0c0b  ................
00000030: 1d14 0f13 1d1e 1f1a 201c 1c1a 2027 2e24  ........ ... '.$
```

But a normal JPEG/JFIF starts with:

```bash
ffd8 ffe0 0010 4a46 4946
```

This suggests the **byte order is wrong** — specifically, it looks like **each group of 4 bytes was reversed**, consistent with how a 32-bit little-endian system might write data from a big-endian image buffer.

### 🧠 Root Cause

The 32-bit system likely wrote `uint32_t` values (4 bytes each) to the file in **little-endian order**. This corrupted the byte order of the original image.

We need to:

- Read the file 4 bytes at a time (a _chunk_)
    
- Reverse each 4-byte chunk to restore the original order

### 🛠️ Solution

Here's the Python script used to fix the image:

```python
with open("challengefile", "rb") as file:
	stream = file.read()
	endian_change = b''
	for it in range(0, len(stream), 4):
		chunk = stream[it:it+4]
		endian_change += chunk[::-1]  # Reverse each 4-byte chunk
	with open("image.jpg", "wb") as img:
		img.write(endian_change)
```

**Result:**
![[Endianness-v2 2.png]]

## 🔁 Explaining the `for` Loop: `for it in range(0, len(stream), 4)`

In the Python script that fixes the corrupted image, we use the following loop:

```python
for it in range(0, len(stream), 4):
	chunk = stream[it:it+4]
	endian_change += chunk[::-1]
```
### 🔍 Purpose

This loop processes the corrupted file in **4-byte chunks** and reverses the order of bytes in each chunk to fix a 32-bit endian issue.

---

### 📌 Breaking it Down

- `stream` is the full binary content of the corrupted file, read using:
```python    
stream = file.read()
```
- `len(stream)` gives the **total number of bytes** in the file.
    
- `range(0, len(stream), 4)` means:
    
    - Start at byte `0`
        
    - Go up to the end of the file (`len(stream)`)
        
    - Move **4 bytes at a time**
        

Each iteration:

1. Takes a **4-byte chunk**:
```python
chunk = stream[it:it+4]
```

2. Reverses that chunk:
```python
chunk[::-1]
```
    
3. Adds it to the fixed output:
    `endian_change += ...`
---

### 💡 Why 4 Bytes?

Because the corruption came from a **32-bit system** (4 bytes = 32 bits). The system stored each 4-byte word in **little-endian** order, which reversed the intended byte sequence.

To undo the damage, we reverse each 4-byte block to restore the original byte order.

---

### 🔠 Visual Example

Given this hex data:

```text
Original Corrupted Bytes:   01 02 03 04  AA BB CC DD  11 22 33 44 
Indexes:                     0  1  2  3   4  5  6  7   8  9  10 11
```

The loop does:

| Iteration | `it` Value | `stream[it:it+4]` | Reversed Chunk |
| --------- | ---------- | ----------------- | -------------- |
| 1         | 0          | `01 02 03 04`     | `04 03 02 01`  |
| 2         | 4          | `AA BB CC DD`     | `DD CC BB AA`  |
| 3         | 8          | `11 22 33 44`     | `44 33 22 11`  |

Each reversed chunk is then combined to rebuild the fixed binary.

---
### ✅ Final Summary

> The `for` loop reads the file 4 bytes at a time, reverses each chunk, and rebuilds the binary data with the correct byte order.

This is critical for recovering files corrupted by **32-bit endian mismatches**, such as those dumped from embedded systems.