---
title: Dear Diary
draft: false
tags:
  - Forensic
  - Disk
  - Autopsy
  - Easy
date: 2025-04-11
---
![](https://miro.medium.com/v2/resize:fit:700/1*d7MmJY4O_klQh6iOkrVQlg.png)

## Description

Hint: If you’re observing binary data raw in the terminal you may be misled about the contents of a block. The given file is disk.flag.img, so I use autopsy to analyze it. Moreover, should be looking at the what is being executed which is the fragments instead of the file itself (file content being removed).

![](https://miro.medium.com/v2/resize:fit:624/1*aDIRNN6_4-v3WDKbZhiWQg.png)

![](https://miro.medium.com/v2/resize:fit:700/1*AE-3FYakJx5QXXtT7Iki6A.png)

Create a new case and name it whatever you want, then add the path to the img file downloaded.

![](https://miro.medium.com/v2/resize:fit:629/1*XPlyY6togdFqRmIT6HjnsQ.png)

I use all the default settings and analyze the raw disk file.

![](https://miro.medium.com/v2/resize:fit:666/1*Ep7vMbEvGXAAhouLTb9XwQ.png)

I then used **keyword search** with **ASCII** option to search for raw data. My first search is “.txt” to find if there’s a flag file. Although there’s no file named flag directly, I saw parts of the flag distributed around `/img_disk.flag.img/vol_vol4/root/secret-secrets/innocuous-file.txt`

![](https://miro.medium.com/v2/resize:fit:700/1*LRrSGhpNVRUJNnXMc2eEAg.png)

![](https://miro.medium.com/v2/resize:fit:700/1*umWw0Mir8Yn9EBmQeb70Vg.png)

![](https://miro.medium.com/v2/resize:fit:700/1*MZuoLashyynpU1vr8PXqjQ.png)

Just like that, we can continue to view some more files, assemble all the parts found, and then send them in smoothly.

**Result:**
```text
picoCTF{1_533_n4m35_80d24b30}
```