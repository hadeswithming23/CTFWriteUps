---
title: Red
draft: false
tags:
  - Forensic
  - Easy
  - LSB
  - Zsteg
date: 2025-04-06
---
## Red
![[Red.png]]


## Solution
From the description, which enforces the word "Red", so it might be related to LSB (Least Significant Bytes) which refers that might be some information hidden beneath the image. Therefore, in this challenge you can use the tool `zsteg` to find the flag.

### 💾 **How LSB Stores Information**

LSB steganography works by replacing the **least significant bit** of each byte in a cover file (like an image or audio file) with the **bits of the secret data** you want to hide. This causes **minimal change** to the original file, so the hidden data is not obvious.

#### 🖼 Example with an Image Pixel:

Suppose a pixel is stored using 3 bytes (RGB):

- Red = `10110010`
    
- Green = `11101100`
    
- Blue = `11010101`

We want to hide the bits `101` (from a secret message).

We replace the LSB of each color:

- Red becomes `101100**1**0` → `101100**1**1` (last bit changes to 1)
    
- Green becomes `111011**0**0` → `111011**0**0` (no change)
    
- Blue becomes `110101**0**1` → `110101**0**1` (no change)
    

**Result**: Secret bits `101` are stored without noticeable changes to the image.

### 🔍 What Does `zsteg` Do?

`zsteg` analyzes image files (especially PNGs) to detect hidden messages, typically hidden using **LSB** or **bit-plane** techniques.

![[Red2.png]]

### 📘 Understanding the Output: `b1,rgba,lsb,xy`

This line tells you **where and how** the data is hidden.

### Let's break down `b1,rgba,lsb,xy`:

|Component|Meaning|
|---|---|
|`b1`|Bit plane 1 → this means it’s using the **least significant bit (LSB)**|
|`rgba`|Uses all four channels: **Red, Green, Blue, Alpha**|
|`lsb`|It's checking the **Least Significant Bit** of each byte|
|`xy`|Reading the image **pixel by pixel (left to right, top to bottom)**|

So `b1,rgba,lsb,xy` means:

> Extract the **1st (least significant)** bit from each **RGBA channel**, reading pixels in **XY order**, and reconstruct the message from those bits.


### 🧠 What Did It Find?

This part:
```bash
text: "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ=="
```
That’s **Base64-encoded text**, repeated several times.

Decode it and you will get the flag

```bash
echo "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==" | base64 -d
```

Result:
```text
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_} 
```