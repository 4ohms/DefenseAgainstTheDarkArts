# Thursday, January 18th: Course Introduction
* This is my favorite course
* A little exercise:
	- When you hear the term "cyber security", list some topics, issues, or things that come to mind. Free-for-all
* What this course is NOT
* What this course IS and expectations
* The schtick is very different in this course, and this may be arguably the most "different" course you will take in the CS curriculum
* The definition of security in the context of technology, information, computers
* Terminology: Is it Computer Security?  Is it Information Security (infosec)?  Is it Cyber Security?
* Why study security?
	- So how far have we come? (or lack thereof)
	- The Tacoma Bridge Failure https://www.youtube.com/watch?v=XggxeuFDaDU
* One outcome of this course: question and debate everything
* What's my ultimate goal for you in this course?  We'll revisit this on the last day of class.
* The basics you all absolutely need to have: experience and comfort with the command line
  - Why? Versatility, productivity, accessibility, scripting
  - Not everything can be accomplished by fancy graphics and GUIs (sometimes constrained to certain features too)
  - Many systems do not use a windows manager or have a graphical desktop interface (e.g., servers)
    - Graphical interface (especially on servers) => more software requirements, more overhead, more bloat, more vulnerabilities
  - Many security tools are command line based
  - Remote execution of commands
* Technology requirements for this class: Git, a hypervisor, and Kali Linux

# Tuesday, January 23th: Networking 101
* The really hard problems in Security
	- Sexual harassment and gender
* Why study networking?
	- Basics
  - The "Trinity of Trouble" by Gary McGraw
    - Complexity
    - Extensibility
    - Connectivity
	- The "Connectivity" issue
	- Where the "cool stuff" happens
	- The cyber attribution problem => https://twitter.com/thegrugq/status/706545282645757952
* The telephone conversation analogy for basic networking stuff
* Take it a step further: the OSI model and the seven layers
* Analogy for the OSI model: the US Postal Service => http://bpastudio.csudh.edu/fac/lpress/471/hout/netech/postofficelayers.htm
* Get comfortable reading Request For Proposals (RFPs):
	- Internet Protocol (IP): RFC 791 => http://www.ietf.org/rfc/rfc791.txt
	- Transfer Control Protocol (TCP): RFC 793 => http://www.ietf.org/rfc/rfc793.txt
* A packet: contains implementations of all the protocol layers; encapsulation model
* A PCAP: a file of packet captures from a network

# Thursday, January 25th: Basic Packet Analysis
* Last class: OSI model, a packet, PCAP
* simple.pcap
* Things that you can do with PCAP files
  - Visualizing the traffic; see which computer is communicating with who
  - Troubleshoot network problems
  - See if there is any malicious traffic or malware
  - Reconstruct files (e.g., images) and conversations
  - Rip out any username:password pairs sent-in-the-clear
* The Wall of Sheep
* Ettercap
* The next lab

# Tuesday, January 30th: Sniffing
* Last class: we delved into basic packet analysis
  - Visualized network at a given time using Etherape
  - Extracted all images in PCAP using Driftnet
  - Filter packets by IP address in Wireshark
  - Extract username:password pairs sent in plaintext on the network using Ettercap
  - Reconstructed a conversation in Wireshark via show TCP stream
* Recent news: Tinder
* An almost ugly spat: https://twitter.com/0xmchow/status/941145910213562369
* So you may be curious: how did we at the Wall of Sheep capture all those packets?
* tcpdump, Wireshark, ettercap
* Two types of networks:
  1. Unswitched - packets flow through all devices on network but you look at only the packets addressed to you......
    - Welp... http://superuser.com/questions/191191/where-can-i-find-an-unswitched-ethernet-hub
  2. Switched - packets flow through specific devices on network (most networks now)
