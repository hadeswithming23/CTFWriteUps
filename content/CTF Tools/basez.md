---
title: basez
draft: false
tags:
  - Encoding
date: 2025-04-14
---
## 🧩 What is `basez`?

`basez` is a **command-line tool** used for **base encoding and decoding** in various formats. It supports many base encodings that go beyond standard `base64`, including some rare or obscure ones.

Think of it like a supercharged version of `base64`.

---

## ✅ What Can `basez` Do?

- Encode and decode data using:
    
    - Base2, Base8, Base10, Base16, Base32, Base36, Base58, Base62, Base64, Base85
        
    - Base91, Base92, Base94, Ascii85, Z85
        
    - Hex, Binary, Decimal, and more
        
- Can also work with:
    
    - Custom alphabets
        
    - Compression (sometimes used with pipes)
        

---

## basez Supported Encodings and Commands

### 📋 Summary Table of Encodings

| **Command**   | **Alias(es)** | **Description**                                                           | **Standard / RFC**                                                     |
| ------------- | ------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `hex`         | `base16`      | Encodes/decodes using Base16 (hexadecimal: `0-9`, `a-f`).                 | [RFC 4648 §8](https://datatracker.ietf.org/doc/html/rfc4648#section-8) |
| `unhex`       | —             | Decodes hexadecimal input (inverse of `hex`).                             | —                                                                      |
| `base16`      | `hex`         | Alias for hex encoding (Base16 lowercase).                                | RFC 4648 §8                                                            |
| `base32`      | `base32plain` | Standard Base32 encoding with `A-Z`, `2-7` characters. Case-insensitive.  | RFC 4648 §6                                                            |
| `base32plain` | `base32`      | Explicit command for RFC 4648 Base32 (same as `base32`).                  | RFC 4648 §6                                                            |
| `base32hex`   | —             | Base32 with extended hex alphabet `0-9`, `A-V`.                           | RFC 4648 §7                                                            |
| `base64`      | `base64plain` | Standard Base64 encoding with `A-Z`, `a-z`, `0-9`, `+`, `/`.              | RFC 4648 §4                                                            |
| `base64plain` | `base64`      | Explicit name for standard Base64 encoding (same as `base64`).            | RFC 4648 §4                                                            |
| `base64url`   | —             | URL-safe Base64 encoding: `A-Z`, `a-z`, `0-9`, `-`, `_`.                  | RFC 4648 §5                                                            |
| `base64mime`  | —             | MIME Base64 encoding with line wrapping at 76 characters. Used in emails. | RFC 2045 §6.8                                                          |
| `base64pem`   | —             | PEM Base64 encoding used for certificates/keys with `BEGIN/END` lines.    | RFC 1421                                                               |

---

## ⚙️ Command Syntax

### General Usage
```bash
<command> [OPTION]... [FILE]
```

