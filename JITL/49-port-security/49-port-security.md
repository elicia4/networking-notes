Port security is a security feature on Cisco switches that allows you to 
control what source MAC addresses are allowed on a switch port, as well as how
many.

It's covered in exam topic:
5.7 Configure Layer 2 security features (DCHP snooping, dynamic ARP inspection,
and PORT SECURITY)

Things we'll cover:
	
	Intro to port security
	Why use port security?
	Port security configuration

== Intro to port security

Port security is a security feature of Cisco switches.
It allows you to control which source MAC address(es) are allowed to enter the
switchport, it's configured on a per-interface basis (port=interface)
If an unauthorized source MAC address enters the port, an action will be taken.

	The default action is to place the interface in an 'err-disabled' state, it
	means that any traffic from an interface different from the configured MAC
	address will not be received and the port will be disabled. It won't send
	or receive data until you enable the interface again. 

When you enable port security on an interface with the default settings, one 
MAC address is allowed.
	
	You can configure the allowed MAC address manually.
	If you don't configure it manually, the switch will allow the first MAC
	address that enters the interface.

You can change the maximum number of MAC addresses allowed.
A combination of manually configured MAC addresses and dynamically learned
addresses is possible.

== Why use port security?

	Port security allows network admins to control which devices are allowed to
	access the network.
	However, MAC address spoofing is a simple task.

		It's easy to configure a device to send frames with a different source
		MAC address.
	
	Rather than manually specifying the MAC addresses allowed on each port, 
	port security's ability to limit the number of MAC addresses allowed on an
	interface is more useful.
	
	Think of the DHCP starvation attack.
		
		the attacker spoofed thousands of fake MAC addresses
		the DHCP server assigned IP addresses to those fake MAC addresses,
		exhausting the DHCP pool
		the switch's MAC address table can also become full due to such an
		attack

	Limiting the number of MAC addresses on an interface can protect against
	those attacks*.

	*With port security, you can limit the number of MAC addresses learned by
	the port. Hence, the switch would forward packets with known MAC addresses,
	and discard others. This would prevent bogus packets from reaching the DHCP
	server. 

== Port security configuration

*enter global config mode* (enable -> configure terminal)

Choose the interface you'd like to configure:
	interface g0/1

Then to configure port security:
	switchport port-security

This might fail with an error "Command rejected: *port* is a dynamic port."
Check the interface and try to figure out the answer:
	do show int g0/1 switchport
	
Notice that there's a line that says:
	Administrative Mode: dynamic auto

By default, switchports have an administrative mode of dynamic auto. That's the
DTP command, 'switchport mode dynamic auto'. Port security can be enabled on
access ports or trunk ports, but they must be statically configured as access
or trunk.
	switchport mode access = OK
	switchport mode trunk = OK
	switchport mode dynamic auto = NOT OK
	switchport mode dynamic desirable = NOT OK

So, for example, configure it as an access port:
	switchport mode access	

Run the command again to check:
	do show int g0/1 switchport

The administrative mode is now static access, so the 'switchport port-security'
command should work:
	switchport port-security

The command works, so port security is now enabled on G0/1.
When you use just these command, port security is enabled with the default
settings. To show them, run:

	show port-security interface g0/1

The output is:

	Port Security: Enabled - port security is enabled
	Port Status: Secure-up - port security is enabled, the interface is up
	Violation mode: Shutdown - if an unauthorized frame enters the interface,
	it will be disabled
	Aging Time: 0 mins - the addresses will never age out, there is no timer
	Aging Type: Absolute -
	SecureStatic Address Aging: Disabled
	Maximum MAC Addresses: 1
	Total MAC Addresses: 0
	Configured MAC addresses: 0
	Sticky MAC Addresses: 0
	Last Source Address:Vlan: 0000.0000.0000:0
	Security Violation Count: 0
	
