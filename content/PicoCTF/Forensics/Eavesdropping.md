---
title: Eavesdropping
draft: false
tags:
  - Forensic
  - tshark
  - Networking
  - Easy
date: 2025-04-14
---
  

# 🕵️‍♂️ Eavesdropping with Wireshark & Tshark

In this challenge, we’re given a packet capture file (`capture.flag.pcap`) and need to extract a secret flag. Although it's possible to use Wireshark GUI (`Analyze → Follow TCP Stream`), we’ll stick with **`tshark`**, the command-line version of Wireshark.

---
## 📦 Step 1: Find TCP Streams in the PCAP


To identify active TCP conversations in the capture:
```bash
tshark -r capture.flag.pcap -q -z conv,tcp
```

**Output (shortened for clarity):**
```text
TCP Conversations

10.0.2.15:57876 <-> 10.0.2.4:9001     # Stream 0

10.0.2.15:43928 <-> 35.224.170.84:80  # Stream 1

10.0.2.15:56370 <-> 10.0.2.4:9002     # Stream 2
```

  

> These are the three main TCP streams in the PCAP file.
---


## 📨 Step 2: Analyze TCP Stream 0 — The Chat Log

```bash
tshark -r capture.flag.pcap -q -z follow,tcp,ascii,0
```

  
**Conversation:**

```text
Hey, how do you decrypt this file again?

You're serious?

Yeah, I'm serious

*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123

Ok, great, thanks.

...
```

  

🔑 **Key takeaway:** A user shares an OpenSSL command to decrypt a file using the password:  

`supersecretpassword123`

---

## 🌐 Step 3: Skip Stream 1 — Just a Connectivity Check

```bash

tshark -r capture.flag.pcap -q -z follow,tcp,ascii,1

```

  

This stream contains only a `GET` request to Ubuntu’s connectivity-check service.  

**Nothing interesting here, skip it.**

---
## 🔐 Step 4: Extract the Encrypted File from Stream 2

```bash
tshark -r capture.flag.pcap -q -z follow,tcp,ascii,2
```

The ASCII output shows:

```

Salted__............=a.....Z..........F8..v.<8EY

```

  

This looks like a file encrypted using OpenSSL with the `-salt` option. But since ASCII mode replaces non-printable characters with dots (`.`), we’ll need to extract the **raw binary**

---
## 🧪 Step 5: Extract Raw Binary Data

Use the raw output and pipe it to remove extra lines, then convert it back to binary:

```bash
tshark -r capture.flag.pcap -q -z follow,tcp,raw,2 | tail -n +7 | head -n 1 | xxd -r -p > secret
```

### 🔍 What does this do?

- `follow,tcp,raw,2`: Get raw hex dump of stream 2

- `tail -n +7`: Skip tshark’s header

- `head -n 1`: Get only the hex line

- `xxd -r -p`: Convert hex to binary

- `> secret`: Save the output to a file

---
## 🔓 Step 6: Decrypt the File with OpenSSL

Now that we have the encrypted binary file `secret`, decrypt it using the password from Stream 0:

```bash
openssl des3 -d -salt -in secret -k supersecretpassword123
```

**Output:**
```bash
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.

picoCTF{nc_73115_411_0ee7267a}                                
```

  
🎉 **FLAG FOUND!** → `picoCTF{nc_73115_411_0ee7267a}`
