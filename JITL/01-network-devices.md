Who is this course for?

This course is for anyone...
	
	who wants to pass the CCNA 200-301 exam (from February 24, 2020)
	who wants to learn networking

All you need to get started is a basic familiarity with computers

This will cover network devices.

What is a network?

A computers network is a digital telecommunications network which allows nodes
to share resources.
What is a node? Some types of network nodes are:

	router
	switch
	firewall
	server
	client

Servers and clients are referred to as end hosts or endpoints.
Two computers by themselves do not constitute a network, but if you connect 
them they do. It fits the definition above.

A client is a device that accesses a service made available by a server.
A server is a device that provides functions or services for clients.

In network diagrams, a cloud represents the Internet, or part of the network
whose details aren't necessary.

The same device can be a client in some situations, and a server is other
situations.

You typically don't connect devices like pc and servers to each other, you 
aggregate the connections to a device called a switch. Switches have lots of
interfaces for you to connect end hosts to. Switches are used to forward 
traffic within a LAN, a local area network. However, switches cannot connect
directly ot the Internet and send data between LANs. Examples of switches:

	Catalyst 9200
	Catalyst 3650

Switches...
	have many network interfaces/ports for end hosts to connect to, usually 24+
	provide connectivity to hosts within the same LAN (Local Area Network)
		LAN = end hosts in the same area, like a bunch of computers on one 
		floor of an office, or perhaps an entire small office, or your home
		network
	do not provide connectivity between LANs/over the Internet.

Examples of routers:
	
	ISR 1000
	ISR 900
	ISR 4000

Routers...
	have fewer network interfaces than switches
	are used to provide connectivity between LANs
	are therefore used to send data over the Internet

Imagine there's an attacker somewhere on the Internet, with an arsenal of many
was he could attack our networks to steal information or otherwise damage our
Enterprise. Although routers can also provide some basic security features, 
what we should really be using to protect our networks is a Firewall.

Firewalls are specialty network security devices that control network traffic
entering and exiting your network. Firewalls can be places 'outside' of your
router, or 'inside' of your network. What's important is that they protect the
end hosts inside, like PCs and Servers. Firewalls must be configured with
security rules to determine which network traffic should be allowed and which
should be denied. If you configure the rules properly, the firewalls should 
permit the traffic between your devices through, however, if an attacker tries
to access anything inside your networks, the firewall should block it. 

Examples of firewalls:

	ASA5500-X (ASA = Adaptive Security Appliance)
	Firepower 2100

Modern ASAs include modern features of so-called 'next generation firewalls',
including things like IPS, or intrusion prevention system. You'll hear a lot 
more about that in the security section of this course.

Firewalls:
	Monitor and control network traffic based on configured rules
	Can be placed 'inside' the network, or 'outside the network'
	Firewalls are known as 'Next-Generation Firewalls' when they include more
	modern and advanced filtering capabilities.

Network firewalls...
	are hardware devices that filter traffic between networks.

Host-based firewalls...
	 are software applications that filter traffic entering and exiting a host
	 machine, like a PC.
