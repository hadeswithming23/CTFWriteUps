---
title: Torrent Analysis
draft: false
tags:
  - Forensic
  - pcap
  - Networking
  - Torrent
date: 2025-04-11
---
## 📁 Torrent Analysis — _What are Seeds, Peers, and Leechers?_

> "Choose your peers wisely — it's the key to faster BitTorrent downloads!"

### 🔑 Key Terms in Torrenting

- **Seed**: A _seed_ refers to a user who has already downloaded the complete file and is now uploading (sharing) it with others. More seeds = higher available bandwidth = faster downloads.
    
- **Peer**: A _peer_ is a participant who is still downloading the file. Peers can download parts from both seeds and other peers. However, peers compete with one another for limited bandwidth, so if there are too many peers, speed might drop.
    
- **Leechers**: A **leecher** is a participant in a torrent swarm who is currently **downloading a file but hasn't completed it yet**. While leechers download, they also upload parts of the file they have already received to other users.
	- **Originally**, the term had a **negative connotation**, referring to users who **intentionally limited their upload speed** or used modified torrent clients to avoid sharing — behaving like digital parasites that "took" but didn't "give".
	    
	- **Today**, the term simply describes anyone who **hasn't yet finished downloading** the file. Most leechers **do upload** while downloading, although often at a lower rate (1kb/s).
    

---

## 🚨 Challenge: Who's Torrenting on the Network?

### 🔍 Description & Hints

> One of your colleagues is using BitTorrent on the company network. Can you find out **what file** they were downloading? The **file name** will be the flag, e.g. `picoCTF{filename}`.

- 📥 **Hint 1**: Download and open the PCAP file with Wireshark.
    
- 🔄 **Hint 2**: Enable BitTorrent protocols like BT-DHT via `Analyze -> Enabled Protocols`.
    
- 👀 **Hint 3**: Understand seeds, peers, leechers.
    
- 📄 **Hint 4**: File name ends with `.iso`.
    

---

## 🧠 Strategy (Recon)

The PCAP file logs traffic involving BitTorrent file transfers. Using the BT-DHT (BitTorrent Distributed Hash Table) protocol, files are identified using a unique **info_hash** — kind of like a fingerprint of the file.

### 🕵️ Goal: Extract the info_hash and map it to the filename

---

## 🧪 Exploitation Steps

### 🔎 Step 1: Filter in Wireshark

Use a display filter to isolate relevant packets:

```wireshark
bt-dht contains "info_hash"
```

This reveals several packets, each containing different `info_hash` values. But identifying the correct one manually is inefficient.

---

## 🐍 Step 2: Python Script to Dump All Info Hashes



```python
import pyshark
from collections import defaultdict

# Load the pcap file with a display filter for BitTorrent DHT packets containing info_hash
capture = pyshark.FileCapture('torrent.pcap', display_filter='bt-dht contains "info_hash"')

# Dictionary to count occurrences of each info_hash
info_hash_counts = defaultdict(int)

# Process each packet
for pkt in capture:
    try:
        # Extract the info_hash field from the packet
        info_hash = pkt.layers[3].get_field_by_showname('info_hash').showname_value
        info_hash_counts[info_hash] += 1
    except Exception as e:
        # Skip packets that do not have the expected structure
        print(f"Error processing packet: {e}")
        continue

# Display the results
print("\nInfo Hash Counts:")
for info_hash, count in sorted(info_hash_counts.items(), key=lambda x: x[1], reverse=True):
    print(f"{info_hash}: {count} times")
```



### 🧾 Output:

```text
Info Hash Counts:
e2467cbf021192c241367b892230dc1e05c0580e: 29 times
17d62de1495d4404f6fb385bdfd7ead5c897ea22: 1 times
17c1e42e811a83f12c697c21bed9c72b5cb3000d: 1 times
d59b1ce3bf41f1d282c1923544629062948afadd: 1 times
078e18df4efe53eb39d3425e91d1e9f4777d85ac: 1 times
7af6be54c2ed4dcb8d17bf599516b97bb66c0bfd: 1 times
17c0c2c3b7825ba4fbe2f8c8055e000421def12c: 1 times
17c02f9957ea8604bc5a04ad3b56766a092b5556: 1 times
```


The hash **`e2467cbf021192c241367b892230dc1e05c0580e`** appeared the most — indicating the file was large and downloaded the most.

---

## 🌐 Step 3: Look up the info_hash online

Search the hash online on a **torrent hash lookup site** (like [btindex.org](https://btindex.org), [itorrents.org](https://itorrents.org), etc.)

### ✅ Result:

```text
Info Hash: e2467cbf021192c241367b892230dc1e05c0580e
File Name: ubuntu-19.10-desktop-amd64.iso
```


---

## 🏁 Flag

```text
picoCTF{ubuntu-19.10-desktop-amd64.iso}
```
