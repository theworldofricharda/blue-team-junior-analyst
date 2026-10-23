# OSINT Reconnaissance and Investigation Writeup 001

**Author:** Akintomide R F aka Syntaxfoxx
**Date:** 21 October 2026
**Category:** Hands-on lab, open-source intelligence (OSINT) and passive reconnaissance
**Context:** Security Blue Team, Introduction to OSINT course (practice work)
**Tools used:** theHarvester, maigret, Wayback Machine, Google DNS-over-HTTPS, Base64 decoding, Google dorking

> Scope note: everything here is passive open-source collection. Nothing was sent to any target
> system: the enumeration reads third-party sources (certificate transparency logs, passive DNS,
> search and archive indexes) rather than touching the domain itself. No credentials were attacked
> and no real individual was profiled. Target names and specific results are withheld, so this is a
> record of method rather than of anyone's infrastructure. Where a username collision pointed at a
> real person, that record was discarded (see the false-positive section).

---

## Overview

This entry documents two pieces of OSINT practice. The first is passive subdomain enumeration of a
public domain with theHarvester. The second is an identity-investigation methodology worked against
a fabricated training persona, which demonstrates the full pivot chain an analyst uses to move from
a single username to a subject's wider footprint, including the judgment needed to reject false
positives. The specific persona findings are intentionally left out, as they belong to a training
provider's challenge; the value here is the repeatable method.

---

## Part 1: Passive subdomain enumeration with theHarvester

**Goal:** map a domain's public footprint without sending any traffic to the target itself.

**Command** (the domain I ran this against is withheld, and the results below are described in
general terms, so this writeup does not publish another organisation's infrastructure detail):
```bash
theHarvester -d <target-domain> -b crtsh,certspotter,duckduckgo,hackertarget,rapiddns,otx,urlscan,subdomaincenter,waybackarchive -f harvest_output
```

**Result:** a few hundred hosts surfaced from certificate transparency logs, passive DNS databases,
and search-engine sources. Key analytical takeaways:

- **CDN mapping.** Most hosts resolved into a single CDN provider's ranges, on both IPv4 and IPv6.
  Those are shielded by the CDN, so the real origin is not directly reachable.
- **Exposed origin servers.** A smaller set resolved to cloud-hosting ranges outside the CDN, which
  reveals the actual back-end machines. These are the higher-value targets in an assessment and the
  assets a defender would want kept behind the CDN.
- **Naming convention as intelligence.** Hostnames followed a consistent themed naming scheme rather
  than arbitrary labels. A predictable convention lets an analyst guess and probe for names that did
  not appear in passive results, so it is worth noticing as a finding in its own right.
- **Signal versus noise.** Several results were scraping artifacts rather than real subdomains, for
  example mangled domain concatenations and URL-encoded fragments that leaked into hostnames.
  Discarding these false positives is the core analyst skill, reporting them as findings would
  destroy credibility.

**Defensive angle:** the same enumeration shows a blue team exactly what an attacker can learn
passively, which drives decisions about what to place behind a CDN, which staging hosts to take
offline, and what to monitor in certificate transparency logs.

---

## Part 2: Identity investigation methodology (pivot chain)

Worked against a fabricated training persona. The point is the method, not the subject.

1. **Start from one identifier.** A single social-media handle is the seed.
2. **Decode obfuscated data in every field, not just the obvious one.** The profile's linked website
   was Base64 encoded; decoding it (`echo '<string>' | base64 -d`, or a tool like CyberChef) revealed
   the real domain. The important lesson here is thoroughness: encoded clues also hide in non-obvious
   fields such as a profile's location, bio, or a secondary account's metadata, and they may be hex
   rather than Base64. Each encoded value can open a separate branch of the trail, so every field must
   be checked. Missing one encoded field means missing a whole branch, which is exactly how an analyst
   can wrongly conclude a trail has gone cold.
3. **Recover dead infrastructure with the Wayback Machine.** The decoded site was offline, but the
   Internet Archive held multiple captures. Pulling archived snapshots recovered the original site
   structure that live browsing could not.
4. **Enumerate the username across platforms.** A username search tool (maigret, 3000+ sites)
   mapped where else the handle existed, surfacing a blog and other accounts in seconds.
5. **Reject false positives (the most important step).** The username tool also matched the handle on
   a Russian social network, but that account belonged to a real, unrelated individual with a
   different name, age, city, and interests (a gaming profile). A username match is not an identity
   confirmation. That record was discarded rather than attributed to the subject. Treating a
   collision as a finding would both fail the analysis and wrongly implicate a real person.

**Key lesson:** OSINT is as much about disciplined rejection as collection. Confidence must be tied
to corroboration across independent sources, not to a single username hit.

**Lesson learned (process correction):** in my first pass I concluded part of the trail had decayed
and that some fields were unsourceable. That conclusion was premature. A second encoded clue, hidden
in a profile field I had not decoded, pointed to further accounts that carried the missing details.
The correct takeaway is to exhaust every field for encoding before declaring a lead dead, and to
verify findings against an independent reference where one exists. Revising a conclusion when new
evidence appears is part of good analysis, not a failure of it.

---

## Part 3: Google dorking reference

Compiled a Google dork cheatsheet (operators, logic, and recon queries grouped by goal) as a
standing reference for scoped searches, document discovery, exposed-file checks, and people OSINT.
Stored separately as `google-dork-cheatsheet.md`. Intended for authorized use and for checking my
own exposure, which is the defensive half of OSINT.

---

## Skills demonstrated

Passive subdomain enumeration from certificate transparency and passive DNS, separating real assets
from scraping artifacts, distinguishing CDN-fronted hosts from exposed origins, reading a naming
convention as intelligence, decoding Base64 and hex, recovering decommissioned web infrastructure
through the Internet Archive, username enumeration across platforms, DNS TXT inspection over
DNS-over-HTTPS, and, above all, the analytical discipline to reject username collisions and avoid
misattributing a real person.

---

*Part of my Blue Team Junior Analyst pathway portfolio. All collection was passive and open-source.
Target names and specific findings are withheld, both to keep the training exercise intact for
others and to avoid publishing any organisation's infrastructure detail.*
