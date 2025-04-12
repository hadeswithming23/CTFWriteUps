---
title: Where Did He Go
draft: false
tags:
  - Forensic
  - Osint
  - Easy
  - I2C
date: 2025-04-12
---
![[Where did he go.png]]
## 🕵️‍♀️ CTF Challenge Write-up: _Where Did He Go?_

### 🎯 Challenge Category: Forensics

### 🧠 Skills Involved: Logic signal analysis, I2C protocol decoding, GPS coordinate interpretation, OSINT

---

### 🧩 Challenge Description:

A friend of yours went silent after telling you he’s attending English lessons at a new language center. The only clue left was a logic capture file named `GPS_LA.sal`. Can you figure out where he went?

---

### 🔍 Investigation Steps:

#### 1. **Understanding the File**

The challenge provided a `GPS_LA.sal` file. Opening it with [Logic 2 by Saleae](https://www.saleae.com/) revealed 8 digital channels with a recorded I2C communication.

#### 2. **Decoding the Signal**
Using Logic 2’s I2C protocol analyzer or `sigrok-cli`, we decoded communication between an MCU and an EEPROM device.

![[Where did he go 3.png]]
Extracted I2C data written to address `0x50` (likely an AT24C32 EEPROM):

```hex
0x00 0x00 0x24 0x47 0x50 0x47 0x4C 0x4C 0x2C 0x35 0x30 0x32 0x36 0x2E 0x36 0x30 0x38 0x32 0x2C 0x4E 0x2C 0x30 0x31 0x38 0x35 0x31 0x2E 0x34 0x36 0x38 0x35 0x2C 0x45 0x2C 0x31 0x32 0x33 0x35 0x31 0x39 0x2C 0x41 0x2A 0x32 0x38
```

#### 3. **Decoding the GPS Data**

Converted from hex to ASCII:

```
$GPGL,5026.6082,N,01851.4685,E,123519,A*28
```

This appears to be an NMEA GPS sentence:

- **Latitude**: `5026.6082,N` → **50.44347° N**
    
- **Longitude**: `01851.4685,E` → **18.85781° E**
    
#### 4. **Reverse-Geolocation**

Using Google Maps or an OSINT geolocation tool, the coordinates lead to **Golden Gate Language Centre**, located in Poland.

#### 5. **Confirming the Location**
![[Where did he go 2.png]]

A quick OSINT search around the location confirmed that **Golden Gate** is a real English language learning center, aligning perfectly with the backstory of the challenge.

---

### 🏁 The Flag

Based on the name of the language center:

```
1753c{GOLDEN_GATE}
```