* Step 1: promiscuous mode
* LAN Tap: http://hakshop.myshopify.com/products/throwing-star-lan-tap-pro
* Address Resolution Protocol
  - IP address to MAC address on a network
  - Recall OSI model and packets
  - `arp -a`
  - ARP cache on machine for 20 minutes
  - No authentication
* ARP spoofing or ARP poisoning
* Bettercap
* Video: https://www.youtube.com/watch?v=9uiA6dGuEE0

# Thursday, February 1st: Scanning
* Last class: sniffing unswitched and switched networks
* Is sniffing still relevant today?
* Preventing sniffing:
  1. Use encryption and encrypted network protocols
  2. VPN
  3. Use switched network......?
* Preventing ARP poisoning on switched network:
  - anti-arpspoof
  - ArpON
  - Antidote
  - Arpwatch
* Scanning
  - Why? Network reconnaissance.  Warfare 101
  - What devices and computers are up?
  - What ports are open on a computer?
  - What services are running?
  - Where are the computers geographically?
  - Determine possible vulnerabilities
* Is scanning still relevant today?
* Basic method: ping sweep
* Problems with ping?

# Tuesday, February 6th, Part II
* Last class: the idea of scanning, ping, Netcat...
* Think poking holes, "ask questions"
* Poking holes => finding interesting and unwanted stuff on networks
  - https://pen-testing.sans.org/blog/2017/02/28/opening-a-can-of-active-defense-and-cyber-deception-to-confuse-and-frustrate-attackers
* Netcat
* Nmap
* Geographical information: SHODAN

# Thursday, February 8th, Tuesday, Febraury 13th: Distributed Denial of Service (DDoS) Attacks
* "Inexperienced users and script kiddies, on the other hand, try to solve every problem with the default SYN scan. Since Nmap is free, the only barrier to port scanning mastery is knowledge. That certainly beats the automotive world, where it may take great skill to determine that you need a strut spring compressor, then you still have to pay thousands of dollars for it... By default, Nmap performs a SYN Scan, though it substitutes a connect scan if the user does not have proper privileges to send raw packets (requires root access on Unix). Of the scans listed in this section, unprivileged users can only execute connect and FTP bounce scans." https://nmap.org/book/man-port-scanning-techniques.html
* Defending against scanners
  - No certain way
  - Firewalls?
  - Close services
  - Packet filtering
