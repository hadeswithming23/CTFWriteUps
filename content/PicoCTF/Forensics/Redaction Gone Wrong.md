---
title: Redaction Gone Wrong
draft: false
tags:
  - Forensic
  - Easy
  - PDF
  - Redaction
date: 2025-04-12
---
![[Redaction Gone Wrong.png]]
## Solution
Just use the command `pdftotext` to get all the texts.

```bash
┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ pdftotext Financial_Report_for_ABC_Labs.pdf

┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ ls
Financial_Report_for_ABC_Labs.pdf  Financial_Report_for_ABC_Labs.txt

┌──(kali㉿kali)-[~/Downloads/picoCTF]
└─$ cat Financial_Report_for_ABC_Labs.txt 
Financial Report for ABC Labs, Kigali, Rwanda for the year 2021.
Breakdown - Just painted over in MS word.

Cost Benefit Analysis
Credit Debit
This is not the flag, keep looking
Expenses from the
picoCTF{C4n_Y0u_S33_m3_fully}
Redacted document.
```

**Result:**
```text
picoCTF{C4n_Y0u_S33_m3_fully}
```
