# 🧩 CTF Challenge Writeup  
**Challenge Name:** Ph4nt0m 1ntrud3r  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2025  
**Author:** PRINCE NIYONSHUTI N.  
**Date:** 24-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a PCAP network capture file with the following description:

> *"A digital ghost has breached my defenses, and my sensitive data has been stolen! Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag. To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner."*

A PCAP file `myNetworkTraffic.pcap` is provided for download.  
The hint **"Attacks were done in timely manner"** strongly suggests that the flag is hidden across multiple packets and must be **reassembled by sorting packet timestamps**.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category  
- **Easy** difficulty  
- A PCAP file with network traffic to analyze
- Hint: **"Attacks were done in timely manner"** → timestamps matter

Key observations:
- The PCAP contains only **22 packets** — small and manageable
- All packets are TCP from `192.168.0.2` to `192.168.1.2` on port 80
- Each packet has a **TCP payload** containing hex-encoded data
- The hex payload decodes to **ASCII Base64 strings**
- Each Base64 string decodes to a **fragment of the flag**
- The fragments are in **scrambled packet order** but correct **timestamp order**
- Sorting by timestamp and filtering printable decoded strings reveals the full flag

The intended approach is to **extract TCP payloads**, decode hex → Base64 → ASCII, then **sort by timestamp** to assemble the flag.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Inspect the PCAP
```bash
wget <challenge-url> -O myNetworkTraffic.pcap
capinfos myNetworkTraffic.pcap
```

Output shows only **22 packets**, all TCP, captured over ~6ms.

### 3.2 Extract Packet Details
```bash
tshark -r myNetworkTraffic.pcap -T fields -e frame.number -e frame.time_delta -e tcp.payload 2>/dev/null
```

Each packet has a hex payload — but `strings` and `grep picoCTF` find nothing directly.

### 3.3 Decode Payloads with Python
```python
import dpkt

with open('myNetworkTraffic.pcap', 'rb') as f:
    pcap = dpkt.pcap.Reader(f)
    for i, (ts, buf) in enumerate(pcap):
        ip = dpkt.ip.IP(buf)
        tcp = ip.tcp
        print(f"Pkt {i+1}: payload={tcp.data.hex()}")
```

Each payload is a hex string that decodes to an ASCII Base64 string.

### 3.4 Decode Hex → Base64 → ASCII
```python
import dpkt
import base64

with open('myNetworkTraffic.pcap', 'rb') as f:
    pcap = dpkt.pcap.Reader(f)
    for i, (ts, buf) in enumerate(pcap):
        ip = dpkt.ip.IP(buf)
        tcp = ip.tcp
        ascii_str = tcp.data.decode('ascii')        # hex bytes → ASCII Base64
        decoded = base64.b64decode(ascii_str).decode('ascii', errors='replace')
        print(f"Pkt {i+1}: {decoded}")
```

Output shows flag fragments scattered across packets:
```
Pkt 3:  bh_4r_a
Pkt 5:  nt_th4t
Pkt 7:  picoCTF
Pkt 14: _34sy_t
Pkt 16: }
Pkt 22: {1t_w4s
```

### 3.5 Sort by Timestamp to Assemble Flag
```python
import dpkt
import base64

with open('myNetworkTraffic.pcap', 'rb') as f:
    pcap = dpkt.pcap.Reader(f)
    packets = []
    for ts, buf in pcap:
        try:
            ip = dpkt.ip.IP(buf)
            tcp = ip.tcp
            ascii_str = tcp.data.decode('ascii')
            decoded = base64.b64decode(ascii_str).decode('ascii', errors='replace')
            packets.append((ts, decoded))
        except:
            pass

# Sort by timestamp
packets.sort(key=lambda x: x[0])

# Filter only fully printable fragments
flag_parts = [p[1] for p in packets if all(32 <= ord(c) <= 126 for c in p[1])]
print(''.join(flag_parts))
```

Output:
```
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_af160980}
```

---

## 🧩 4. Final Flag

```
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_af160980}
```

---

## 📚 5. Key Learnings

- **TCP payloads** can carry encoded data that looks like garbage until properly decoded
- A multi-layer encoding chain (hex → Base64 → ASCII) is a common obfuscation technique in CTFs
- The hint **"timely manner"** meant sorting by **packet timestamp** — packet numbers were scrambled deliberately
- `dpkt` is a powerful Python library for parsing PCAP files at the packet level
- Always filter for **fully printable strings** when assembling flag fragments — non-printable decoded data is noise
- Small PCAPs (22 packets) are often completely hand-crafted for CTFs — every packet is intentional

---

## 🚀 6. Improvements for Next Time

- For PCAP challenges, always try `strings | grep picoCTF` first — if nothing found, dig into packet payloads
- Use `dpkt` to extract raw TCP/UDP payloads programmatically
- When payloads look like hex strings, decode them to ASCII first — they're often Base64
- Pay attention to hints about **timing** — timestamps are a common way to order scrambled data
- Use `capinfos` early to understand the PCAP size and protocol — 22 packets means every packet matters
- Always sort by timestamp when the challenge mentions "timely" or "timing"

---

## 🔗 7. References

- [dpkt Documentation](https://dpkt.readthedocs.io/en/latest/)
- [Wireshark/tshark Documentation](https://www.wireshark.org/docs/)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [PCAP File Format](https://wiki.wireshark.org/Development/LibpcapFileFormat)
- [picoCTF Forensics Challenges](https://picoctf.org)