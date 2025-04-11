---
title: Pcap Poisoning
draft: false
tags:
  - Forensic
  - Networking
  - Wireshark
  - pcap
  - Easy
date: 2025-04-11
---


![](https://tan-junwei.github.io/CTF-Writeups/Assets/PicoCTF-PcapPoisoning.png)

After downloading the file, I realize that it is a `pcap` file, as the challenge name already suggests.

### Using `strings`
```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ strings trace.pcap| grep pico    
picoCTF{P64P_4N4L7S1S_SU55355FUL_31010c46}
```
Alternatively, we can use Wireshark for packet analysis.

### Using Wireshark
![[Pasted image 20250411155210.png]]
Using the display filter `tcp contains "pico"`, we narrow our search to 1 packet that contains our flag.

```text
picoCTF{P64P_4N4L7S1S_SU55355FUL_31010c46}
```

