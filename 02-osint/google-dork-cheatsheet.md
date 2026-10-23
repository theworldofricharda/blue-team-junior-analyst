# Google Dork Cheatsheet (OSINT Reference)

**Author:** Akintomide R F aka Syntaxfoxx

A quick reference for Google search operators ("dorks") used in open-source intelligence and reconnaissance.

> Use only against targets you own or are explicitly authorized to assess. Google dorking reaches
> data that is publicly indexed, but accessing or acting on exposed data you have no authorization
> for can still be illegal. For learning, point these at your own domain, deliberately vulnerable
> practice sites, or during an authorized engagement.

---

## 1. Core search operators

| Operator | What it does | Example |
|---|---|---|
| `intext:` | Word appears in the page body | `intext:"login"` |
| `allintext:` | All words appear in the body | `allintext:username password` |
| `inurl:` | Word appears in the URL | `inurl:admin` |
| `allinurl:` | All words appear in the URL | `allinurl:admin login` |
| `intitle:` | Word appears in the page title | `intitle:"index of"` |
| `allintitle:` | All words appear in the title | `allintitle:restricted files` |
| `site:` | Limit results to one domain | `site:example.com` |
| `filetype:` / `ext:` | Limit to a file extension | `filetype:pdf` |
| `cache:` | Show Google's cached copy of a page | `cache:example.com` |
| `related:` | Find sites similar to one | `related:github.com` |
| `link:` | Pages linking to a URL (largely deprecated) | `link:example.com` |
| `inanchor:` | Word in the anchor text of links | `inanchor:"click here"` |
| `allinanchor:` | All words in anchor text | `allinanchor:download now` |
| `numrange:` | Numbers within a range | `numrange:1000-2000` |
| `before:` / `after:` | Filter by date | `after:2024-01-01 before:2025-01-01` |
| `AROUND(n)` | Two terms within n words of each other | `"security" AROUND(3) "breach"` |
| `inpostauthor:` | Blog posts by an author (Blogger) | `inpostauthor:"john doe"` |
| `define:` | Dictionary definition | `define:reconnaissance` |

---

## 2. Logic and symbols

| Symbol | Meaning | Example |
|---|---|---|
| `"..."` | Exact phrase match | `"confidential report"` |
| `OR` or `\|` | Either term | `site:example.com (admin \| login)` |
| `AND` or `&` | Both terms (AND is default) | `admin AND password` |
| `( )` | Group operators together | `site:example.com (filetype:pdf OR filetype:docx)` |
| `-` | Exclude a term or site | `site:example.com -www` |
| `+` | Force-include a term | `+intitle:index` |
| `*` | Wildcard for an unknown word | `"how to * a server"` |
| `~` | Include synonyms (legacy) | `~hacking tutorial` |

**Tip:** combine operators. `site:example.com filetype:pdf intext:"internal use only"` scopes to one
domain, one file type, and one phrase at once. Stacking operators is where dorking gets powerful.

---

## 3. OSINT recon dorks by goal

### Mapping a target's footprint
```
site:example.com                      # everything Google indexed for the domain
site:*.example.com                    # subdomains
site:example.com -www                 # non-www hosts (often dev/staging/apps)
site:example.com (inurl:admin OR inurl:login OR inurl:portal)
```

### Finding documents
```
site:example.com filetype:pdf
site:example.com (filetype:xls OR filetype:xlsx OR filetype:csv)
site:example.com (filetype:doc OR filetype:docx)
site:example.com filetype:pptx
site:example.com ext:txt intext:"password"
```

### Exposed directory listings
```
intitle:"index of" site:example.com
intitle:"index of" "parent directory"
intitle:"index of" (backup OR bak OR old)
intitle:"index of" "*.sql"
```

### Config, env, and secret files
```
site:example.com ext:env
site:example.com ext:ini OR ext:conf OR ext:cnf
site:example.com ext:log
site:example.com ext:sql OR ext:bak OR ext:old
site:example.com intext:"BEGIN RSA PRIVATE KEY"
```

### Login and admin interfaces
```
site:example.com (inurl:admin OR inurl:wp-admin OR inurl:administrator)
intitle:"login" site:example.com
inurl:"/phpmyadmin/" site:example.com
```

### Error messages that leak detail
```
site:example.com intext:"sql syntax near"
site:example.com intext:"Warning: mysql_"
site:example.com intext:"Fatal error" intext:"on line"
```

### Code and credential leaks (search these on code hosts, not the target)
```
site:github.com example.com password
site:pastebin.com example.com
site:gitlab.com "example.com" (token OR api_key OR secret)
```

### Exposed devices and services (use with extreme care, authorized only)
```
intitle:"webcamXP" OR intitle:"Live View / - AXIS"
inurl:"ViewerFrame?Mode="
intitle:"index of" "config.php"
```

---

## 4. People / account OSINT (SOCMINT)
```
"John Doe" site:linkedin.com
"john.doe@example.com"                # exact email across the web
intext:"john.doe@example.com" (filetype:pdf OR filetype:xlsx)
site:twitter.com OR site:x.com "username"
"John Doe" (resume OR cv) filetype:pdf
```

---

## 5. Workflow notes

- **Start broad, then narrow.** `site:example.com` first, then layer `filetype:`, `inurl:`, `intext:`.
- **Pair with the resource you already have.** After `theHarvester` gives you subdomains, dork each
  interesting one: `site:staging.example.com`.
- **Google is not the only engine.** The same logic (with slightly different syntax) works on Bing,
  DuckDuckGo, and Yandex. Yandex is notably strong for image and some regional results.
- **Automated dorking exists but gets you rate-limited or CAPTCHA'd fast.** For course learning,
  run queries by hand in the browser.
- **For your own exposure:** run the sensitive dorks against your own domains and accounts first.
  That is the defensive half of OSINT, and it is exactly what the "Securing Yourself Online" section
  is about.

---

*Reference compiled for OSINT study. Operators are public Google search syntax. Based on the widely
circulated community cheatsheet by sundowndev, reorganized and extended.*
