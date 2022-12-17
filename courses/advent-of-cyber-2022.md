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

# Day 6 - Email Analysis

- *emlAnalyser*: Parse mail data for a better view of the contents
- emailrep.io / ipinfo.io / Talos Reputation: Get reputation of mail server or IP
- Browserling / Wannabrowser: Sandboxes browsers for opening links
- VirusTotal / InQuest Labs: attachment analysis (by upload, link or hash)

# Day 7 - CyberChef

*Cyberchef* is a GUI tool for deep analysis and parsing of data or files. Has lots of options and, for well-known operations, it is quite fast compared to writing custom scripts.

*Defanfing* an URL means making it unclickable.

# Day 8 - Smart Contracts

> I did not take notes on this one as it is not one of my interests.

# Day 9 - Pivoting

*/.dockerenv* is a file present in Docker containers. Useful for checking if a compromised application is running inside one.

Some commands in the Metasploit console:

	search MODULE
	use MODULE

	# Once a module is used...
	info (description, options, CVE, etc)
	show options
	set OPTION VALUE
	check 		# not always available
	run

When running a module, a session may be opened. The commands for managing sessions are:

	# show sessions
	sessions

	# upgrade the session to Meterpreter
	sessions -u SESSION_ID

	# open (interact) with a session
	session -i SESSION_ID

	# *once in the session*, go back to the ms console
	background

Meterpreter allows running more complex commands on top of the normal command line, for example:

	# get system information (OS...)
	sysinfo

	# upload a local file to the remote machine
	upload SRC TARGET

	# display interfaces
	ipconfig

	# resolve a host
	resolve HOST


In order to pivot through the network, one can use a Meterpreter session in combination with MS internal routing table.

	# Example usage
	route [add/remove] subnet netmask [comm/sid]

	# Output the routing table
	route print

This greatly facilitates pivoting from the attacker's machine through the Meterpreter session.


*Socks Proxy* is used to channel the traffic between the attacker and host machine. It can be used in combination with proxychains with any command, but then it has to be on port 9050 (on the attacker's side) by default.


Some interesting modules (partial names):

- ignition_laravel
- postgres_schemadump
- postgres_sql
- socks_proxy
- ssh_login
