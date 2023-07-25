CCNA Topics covered:

5.0 Security Fundumentals
	
	5.1 Define Security concepts (threats, vulnerabilities, exploits,
	mitigation techniques)
	5.2 Describe security program elements (user awareness, training and
	physical access control)
	5.4 Describe security password policies elements, such as management, 
	complexity, and password alternatives (multifactor authentication,
	certificates, and biometrics)
	5.8 Differentiate authentication, authorization, and accounting concepts

Things we'll cover:

- Key security concepts
- Common attacks
- Passwords/Multi-Factor Authentication (MFA)
- Authentication, Authorization, Accounting (AAA)
- Security Program Elements

=== Why Security?

What is the purpose of security in an enterprise?

Foundation of security = CIA Triad:

- C = Confidentiality
	> only authorized users should be able to access data
	> some data is public, some is secret
	> there are degrees in the middle, some is more secret, some is less
- I = Integrity
	> data should not be modified by unauthorized users
	> data should be correct and authentic
- A = Availability
	> The network/systems should be available to authorized users

Attackers threaten the CIA of an enterprise

A vulnerability is any weakness that can damage the CIA of a system
	- a weakness is not a problem on its own, ie windows of a house

An exploit is something that can be used to exploit the vulnerability
	- an explout is not a problem on its own, ie a rock near a house

A threat is the potential of a vulnerability to be exploited
	- a hacker exploiting a vulnerability is a threat, ie a robber that uses a
	rock to break a house's window

A mitigation technique is something that can protect against threats
	- should be implemented everywhere a vulnerabilty can be exploited: client
	devices, servers, switches, routers, firewalls, etc.

NO SYSTEM IS PERFECTLY SECURE! THERE ARE NO GUARANTEES IN SECURITY!

=== Common attacks

- DoS (denial-of-service) attacks
- Spoofing attacks
- Reflection/amplification attacks
- Man-in-the-middle attacks
- Reconnaissance attacks
- Malware
- Social engineering attacks
- Password-related attacks

~ DoS attacks
DoS attacks threaten the A-availabilty of a system.
One common DoS attack is the TCP SYN flood.
	- TCP three-way handshake: SYN | SYN-ACK | ACK
	- The attacker sends countless TCP SYN messages to the target
	- The target sends a SYN-ACK in response to each SYN it gets
	- The attacker never replies with the final ACK of the TCP three-way 
	handshake
	-> Incomplete connections fill up the target's TCP connection table
	-> But the incomplete connections time out? The attacker keeps sending 
	SYN messages
	=> The target can't make TCP connections.

A DoS attack is typically not done by a single user, but by many. In a DDoS
(distibuted DoS) attack, the attacker infects many target computers (or simply
has many at his disposal somehow) with malware and uses them all to initiate a
denial-of-service attack, for example a TCP SYN flood attack. The group of
infected computers is called a botnet.

Again, as a result the target cannot respond to legitimate TCP connection 
requests.

~ Spoofing attacks
To spoof an address is to use a fake source address (IP or MAC).
Numerous attacks involve spoofing, it's not a single kind of attack.
An example is a DHCP exhaustion attack.
An attacker uses spoofed MAC address to flood DHCP Discover messages.
The target's DHCP pool becomes full, resulting in denial-of-service to others.
Not all spoofing attacks are DoS attacks, an example of spoofing attack that
threatens C and I is discussed later.

~ Reflection/Amplification attacks
In a reflection attack, the ATTACKER sends traffic to a REFLECTOR, and spoofs
the source address of its packets using the TARGET's IP address.
The REFLECTOR (ie a DNS server) sends the reply to the TARGET's IP address.
If the amount of traffic sent to the target is large enough, this can result in
denial-of-service.
A reflection attack becomes an amplification attack when the amount of traffic
sent by the ATTACKER is small, but it triggers a large amount of traffic to be
sent from the REFLECTOR to the TARGET.
For example, there are DNS and NTP vulnerabilities that can be exploited for
amplification attacks. You can take a closer look at these types of attacks 
here:

	https://www.cloudflare.com/learning/ddos/dns-amplification-ddos-attack/
	https://www.cloudflare.com/learning/ddos/ntp-amplification-ddos-attack/

~ Man-in-the-middle attacks
In a man-in-the-middle attack, the attacker places himself between the source
and destination to eavesdrop on communications, or to modify traffic before it
reaches the destination.

A common example is ARP spoofing, aka ARP poisoning.

A host sends an ARP request, asking for the MAC address of another device.
The target sends an ARP reply, informing the requester of its MAC address.
The attacker waits and sends another ARP reply after the legitimate replier.
If the attacker's ARP reply arrives last, it will overwrite the legitimate ARP
entry in PC1's ARP table.
In PC1's ARP table, the entry for target's IP will have the attacker's MAC 
address.
When PC1 tries to send traffic to target, it will be forwarded to the attacker
instead.
The attacker can inspect the messages, and then forward them on to the target.
The attacker can also modify the messages before forwarding them to the target.
This compromises the C and I of comms between a host and a target.

~Reconnaissance attacks

Recon attacks aren't attacks themselves, but they are used to gather info about
a target which can be used for a future attack.
This is often publicly available info.
ie. you can 'nslookup' to learn the IP address of a site.
You can also perform a WHOIS query to learn email addresses, phone numbers,
physical addresses, etc.
Try the following website:

	https://lookup.icann.org/lookup

~Malware

Malware (malicious software) refers to a variety of harmful programs that can
infect a computer.

Viruses infect other software (a 'host program'). The virus spreads as the
software is shared by users. Typically they corrupt or modify files on the
target computer.

