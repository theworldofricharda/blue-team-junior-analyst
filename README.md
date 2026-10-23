<div align="center">

# 🛡️ Blue Team Junior Analyst Pathway

### Hands-on lab writeups from the Security Blue Team Junior Analyst pathway. Method, not answers.

![Pathway](https://img.shields.io/badge/Pathway-6%2F6%20Complete-2ea44f)
![Provider](https://img.shields.io/badge/Security%20Blue%20Team-BTJA-004c6d)
![Writeups](https://img.shields.io/badge/Writeups-Spoiler--safe-1f6feb)
![Lab](https://img.shields.io/badge/Environment-Isolated%20Home%20Lab-8957e5)
![License](https://img.shields.io/badge/License-CC--BY--4.0-brightgreen)

![Kali Linux](https://img.shields.io/badge/Kali%20Linux-557C94?logo=kalilinux&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?logo=wireshark&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-4682B4?logoColor=white)
![Nessus](https://img.shields.io/badge/Nessus-00A4E4?logoColor=white)
![Tor](https://img.shields.io/badge/Tor-7D4698?logo=torproject&logoColor=white)

<!-- Banner / demo placeholder: drop a banner image or a short GIF of a writeup here -->
<!-- ![banner](assets/banner.png) -->

</div>

---

## 🚀 Overview

This is my working portfolio for the Blue Team Junior Analyst pathway, six introductory courses covering the core defensive skills expected of a junior SOC analyst. I read the material, then worked every quiz and capstone hands-on in my own lab rather than just following along. Each writeup here records how I approached the work, the tools I used, and what I took from it.

> [!NOTE]
> These are **methodology** writeups, not answer keys. I have left out the specific challenge answers, flags, decoded values, and findings on purpose, so anyone else taking the courses still does the work themselves. You will find the how, not the what.

> [!TIP]
> Hands-on exercises were run against intentionally vulnerable practice targets or provided training material in an **isolated lab**. The OSINT work is the one exception: it is passive collection from public third-party sources (certificate transparency logs, passive DNS, search and archive indexes), so nothing was sent to any target system. Real addresses, hostnames, account names, and file paths have been removed throughout, and target names are withheld. Nothing here points at a live system.

### Key features

- 🧠 **Six defensive domains** covered end to end, from packet analysis to vulnerability management.
- 🔒 **Spoiler-safe** by design, method and lessons, never the answers.
- 🧽 **Fully sanitized**, no real addresses, hostnames, account names, or file paths.
- 🧪 **Self-built lab**, every hands-on exercise run in my own isolated virtualisation lab.
- 🎯 **Mapped to SOC skills**, each writeup ties back to what the role actually asks for.

---

## 🛠️ Tech Stack & Tools

| Domain | Tools |
|---|---|
| Packet analysis | Wireshark, tcpdump |
| OSINT | passive enumeration, archive recovery, username enumeration, data decoding |
| Digital forensics | command-line file analysis, archive password recovery, steganography |
| Dark web / data recovery | Tor, metadata and image analysis, encoding and decoding |
| Threat hunting | IOC generation, hashing, string analysis, scoped hunting |
| Vulnerability scanning | Nmap, Nessus, OpenVAS, WPScan |
| Lab and virtualisation | Linux, KVM, isolated lab networking, intentionally vulnerable VMs |

---

## 🗂️ How to Use This Repo

Each folder holds the spoiler-safe writeup for one course, plus any reference I built along the way. Read them in order for the full pathway, or jump to a domain you care about.

```
.
├── README.md
├── 01-network-analysis/
├── 02-osint/
├── 03-digital-forensics/
├── 04-dark-web-operations/
├── 05-threat-hunting/
└── 06-vulnerability-management/
```

---

## 📚 What Each Writeup Covers

### 1. 📡 Network Analysis
Reading packet captures to reconstruct what happened on a network and to spot malicious traffic. **Takeaway:** if you know what normal looks like, the abnormal stands out, and a capture is often the ground truth when logs disagree.

### 2. 🔎 OSINT
Passive reconnaissance and identity investigation from public information only. **Takeaway:** OSINT is as much disciplined rejection as collection. A match is not a confirmation.

### 3. 🧬 Digital Forensics
Examining a disk image to uncover how data was hidden and moved. **Takeaway:** extensions lie, so inspect the real file type, and staging before exfiltration is an early warning sign.

### 4. 🧅 Dark Web Operations
A sanctioned, lab-hosted investigation of a mock hidden-service site. **Takeaway:** treat anything from an untrusted source as hostile, and client-side secrets are not secrets.

### 5. 🏹 Threat Hunting
Turning malware samples into indicators of compromise, then hunting an endpoint for them. **Takeaway:** a hunt needs more than hashes. A tampered copy changes its hash and slips past, and only content indicators like strings expose it. Combine hash, size, name, and string.

### 6. 🧰 Vulnerability Management
Finding, scoring, reporting, and remediating weaknesses. **Takeaway:** the CVSS number is a guide, not a verdict. A medium issue under active exploitation can outrank a critical one that is not.

---

## 🤝 Contributing

This is a personal learning portfolio, so I am not taking contributions. If you spot a genuine technical error in a method, feel free to open an issue, that kind of correction is always welcome.

## 📄 License

Released under **CC BY 4.0**. Use and share freely with attribution. See `LICENSE`.

---

<div align="center">

**Worked and written by Akintomide R F aka Syntaxfoxx** · October 2026
Hands-on exercises carried out against intentionally vulnerable practice targets in an isolated home lab.
OSINT work was passive, public-source collection only.

</div>
