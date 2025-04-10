---
title: Event-Viewing
draft: false
tags:
  - Forensic
  - Medium
  - Event-Logs
date: 2025-04-10
---
![[Event-Viewing.png]]
## Solution 
From the description, we know that that are several things we needed to look for which are:
- installed software
- login
- shutdown

Therefore, to analyze an `.evtx` file on a Linux system you need to download the `python-evtx`.

### Step 1: Install python-evtx

```bash
sudo apt install python3-evtx
```


### Step 2: Export to XML for better readability
```bash
evtx_dump.py Windows_Logs.evtx > logs.xml  
```

### Step 3: Search for related event (eg: install)
```bash
grep -i -a45 install logs.xml
```

![[Event-Viewing 2.png]]
The installed program `Totally_Legit_Software` has an event data that encoded in base 64 which is the first part of the flag

```text
echo "cGljb0NURntFdjNudF92aTN3djNyXw==" | base64 -d
picoCTF{Ev3nt_vi3wv3r_
```

```bash
grep -i -a45 "Totally_Legit_Software.exe" logs.xml
```
![[Event-Viewing 4.png]]

```text
echo "MXNfYV9wcjN0dHlfdXMzZnVsXw==" | base64 -d   
1s_a_pr3tty_us3ful_ 
```


```bash
grep -i -a45 1074 logs.xml 
```
![[Event-Viewing 3.png]]
- 1074 is shutdown logs

```text
echo "dDAwbF84MWJhM2ZlOX0=" | base64 -d
t00l_81ba3fe9} 
```

**Result:**
```text
picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}
```
