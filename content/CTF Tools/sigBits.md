---
title: sigBits
draft: false
tags:
  - Forensic
  - Steganography
date: 2025-04-11
---
## 🔍 What Does `sigBits` Do?

`sigBits` allows you to extract hidden data from images by targeting:​

- **Least Significant Bits (LSB):** Often used in steganography to hide data with minimal impact on the image's appearance.​
    
- **Most Significant Bits (MSB):** Less commonly used but can store more noticeable alterations, sometimes used to mislead or distract.​
    
- **Custom Bit Selection:** Specify exact bits to extract, offering flexibility for various steganographic techniques.
    

The tool supports different color channel orders (e.g., RGB, BGR) and extraction methods (by row or column), providing versatility in analyzing images.​

---

## ⚙️ How to Use `sigBits`

### 📥 Installation

Ensure you have Python 3 installed. Then, install the required Pillow library:
```bash
pip install Pillow
```


Download the `sigBits.py` script from the [GitHub repository](https://github.com/Pulho/sigBits).​
```bash
wget https://raw.githubusercontent.com/Pulho/sigBits/master/sigBits.py  
```

### 🛠️ Usage Syntax

```bash
python sigBits.py [OPTIONS] [FILE]
```

### 🔧 Options

- `-h`, `--help`: Display help information.​
    
- `-t=`, `--type=`: Choose between reading LSB or MSB (default is LSB).​
    
- `-o=`, `--order=`: Specify the order of color channels (default is RGB).​[
    
- `-out=`, `--output=`: Set the name of the output file (default is `outputSB`).​
    
- `-e=`, `--extract=`: Choose extraction method: by row or column (default is column).​
    
- `-b=<bits>`, `--bits=<bits>`: Specify exact bits to extract; this overrides the `--type` option.​
    

### 📌 Examples

- Extract LSB data from an image:
```bash
python sigBits.py -t=lsb -o=rgb -out=MyOutputFile -e=row MyImage.png
```
    
- Extract MSB data with a different color channel order:​
```bash
python sigBits.py -t=msb -o=bgr -e=column AnotherImage.jpg
```
    
- Extract specific bits (e.g., bits 0 and 1):​
```bash
python sigBits.py -b=01 -o=rgb -e=row ImageWithHiddenData.png
```