Worms do not require a host program. They are standalone malware and they are
able to spread on their own, without user interaction. The spread of worms can
congest the network, but the 'payload' of a worm can cause additional harm to
target devices.

Trojan Horses are harmful software that is disguised as legitimate software. 
They are spread through user interaction such as opening email attachments, or
downloading a file from the Internet.

The above malware types can exploit various vulnerabilities to threaten any of
the CIA of the target device.
*there are MANY types of malware!

~Social Engineering Attacks

Social engineering attacks target the most vulnerable part of any system - 
people!
They involve psychological manipulation to make the target reveal confidential
information or perform some action.

Phishing typically involves fraudulent emails that appear to come from a 
legitimate business (bank, credit card company, etc) and contain links to a
fradulent website that seems legitimate.
Users are told to login to the fraudulent website, providing their login 
credentials to the attacker.

	spear phishing is a more targeted form of phishing, typically aimed at 
	employees of a certain company.

	whaling is phishing targeted at high-profile individuals, ie. a company
	president.

Vishing (voice phishing) is phishing performed over the phone.
Smishing = SMS phishing.

Watering hole attacks compromise sites that the target frequently visits. If a 
malicious link is placed on a website the target trusts, they might not 
hesitate to click it.

Tailgating attacks involve entering restricted areas by simply walking in 
behind an authorized person as they enter.

To summarize, social engineering attacks target employees, not systems 
directly.

~Password-related attacks

Most systems use a username/password combination to authenticate users.
The username is often simple/easy to guess (ie. the user's email address), and
the strength and secrecy of the password is relied on to provide the necessary
security.
Attackers can learn a password through multiple methods:
	Guessing
	Dictionary attack: a program runs through a 'dictionary' or list of common
	words/passwords to find the target's password.
	Brute force attack: a program tries every possible combination of letters,
	numbers, and special characters to find the target's password.

Strong passwords should contain: 

	at LEAST 8 characters (preferably more).
	a mixture of UPPERCASE and lowercase characters.
	a mix of letters and numbers
	one or more special characters (# @ ! ? etc.)
	should be changed REGULARLY

You should follow these rules with your personal passwords too.

Review:
	DoS (denial of service) attacks
		target the availability of a system so users can't access it
	Spoofing attacks
		involve using fraudulent source IP/MAC addresses
	Reflection/amplification attacks
		involve spoofing a source IP address to cause a reflector to send lots
		of traffic to the target
	Man-in-the-middle attacks
		an attacker intercepts traffic between the source and destination to
		eavesdrop and/or modify the traffic
	Reconnaissance attacks
		used to gather information on the target to perform future attacks
	Malware
		malicious software such as viruses, worms, and trojan horses that 
		infect a system
	Social engineering attacks
		attacks that use psychological manipulation to target people and make
		them reveal info or perform an action
	Password-related attacks
		attacks such as dictionary attacks and brute force attacks, used to
		guess the target's password

== Multi-factor Authentication

Multi-factor authentication involves providing more than just a username and
password to prove your identity.
It usually involves providing two of the following (two-factor authentication):

	Something you know
		a username/password combination, a PIN, etc.
	Something you have
		pressing a notification that appears on your phone, a badge that is
		scanned, etc.
	Something you are
		biometrics such as face scan, palm scan, fingerprint scan, retina scan,
		etc.

Requiring multiple factors of authentication greatly increases the security.
Even if an attacker learns the target's password (something you know), they 
won't be able to login to the target's account.

~Digital certificates

Digital certificates are another form of authentication used to prove the
identity of the holder of the certificate.
They are used for websites to verify that the website being access is legit.
Entities that want a certificate to prove their identity send a CSR
(Certificate Signing Request) to a CA (certificate Authority), which will
generate and sign the certificate.

When you access a website, modern browsers display a symbol on the left side of
the address bar. If you click on it, you will see certificate status. It has to
be valid. This is how you know the website is not fake.


=== Controlling and monitoring users with AAA

AAA (triple-A) stands for Authentication, Authorization, and Accounting.
It is a framework for controlling and monitoring users of a computer system
(ie. a network).

Authentication is the process of verifying a user's identity.
	logging in = authentication

Authorization is the process of granting the user the appropriate access and
permissions.
	granting the user access to some files/services, restricting access to 
	other files/services = authorization

Accounting is the process of recording the user's activities on the system.
	logging when a user makes a change to a file = accounting

Enterprises typically use a AAA server to provide AAA services.
	ISE (Identity Services Engine) is a Cisco's AAA service.

AAA servers usually support the following AAA protocols:
	RADIUS: an open standard protocol. Uses UDP ports 1812 and 1813.
	TACACS+: a Cisco proprietary protocol. Uses TCP port 49.

For the CCNA, know the difference between Authentication, Authorization, and
Accounting.

=== Security Program Elements

User awareness programs are designed to make employees aware of potential
security threats and risks.

	For example, a company might send out false phishing emails to make 
	employees click a link and sign in with their login credentials.
	
	Although the emails are harmless, employees who fall for the false emails
	will be informed that it is part of a user awareness program and they 
	should be more careful about phishing emails.

User training programs are more formal than user awareness programs.

	Dedicated training sessions which educate users on the corporate security
	policies, how to create strong passwords, and how to avoid potential
	threats.

Physical access control protects equipment and data from potential attackers by
only allowing authorized users into protected areas such as network closets or
data center floors.

	Multifactor locks can protect access to these areas.

		ie. a door that requires users to swipe a badge and scan their 
		fingerprint to enter.

		Permissions of the badge can be easily changed, for example permissions
		can be removed when an employee leaves the company.
