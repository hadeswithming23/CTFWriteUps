---
title: Phantom Intruder
draft: false
tags:
  - Easy
  - Forensic
  - Networking
  - pcap
  - tshark
date: 2025-04-06
---
![[Phantom Intruder.png]]

## Solution
From the description, you will know that's is a pcap file that mainly need to analyze the intrusion activity carried by the hacker in a timely manner. So, just need to download the pcap file and proceed to analyze it with `wireshark`.

### Step 1: Click on the time column to rearrange the packets
![[Phantom Intruder2.png]]
So from the picture above, is clearly shows that illegal segments are being sent out with the length of 52 and encoded in base 64.

Step 2: Use `tshark` command to grep all the data and decode it to get the flag
```bash
tshark -r myNetworkTraffic.pcap -Y "tcp.len==12 || tcp.len==4" -T fields -e frame.time -e tcp.segment_data | sort -k4 | awk '{print $6}' | xxd -p -r | base64 -d
```

- `tshark`: Network protocol analyzer (similar to Wireshark, but command-line based).
- `-r myNetworkTraffic.pcap`: Read packets from the `myNetworkTraffic.pcap` file.
- `-Y "tcp.len==12 || tcp.len==4"`: Filter TCP packets with length 12 or 4 bytes.
- `-T fields`: Output the specific fields.
- `-e frame.time`: Display the timestamp of the frame.
- `-e tcp.segment_data`: Extract the TCP segment data (payload).
- **`sort -k4:`**Sorts the output based on the 4th field (typically the timestamp ).
- **`awk '{print $6}':`**Prints the 6th field from the sorted output, which is the actual segment data (TCP payload).
- **`xxd -p -r:`**Converts the data from a hex dump to binary (`r` reverses the hex dump), so the data is in its raw format.
- **`base64 -d:`**Decodes the raw data from Base64 format back into its original binary form.

Result:
```text
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_8e10e839} 
```