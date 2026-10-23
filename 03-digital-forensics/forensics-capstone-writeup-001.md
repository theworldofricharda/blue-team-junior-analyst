# Digital Forensics Writeup: Disk Image Exfiltration Investigation

**Author:** Akintomide R F aka Syntaxfoxx
**Date:** 22 October 2026
**Category:** Hands-on lab, digital forensics and insider-threat investigation
**Context:** Security Blue Team, Introduction to Digital Forensics (course capstone)
**Tools used:** Linux CLI (find, ls -a, file, strings, grep), fcrackzip, steghide, rockyou.txt

> This writeup documents the investigative method, not the specific challenge answers, so it stays
> clean to publish and does not spoil the exercise for others.

---

## Scenario

A SOC received an anonymous report that an employee was exfiltrating company data. A forensic image
of the user's drive was captured, and the task was to analyze a copy and uncover four separate pieces
of hidden evidence. Each piece was concealed with a different technique, so the challenge exercised a
full range of skills: command-line navigation, spotting incorrect file extensions, finding hidden
files and folders, steganography, and password cracking.

---

## Methodology: sweep first, then target

Rather than clicking through folders at random, I began with a structured triage that surfaces
anomalies quickly:

1. **Full recursive listing including hidden files** (`find`, `ls -a`), to map every file and catch
   dot-prefixed files and folders that a normal listing hides.
2. **True file-type check on everything** (`file`), which reads the magic bytes and exposes files
   whose extension was changed to disguise them.
3. **Content scan** (`grep -r`, `strings`), to pull readable clues out of text and binary files and
   to locate the evidence markers.

That sweep immediately flagged the suspicious items: files whose real type did not match their
extension, a hidden password-protected archive, and an email in the user's saved mail that pointed
the way to the rest.

---

## The four techniques, chained together

What made this a realistic end-to-end investigation is that each piece of evidence required a
different skill:

- **Hidden file plus password cracking.** A dot-prefixed archive, invisible to a normal directory
  listing, was password protected. I cracked it with `fcrackzip` in dictionary mode against the
  `rockyou.txt` wordlist and recovered a staged dump of employee personal data, including account
  identifiers.
- **Steganography.** A passphrase clue was planted as readable text inside an image in the
  saved-email folder. Using that passphrase with `steghide`, I extracted a hidden file (a credential
  list) embedded inside an ordinary-looking photo. The empty passphrase failed first, which confirmed
  the data was passphrase protected and sent me looking for the clue rather than guessing.
- **Incorrect file extension on an image.** A file carrying a data-format extension was actually a
  picture when checked with `file`. Opening it revealed sensitive information rendered inside the
  image, invisible to a text search.
- **Incorrect file extension on a text file.** A file disguised with a web-asset extension was really
  plain text, and reading it exposed an incriminating note.

---

## Reconstructed narrative

Pieced together, the four findings told a clear insider-threat story. The user had collected employee
personal data and credentials, staged them in hidden and disguised files, and was moving sensitive
material off a work machine to a personal external email address. The classic markers were all there:
data collected and archived, obfuscation through both file extension and steganography, and
exfiltration over personal email.

---

## Blue team takeaways

The defensive lessons this investigation reinforces:

- **Extensions lie, so inspect content.** Detection and data-loss-prevention tooling should classify
  files by true type (magic bytes), not by extension, or disguised files pass straight through.
- **Watch for staging and archiving.** Data gathered into archives, especially password-protected
  ones, before any movement is a strong exfiltration precursor.
- **Monitor egress to personal accounts.** Sending sensitive files to personal, external addresses is
  the exfiltration channel in this case, and a prime thing to alert on.
- **Steganography evades keyword inspection.** Payloads hidden inside images defeat text-based DLP,
  which is why egress volume, destination, and file-entropy signals matter alongside content matching.

---

## MITRE ATT&CK mapping

- **T1005** Data from Local System
- **T1560** Archive Collected Data (the password-protected archive)
- **T1027** Obfuscated Files or Information (wrong extensions and steganography)
- **T1048 / T1567** Exfiltration over an alternative protocol or web service (personal email)

---

## Skills demonstrated

Structured forensic triage of a disk image, identifying files by true type rather than extension,
uncovering hidden files and folders, cracking password-protected archives with a wordlist, extracting
steganographically hidden data, correlating separate findings into a single coherent incident
narrative, and translating the result into defensive detection and DLP recommendations.

---

*Part of my Blue Team Junior Analyst pathway portfolio. The disk image was provided training
material, analysed in an isolated lab. Specific answers, filenames, and recovered values are
withheld to keep the exercise intact for others.*
