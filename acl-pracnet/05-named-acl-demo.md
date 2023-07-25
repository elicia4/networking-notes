There are diagrams and pictures that can't be translated to these notes, so
watch the video: 
https://youtu.be/GG8E5FIx9s4
#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#=#

Syntax for Named ACL:

ip access-list standard <ID# or Name> [sequence#] <action> <source>
ip access-list extended <ID# or Name> [sequence#] <action> <protocol> <action>
<source> <destination>

You can reconfigure your Numbered ACL to Named ACL like this:
ip access-list extended PRACNET
	remark My first ACL!!!
	permit ip host 10.0.0.1 host 54.4.4.7
	...

You can remove any access list entry from your ACL like this:

enter global config mode:
	conf t
enter extended named acl configuration mode:
	ip access-list extended PRACNET

THEN YOU RUN THE 'no' COMMAND!!! :
	no permit ip host 10.0.0.1 host 54.4.4.7

And then you can re-add it, thus moving it from whichever place it was at to 
the end of the list

There is another way aside from
	do show run | section access-list
to view your access list:
	R1# show access-list

Notice that the entries have a sequence number:
	...
	30 permit icmp any any
	40 permit ...
	...

They show up in increments of ten. 
You can remove lines by their sequence number:
enter global config mode:
	conf t
enter extended named acl configuration mode:
	ip access-list extended PRACNET
remove the command:
	no 40

You can add commands the same way:
	R1(config-ext-nacl)# 65 deny tcp host 10.0.0.11

You can use using Named ACL Syntax to modify Numbered ACLs. Imagine you have an
access-list:
	conf t
	ip access-list extended 101
	no 20
	67 <contents-of-the-20th-entry>

You can resequence your ACLs at whatever interval you want:
	ip access-list resequence 101 3000 11
						   number starting-index increment

Named ACL Syntax...
	Allow you to remove individual lines
	Allow you to insert lines at desired location
	Allow you to modify Numbered ACL
	Allow you to renumber ACL Sequence Numbers 
	Allow you to configure IPv6 ACLs