* What could possibly go wrong?
* Want to be stealthy!
* RFC 793: if ports are closed and you send "junk" to it, RST packet will be sent! (page 65 of https://tools.ietf.org/html/rfc793)
  - FIN scan: `sudo nmap -sF ...`
  - NULL scan: `sudo nmap -sN ...`
  - XMAS scan: `sudo nmap -sX ...`, # FIN, PSH, URG flags in packet
* Decoy:
  - `sudo nmap -D...`
  - spoofed connections
  - Must use real + alive IP address, else SYN flood
* Rob Graham's Masscan
* The first "D" (Distributed) in DDoS: attack source is more than one, often thousands of, unique IP addresses
* SYN flood
  - The idea: exhaust states in the TCP/IP stack
  - Recall TCP/IP handshaking
  - Attacker sends SYN packets with a spoofed source address, the victim, (that goes nowhere)
  - Victim sends SYN/ACK packet but attacker stays slient
  - Half-open connections must time out which may take a while
  - Alas, good SYN packets will not be able to go through
  - Reference 1: https://www.cert.org/historical/advisories/CA-1996-21.cfm?
  - Reference 2, RFC 4987: https://tools.ietf.org/html/rfc4987
  - Reference 3: https://www.juniper.net/documentation/en_US/junos12.1x44/topics/concept/denial-of-service-network-syn-flood-attack-understanding.html
* Defending against SYN flood
  - Increase queue
  - Filtering
  - SYN cookies
  - Reduce timer for SYN packets
* Teardrop (old)
  - The idea: "involves sending fragmented packets to a target machine. Since the machine receiving such packets cannot reassemble them due to a bug in TCP/IP fragmentation reassembly, the packets overlap one another, crashing the target network device." https://security.radware.com/ddos-knowledge-center/ddospedia/teardrop-attack/
  - Recall RFC 791 (IP), the IP packet fields in question: Fragment Offset, Flag (namely "Don't fragment" and "More fragments")
  - Result: "Since the machine receiving such packets cannot reassemble them due to a bug in TCP/IP fragmentation reassembly, the packets overlap one another, crashing the target network device."
  - Reference: https://www.juniper.net/techpubs/software/junos-es/junos-es92/junos-es-swconfig-security/understanding-teardrop-attacks.html
* Ping of Death (old)
  - The idea: violate the IP contract
  - In RFC 791, the maximum size of an IP packet is 65,535 bytes --including the packet header, which is typically 20 bytes long.
  - An ICMP echo request is an IP packet with a pseudo header, which is 8 bytes long. Therefore, the maximum allowable size of the data area of an ICMP echo request is 65,507 bytes (65,535 - 20 - 8 = 65,507)
  - Result: "However, many ping implementations allow the user to specify a packet size larger than 65,507 bytes. A grossly oversized ICMP packet can trigger a range of adverse system reactions such as denial of service (DoS), crashing, freezing, and rebooting."
* ICMP Flood Attack => Overload victim with a huge number of ICMP echo requests with spoofed source IP addresses.
* UDP Flood Attack => Same idea of ICMP flood attack but using UDP packets

# Tuesday, February 15th
* Smurf Attack (old, 1990s) => An example of abusing `ping` and *amplification*
* Defending against ICMP flood and Smurf attacks => Disable `ping`
* DNS Amplification:
  - The idea: "relies on the use of publically accessible open DNS servers to overwhelm a victim system with DNS response traffic."
  - DNS server port number: 53
  - Reference 1: https://www.us-cert.gov/ncas/alerts/TA13-088A
  - Reference 2: https://blog.cloudflare.com/deep-inside-a-dns-amplification-ddos-attack/
  - Case study from last week: Brian Krebs http://krebsonsecurity.com/2016/09/krebsonsecurity-hit-with-record-ddos/
  - Mirai: https://vinceinthebay.files.wordpress.com/2017/02/rsac-slides-hta-w10-mirai-1.pdf
* How easy it is to spoof packets? I want to introduce you to Scapy......
* Scapy example 1: to make a DNS query: https://gist.github.com/thepacketgeek/6928674
* Scapy example 2: spoofing packets (Ping)
  - `packet = IP(src="",dst="")/ICMP()`
  - `sr1(packet)`

# Thursday, February 15th
* This week and next week: crypto, the foundation of Computer Security
* Before we go into crypto, two things
* About that server target in scanning lab...
* Encoding vs encryption: they are not the same
  - Encoding: "The purpose of encoding is to transform data so that it can be properly (and safely) consumed by a different type of system. The goal is not to keep information secret, but rather to ensure that it’s able to be properly consumed."
  - Encryption: "to transform data in order to keep it secret from others, e.g. sending someone a secret letter that only they should be able to read, or securely sending a password over the Internet. Rather than focusing on usability, the goal is to ensure the data cannot be consumed by anyone other than the intended recipient(s)."
  - Source: https://danielmiessler.com/study/encoding-encryption-hashing-obfuscation/
* About that peculiar port on the honeypot...
* About set3.pcap on the PCAPs lab:
  - A goal of this class: recognition and mindset
  - Base64: binary-to-text encoding scheme.  That is: binary data to ASCII
  - http://stackoverflow.com/questions/6916805/why-does-a-base64-encoded-string-have-an-sign-at-the-end
  - Why? Dangers in printing payload: https://unix.stackexchange.com/questions/73713/how-safe-is-it-to-cat-an-arbitrary-file
  - Why? Basic authentication on web. Example: https://github.com/LiamRandall/BsidesDC-Training/blob/master/http-auth/http-basic-auth-multiple-failures.pcap
* Last week: DDoS and amplification attacks => spoofing packets => Scapy
* So far, you have been using tools like Ettercap, Nmap, Wireshark.  Now, you must learn how they work!

# Tuesday, February 20th: Crypto
* The Python / Scapy lab
* So now crypto...
* Definitions
* The golden rule: "Never Roll Your Own Crypto"
* Crypto algorithms: symmetric, hash functions, asymmetric
* Tradeoffs to consider:
  - Cost of breaking a cipher
  - Value of the information that is encrypted
  - Time required to break info
  - Lifetime of information?
* The only secure crypto algorithm: One-Time Pad
  - Video: https://www.khanacademy.org/computing/computer-science/cryptography/crypt/v/one-time-pad
* Symmetric algorithms: DES, AES, RC4. What do they provide in terms of security? What do they not provide?
* One way hash functions: MD5, SHA-1.  What do they provide in terms of security? What do they not provide?

# Tuesday, February 27th: Crypto, Part II
* Recall the cyber attack cycle.........
* Last class: the golden rules of crypto, symmetric algorithms, one-way hash functions
* Applications: checksums, Git hash (SHA-1)
* Cracking user accounts on Linux systems:
  - Use /etc/passwd and /etc/shadow files from Linux-based systems
  - $algorithm$salt$hash
  - $1$ = MD5
  - $2$ = Blowfish
  - $5$ = SHA-256
  - $6$ = SHA-512
* Today: asymmetric crypto, public and private keys: RSA
* Example: SSH, GitHub
* What does asymmetric crypto does not provide?

# Thursday, March 1st: Crypto, Part III
* So how does Transport Layer Security (TLS) (also commonly known as Secure Socket Layer or SSL work)?
  - Why? HTTPS is HTTP inside of a TLS session
  - Uses BOTH symmetric and asymmetric crypto
  - Secure communications between two parties over a network
  - On top of TCP
  - Different port numbers used for TLS connection.  Port 443 for HTTPS
  - Part 1: Data between two parties encrypted via symmetric crypto.  Why?
  - Part 2: Identity of communicating parties identified via asymmetric crypto
  - Connection integrity via message integrity check using a message authentication code 
  - Digital certificates - assert the online identities of individuals, computers, and other entities on a network
    - They are issued by certification authorities (CAs) that must validate the identity of the certificate-holder both before the certificate is issued and when the certificate is used.
    - Specification: https://technet.microsoft.com/en-us/library/cc776447(v=ws.10).aspx
* TLS process:
  1. Client connects to TLS-enabled server. Client requesting a secure connection and presents a list of supported cipher suites (ciphers and hash functions).
  2. The server checks what the highest SSL/TLS version is that is supported by them both, picks a ciphersuite from one of the client's options (if it supports one), and optionally picks a compression method.
  3. The server sends back its identification via digital certificate (THIS MAY NOT HAPPEN)
  4. Client confirms validity of certificate --or NOT!
  5. Both the server and the client can now compute the session key (or shared secret) for the symmetric encryption and decryption of the data.  This computation of the session key is known as Diffie-Hellman key exchange.
  6. "The client tells the server that from now on, all communication will be encrypted, and sends an encrypted and authenticated message to the server."
* References:
  - https://security.stackexchange.com/questions/20803/how-does-ssl-tls-work
  - https://stackoverflow.com/questions/788808/how-do-digital-certificates-work-when-used-for-securing-websites-using-ssl
  - http://security.stackexchange.com/questions/45963/diffie-hellman-key-exchange-in-plain-english
  - https://blogs.akamai.com/2016/03/enterprise-security---ssltls-primer-part-1---data-encryption.html
  - https://blogs.akamai.com/2016/03/enterprise-security---ssltls-primer-part-2---public-key-certificates.html
* Creating self-signed certificates:
  - For Apache web servers: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04
  - For nginx web servers: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04