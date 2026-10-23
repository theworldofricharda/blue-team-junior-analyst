# Network Analysis Writeup 001

**Author:** Akintomide R F aka Syntaxfoxx
**Category:** Hands-on lab, packet capture analysis
**Context:** Security Blue Team, Introduction to Network Analysis (course capstone)
**Tools used:** Wireshark, tcpdump

> This is a methodology writeup. I have left out the specific challenge answers, the capture's addresses, ports, and filenames on purpose, so anyone else taking the course still works it out themselves. Everything was done against provided training captures in a lab.

---

## Scenario

The course works through reading packet captures to understand what happened on a network, first with ordinary traffic to get fluent in the tools, then with captures from a compromised system where I had to reconstruct an intrusion end to end. The capstone was a traffic investigation where I traced an attack from first contact through to its goal using only the packets.

## Method

I worked captures the same way each time, which keeps the analysis repeatable:

1. **Get the shape of the traffic first.** Before filtering for anything specific, I used the Statistics views (protocol hierarchy, conversations, and endpoints) to see which hosts talked to which, which protocols were in play, and who moved the most data. That overview tells you where to look before you start looking.
2. **Filter down to the question.** Wireshark display filters narrow a capture to exactly the traffic that answers a given question, a protocol, a port, a host pair, or a message type. Writing the filter forces you to be precise about what you are actually asking.
3. **Follow the conversation.** Following a stream reassembles a back-and-forth into readable order, which is how you see a command channel or a file transfer as a sequence rather than scattered packets.
4. **Extract the evidence.** Where files or payloads crossed the wire, I pulled them out of the capture so they could be examined directly, and I used tcpdump with targeted filters for quick command-line passes over the same data.

## The two investigations

**Reconstructing an intrusion.** From a capture of a compromised host I rebuilt the attacker's sequence: the early reconnaissance, how they got a foothold, the channel they used to control the host, and what they moved onto it afterwards. The point was not any single packet but the order, seeing the whole operation as one story told by the traffic.

**An ARP poisoning man-in-the-middle.** The capstone centred on a machine-in-the-middle attack carried out through ARP poisoning. The giveaway is in the ARP traffic itself: one machine answering for addresses that are not its own, so that traffic meant for other hosts is quietly routed through the attacker first. Spotting that pattern is what exposes the attack.

## Lessons I took from it

- **Know normal, and the abnormal stands out.** Most of the skill is being fluent enough with ordinary traffic that the odd conversation or the out-of-place protocol catches your eye.
- **Start wide, then narrow.** The statistics views point you at the interesting hosts and protocols before you commit to a filter. Diving straight into display filters without the overview wastes time.
- **The capture is ground truth.** When logs are missing, incomplete, or disagree, the packets are what actually happened on the wire. That makes capture analysis a strong fallback during an investigation.
- **ARP is trust without proof.** ARP has no authentication, so a host can simply claim an address. Watching for a single MAC answering for several IPs is a practical way to catch poisoning.

## Blue team angle

The same workflow maps straight onto detection work: baseline what normal looks like, alert on the protocol and conversation patterns that signal reconnaissance, command channels, or spoofing, and keep targeted captures as evidence for an investigation. I mapped the findings to the relevant MITRE ATT&CK techniques and noted where a detection rule would sit for each stage.

## Skills demonstrated

Reading and filtering packet captures, using protocol and endpoint statistics to triage traffic, following and reassembling conversations, extracting transferred files and payloads, recognising an ARP poisoning man-in-the-middle from its traffic pattern, reconstructing an intrusion from first contact to objective, and turning the analysis into MITRE-mapped detection ideas.

---

*Part of my Blue Team Junior Analyst pathway portfolio. All analysis was done against provided training captures in a lab. No real network or system is referenced.*
