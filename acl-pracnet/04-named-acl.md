ACLs:

Numbered Standard ACL:
access-list <ID#> <action> <source>

Numbered Extended ACL:
access-list <ID#> <action> <protocol> <source> <destination>
===============================================================================
Syntax for Named ACL:
ip access-list standard <ID# or Name> [sequence#] <action> <source>

ip access-list extended <ID# or Name> [sequence#] <action> <protocol> <source>
<destination>

The syntax is largely the same:
1. access-list -> ip access-list
2. optional [sequence#] parameter
3. <ID#> -> <ID# or Name>
4. added explicit "extended" and "standard" options
	not reliant on number (1-99, 100-199, etc...)

Order of entries in ACL is significant
By default, new ACL entries appear at the end
	Can be controlled by specifying line number (i.e, sequence#)

Named ACLs provide additional features over Numbered ACLs
ACLs have evolved over time:
	Standard ACLs -> Extended ACLs
	Numbered Syntax -> Named Syntax
