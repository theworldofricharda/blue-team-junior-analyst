# Threat Hunting Writeup 001

**Author:** Akintomide R F aka Syntaxfoxx
**Category:** Hands-on lab, IOC-driven threat hunting
**Context:** Security Blue Team, Introduction to Threat Hunting (course capstone)
**Tools used:** IOC tooling (OpenIOC), hashing, string analysis, scoped host hunting

> This is a methodology writeup. I have left out the specific samples, hashes, strings, filenames, and counts on purpose, so anyone else taking the course still does the work. The samples were provided training material, which I handled on Linux where Windows malware cannot run, so it stayed inert data for analysis.

---

## Scenario

The work is about turning what the threat intelligence team hands you into something you can search for, then hunting an endpoint for any trace of it. I was given malware samples and a short intelligence note, asked to build a full set of indicators of compromise from them, and then to search a specific user's profile for matches and report what I found.

## Building the indicators

A good IOC set uses more than one kind of indicator, because each kind catches a different thing:

1. **Hashes (MD5 and SHA1).** A file's fingerprint. An exact match is high confidence, and it holds even if the file has been renamed.
2. **File size.** A cheap, fast supporting indicator that pairs well with a hash.
3. **File name.** Weak on its own, since names are trivial to change, but useful alongside the stronger indicators.
4. **Strings.** Distinctive text baked into the sample. This is the one that matters most when a file has been altered.

The intelligence note also carried extra indicators the analysts had already pulled out, which is a reminder to always read the brief fully before starting. Missing the note means missing part of the hunt.

## The hunt

I scoped the search to the user's own directories rather than the whole system, exactly as the intelligence suggested, which keeps a hunt fast and focused instead of scanning everything. I then matched every file against the full indicator set, hash, size, name, and strings, and recorded which indicator caught each hit.

## The key finding, in general terms

The results made one lesson concrete. Some of the planted files were untouched copies, and the hash indicators caught those cleanly, even the ones that had been renamed, because a hash does not care about the file name. Other files had been deliberately altered, so their hash and size no longer matched the samples at all. Hashes and sizes slid straight past those. The only thing that exposed them was the string indicators, the distinctive text still sitting inside the modified files.

So a hunt built on hashes alone would have found only a fraction of what was there.

## Lessons I took from it

- **Never hunt on hashes alone.** A hash is precise but brittle. The smallest change to a file breaks the match, and attackers change files on purpose. Combine hash, size, name, and strings so a tampered copy still trips something.
- **Strings survive tampering.** Content-based indicators are what catch a variant that has been altered to dodge hash matching.
- **Read the whole brief.** The intelligence note held indicators I would have missed if I had worked only from the samples.
- **Scope the hunt.** Limiting the search to where the activity is expected keeps it fast and keeps the noise down.

## Blue team angle

This is the daily loop of hunting on shared intelligence: take the indicators a threat intel team provides, search your estate for any presence of them, and report cleanly on what you find. The broader value is understanding why a layered indicator set beats a single strong one, which is the same reasoning behind good detection engineering. I mapped the activity to the relevant MITRE ATT&CK techniques and noted detection ideas for each.

## Skills demonstrated

Generating a full IOC set (hash, size, name, strings) from samples and threat intelligence, handling samples safely, scoping a hunt to a likely location, matching indicators against a host, reasoning about why layered indicators beat single ones, and reporting findings mapped to MITRE ATT&CK.

---

*Part of my Blue Team Junior Analyst pathway portfolio. Samples were provided training material, handled in an isolated environment. No real malware detail, system, or person is referenced here.*
