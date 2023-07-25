This is a demonstration on how it's done:
https://youtu.be/3iu7NSqBTDg

access-list 101 remark My First ACL!!!
# this allows you to comment your acls

access-list 101 permit ip host 10.0.0.11 host 54.4.4.7
# one ip to one ip

# subnet to one ip
access-list 101 permit 10.0.0.0 0.0.0.255 host 45.5.5.8

# icmp - pings, traceroutes, etc.
access-list 101 permit icmp any any
# any source can ping any destination

# see everything that's been configured
do show run | section access-list

# configuration with ports
access-list 101 permit tcp host 10.0.0.11 eq 7777 host 54.4.4.7 eq 80
# notice the tcp protocol!
# there is every extended acl parameter mentioned in the command above

# typically when you want to allow specific type of traffic you omit source
# port 
access-list 101 permit tcp host 10.0.0.11 host 54.4.4.7 eq 80
# it allows you to accommodate any selected port number
# this would work the other way around for packets coming the other way

What happens if a packet arrives to your router that doesn't work with any of
these entries? Everything that is not defined in the ACL is implicitly denied.

# let's deny some ntp traffic
# it uses port 123 (udp)
access-list 101 deny udp host 10.0.0.11 host 45.5.5.8 eq 123

# access list are processed top down, so if a packet matches a rule, the 
# process of condition checking stops. ACL are processed based on the first 
# match. 

At the end of each ACL is an implicit deny
	Traffic not matched explicitly is assumed to be undesired
ACL entries are processed based on First match
	Order of entries matter

# you should have more specific statements at the top, and less specific lower
# YOU CANT REMOVE RULES ONE BY ONE. The "no <statement>" command will blow away
# the entire ACL. That is the limitation that exists with the numbered ACLs, 
# but not with the named ones -> use named ones all the time?
