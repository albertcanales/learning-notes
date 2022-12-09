[Advent Of Cyber 2022 - TryHackMe](https://tryhackme.com/room/adventofcyber4)

## Day 1 - Frameworks

Common Security Frameworks:

1. NIST
2. ISO 27000s
3. MITRE ATT&CK
4. Cyber Kill Chain
5. Unified Kill Chain (mix of 3 and 4)
	- In: Gaining access to the system
	- Through: Increasing priviledges and obtaining information
	- Out: Get results and use them

## Day 2 - Log Analysis

A *SIEM* tool allows a simpler and more profound log analysis, but may be difficult to configure or learn to use. 

## Day 3 - OSINT

Common types of OSINT for SW:

- Google Dorks
- WHOIS lookup (not really useful)
- Robots.txt
- Breached DB Search (haveibeenpwned and such)
- OSS public repositories

## Day 4 - Scanning

In port scanning, 3 results possible:

- Closed
- Opened
- Filtered

*Nmap* can be used for:

- Port Scanning with `-Ss`
- OS Detection with `-O`
- Ping Scanning with `-Sn`
- Service Scanning with `-Sv`

*Nikto* can be used to find common vulnerabilities on websites

## Day 5 - Brute-Forcing

Common Remote Access protocols:

- SSH (cli)
- RDP (windows gui)
- VNC (linux gui)

Authentification can be done using some thing you...

- Know (ex: password)
- Have (ex: RFID key)
- Are (ex: fingerprint)

*Wordlists* are list of common passwords (from leaked DBs)

A hydra command will usually look like so:

	hydra -l USER -P WORDLIST SERVER SERVICE

