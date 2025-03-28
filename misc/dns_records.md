# DNS Records

- DNS - domain name system
- DNS resolves domain names to IP addresses

In a DNS hierarchy, there are three main levels of servers:
- root server
- top-level domain server
- authoritative name server - responsible for storing the DNS records &
resolving domain names to IP's, for knowing everything about a domain.

In a DNS databases there are zone files. They contain DNS records.

## A Record

The most common DNS records is the A record (address record):

| TYPE | NAME | IP ADDRESS | TTL |
| ---- | ---- | ---------- | --- |
| A | example.com | 12.34.56.78 | 3600 |

The A record resolves domain names to IPs (IPv4). TTL tells you how long each
record is valid until the next update.

## AAAA record

Same thing as an A record but for IPv6:

| TYPE | NAME | IP ADDRESS | TTL |
| ---- | ---- | ---------- | --- |
| AAAA | example.com | 2501::c2f4 | 3600 |

## CNAME

CNAME = canonical name. The CNAME (canonical) record resolves a domain name or
subdomain to another domain name.

| TYPE | NAME | ALIAS TO | TTL |
| ---- | ---- | -------- | --- |
| CNAME | www.example.com | example.com  | 3600 |

Computers read domain names from right to left, e.g. in `www.example.com.`:
- the last `.` is a root domain
- `.com` is a top level domain
- `.example` is a 2nd level domain
- `www` is a subdomain

It's common to create a CNAME record pointing from `www.<website>` to
`<website>`.

Subdomains are often used for different services running on the same website.

E.g., if a server has an `ftp` server:

| TYPE | NAME | ALIAS TO | TTL |
| ---- | ---- | -------- | --- |
| CNAME | ftp.example.com | example.com  | 3600 |

When a user types in `ftp.example.com`, DNS will forward the user to
`example.com`. But (!!!) once the request reaches the web server, the web
server will inspect the URL and direct it to its FTP service.

## MX

The MX (mail exchanger) record is used for email:

| TYPE | PRIORITY | NAME | HOST | TTL |
| ---- | :--------: | ---- | ---- | --- |
| MX | 10 | example.com  | mail1.example.com | 7200 |

It simply points to the server where email should be delivered for a domain
name.

The MTA (mail transfer agent) will query the MX records for `example.com`
because it's looking for an email server. The DNS will respond back, telling
the MTA which server to send the email to.

**The MX record tells the world which server to send the email to.** 

MX records generally have two entries:

| TYPE | PRIORITY | NAME | HOST | TTL |
| ---- | :--------: | ---- | ---- | --- |
| MX | 10 | example.com  | mail1.example.com | 7200 |
| MX | 20 | example.com  | mail2.example.com | 7200 |

The first one is the primary email server, the second one is secondary. The
lower the priority number, the higher the priority.

## SOA

The SOA (start of authority) record stores administrative information about a
DNS zone.

| TYPE | MNAME | RNAME | SERIAL # | RETRY | TTL |
| :---: | :---: | :---: | :---: | :---: | :---: |
| SOA | ns1.example.com | admin.example.com | 510025 | 60 | 7200 |

A **DNS zone** is a section of a domain name space that a certain administrator
has been delegated control over.

DNS zones allow a domain namespace (such as `example.com`) to be divided into
different sections. Imagine it has three subdomains:
- `shop.example.com` - has 5 computers
- `blog.example.com` - has 4 computers
- `support.example.com` - has 50 computers

The first two can be combined into one DNS zone, the third into another DNS
zone. 

DNS zones are created for manageability purposes. Each has its own DNS file
which contains an SOA record.

The MNAME is the primary name server. The RNAME is the email address of the
administrator for the zone, e.g. `admin.example.com`. The first dot represent
the `@` symbol of an email. The `Serial #` represents a version in the zone. So
whenever an update happens, the SN will change. It tells the secondary servers
to update as well.

## NS

The NS (name server) provides the name of the authoritative name server within
a domain.

| TYPE | VALUE | NAME | TTL |
| ---- | ---- | ---------- | --- |
| NS | ns1.example.com | example.com | 3600 |

The name server contains all the DNS records necessary for users to find a
computer or a server on a network. It is the final authority in the DNS
hierarchy.

There are usually two name servers (a primary and a secondary):

| TYPE | VALUE | NAME | TTL |
| ---- | ---- | ---------- | --- |
| NS | ns1.example.com | example.com | 3600 |
| NS | ns2.example.com | example.com | 3600 |

## SRV

SRV = service record. 

| TYPE | PRIORITY | SERVICE | PORT | NAME | WEIGHT | TTL |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| SRV | 10 | service.example.com | 999 | example.com | 0 | 7200 |

The SRV points to a server and a service by including a port number. When an
application needs to find a location of a service on a domain (VoIP, IM,
printer, etc.),  it will look for a service record to see if there's a listing
for that specific service and it will direct it to the correct server & port
number.

## PTR

A reverse of an A (or AAAA) record. The PTR (pointer) record resolves IPs to
domain names.

| TYPE | IP ADDRESS | NAME | TTL |
| ---- | ---------- | ---- | --- |
| PTR | 12.34.56.78 | example.com | 3600 |

E.g., PTR records are attached to email and are used to prevent email spam.

Whenever an email is received, the email server user the PTR record to make
sure that the sender is authentic by matching the domain name in the email with
its authentic IP. This is what's called a reverse DNS lookup. But if an email
that is sent does not match with its correct and authentic IP, the email will
be flagged as spam.

## TXT

The TXT record contains miscellaneous information about a domain, such as
general or contact information.

| TYPE | NAME | VALUE | TTL |
| ---- | ---- | ---------- | --- |
| TXT | example.com | sample text 123 | 3600 |

These are also used to prevent email spam by making sure incoming email is
coming from a trusted or authorized source.
