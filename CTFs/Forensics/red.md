# 🧩 CTF Challenge Writeup  
**Challenge Name:** RED  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2025  
**Author:** SHUAILIN PAN (LECONJUROR)  
**Date:** 24-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a PNG image with the following description:

> *"RED, RED, RED, RED"*

A single PNG file `red.png` is provided for download.  
The hint **"The picture seems pure, but is it though?"** suggests the image looks like a plain red image but contains hidden data.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category  
- **Easy** difficulty  
- Description repeats **"RED"** four times — hinting at the four RGBA channels
- Hint says the image "seems pure" but isn't — suggesting hidden data in pixel values

Key observations:
- The image is 128x128 pixels in **RGBA** mode (4 channels)
- Inspecting pixel values reveals all values are either `0/1` or `254/255` — perfectly binary!
- Each channel encodes exactly **1 bit per pixel**: `254/0 = 0`, `255/1 = 1`
- Combining all 4 channels (RGBA) per pixel gives **4 bits per pixel**
- The resulting bitstream decodes to a **Base64 string** which contains the flag

The intended approach is to **extract the LSB from each RGBA channel**, combine them into a bitstream, and Base64 decode the result.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Image
```bash
wget <challenge-url>/red.png
```

### 3.2 Inspect the Image
```bash
exiftool red.png
strings red.png | grep picoCTF
```

Nothing found in metadata or strings — the flag is hidden in pixel data.

### 3.3 Inspect Pixel Values
```python
from PIL import Image

img = Image.open('red.png')
pixels = list(img.getdata())
print(f"Size: {img.size}, Mode: {img.mode}")
print(f"Unique red values: {sorted(set(p[0] for p in pixels))}")
for i, p in enumerate(pixels[:5]):
    print(f"Pixel {i}: {p}")
```

Output reveals:
```
Size: (128, 128), Mode: RGBA
Unique red values: [254, 255]
Pixel 0: (254, 1, 1, 254)
Pixel 1: (254, 0, 1, 255)
...
```

Every channel uses only two values — perfectly binary:

| Channel | Value 0 | Value 1 |
|---------|---------|---------|
| Red     | 254     | 255     |
| Green   | 0       | 1       |
| Blue    | 0       | 1       |
| Alpha   | 254     | 255     |

### 3.4 Extract Bits from All Channels

```python
from PIL import Image

img = Image.open('red.png')
pixels = list(img.getdata())

# Combine all 4 channels as bits per pixel
bits = ""
for pixel in pixels:
    bits += '1' if pixel[0] == 255 else '0'  # Red
    bits += str(pixel[1] & 1)                 # Green
    bits += str(pixel[2] & 1)                 # Blue
    bits += '1' if pixel[3] == 255 else '0'  # Alpha

# Convert bits to characters
chars = []
for i in range(0, len(bits), 8):
    byte = bits[i:i+8]
    if len(byte) == 8:
        chars.append(chr(int(byte, 2)))

result = ''.join(chars)
print(result[:100])
```

Output:
```
cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==
```

### 3.5 Base64 Decode the Result

```bash
echo "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==" | base64 -d ; echo
```

Output:
```
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

---

## 🧩 4. Final Flag

```
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

---

## 📚 5. Key Learnings

- Images can hide data by using **near-identical pixel values** (254 vs 255) that look identical to the human eye but encode binary data
- The description **"RED, RED, RED, RED"** hinted at all **4 RGBA channels** — not just the red one
- When all pixel values are only two numbers, it's a strong sign of **1-bit-per-channel steganography**
- Combining bits from multiple channels (RGBA = 4 bits per pixel) can encode a full bitstream
- Always check if the resulting bitstream is **Base64** — it's a very common encoding layer in CTFs
- `exiftool` and `strings` are good first steps but pixel-level analysis is needed for steganography

---

## 🚀 6. Improvements for Next Time

- When a challenge description repeats a word (like "RED, RED, RED, RED"), count the repetitions — it often hints at the number of channels or layers
- Always print **unique pixel values** early — if only 2 values exist per channel, it's binary encoding
- Check all channels (R, G, B, A) independently and in combination
- When you get a decoded bitstream of printable characters ending in `==`, immediately try Base64 decoding
- Use `img.mode` to check if the image has an **alpha channel** (RGBA) — it's often used for hiding data

---

## 🔗 7. References

- [PIL/Pillow Image Documentation](https://pillow.readthedocs.io/en/stable/)
- [CTF Steganography Techniques](https://ctf101.org/forensics/what-is-stegonagraphy/)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [LSB Steganography Explained](https://en.wikipedia.org/wiki/Steganography#Digital_steganography)
- [picoCTF Forensics Challenges](https://picoctf.org)