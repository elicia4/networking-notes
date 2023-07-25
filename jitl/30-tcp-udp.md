# TCP & UDP

TCP and UDP are very important Layer 4 protocols. You must be able to compare
them for the CCNA. You just need a high-level understanding of their basic
characteristics and how they are different.

Things to cover:
- Basics of Layer 4
- TCP (Transmission Control Protocol)
- UDP (User Datagram Protocol)
- Comparing TCP & UDP

### Basics of Layer 4

- Provides transparent transfer of data between end hosts. Layer 4 encapsulates
the data with a Layer 4 header and user the services of the lower layers to
deliver the data unchanged to the destination host. The hosts themselves aren't
aware of the underlying network, the transfer of data is "transparent" to them.
- Provides (or doesn't provide) various services (TCP does and UDP doesn't) to
  applications:
    - reliable data transfer (destination receives every bit of data it's
      supposed to receive)
    - error recovery (if an error occurs, data has to be sent again)
    - data sequencing (if data arrives out of order, the end host can sequence
      it in the correct order)
    - flow control (making sure that traffic is sent at a speed that the
      destination host can handle)
- Provides Layer 4 addressing (port numbers).
    - Identify the Application Layer protocol
    - Provides session multiplexing

A session is an exchange of data between two or more communicating devices. In
daily use, a PC has to handle multiple sessions at once. Perhaps you have
multiple tabs open, accessing multiple services at the same time.

Imagine a PC (PC1) needs to communicate with a server (SRV1). At Layer 4, it's 
using TCP and it uses a source port of 50000 and a destination port of 80. The
destination port is very import, it identifies the application layer protocol.
For example, TCP port 80 is used for HTTP, which is used to access websites.
The source port is randomly selected by PC1, it helps to identify the session. 
After SRV1 receives the message, it will (probably) send a reply. In its reply,
SRC and DST ports will be reversed. The source and destination port numbers will
tell PC1 that the message is from the same communication session as the message
it sent earlier.

    PC1 ---=(TCP):Src:50000 Dst:80=---> SRV1
    SRV1 <---=(TCP):Src:80 Dst:50000=--- PC1 

What if PC1 opens a separate connection to SRV1? It might be using the same
destination port (80), but it's using a different source port (55000). SRV1's
response will use that source port as its destination port, so PC1 knows it's
part of that session. 

    PC1 ---=(TCP):Src:55000 Dst:80=---> SRV1
    SRV1 <---=(TCP):Src:80 Dst:55000=--- PC1 

But PC1 want to access something on SRV2 at the same time. It uses a TCP
destination port of 21[^1], and randomly selects the source port 60000.

    PC1 ---=(TCP):Src:60000 Dst:21=---> SRV2
    SRV2 <---=(TCP):Src:21 Dst:60000=--- PC1 

[^1]: FTP = File Transfer Protocol, it is used to transfer files.

That's how ports identify the Application Layer protocol, such as HTTP or FTP,
and how they are used to manage multiple communication sessions at once.
    
The following ranges have been designated by IANA[^2] for port numbers:

- **well-known** ports: 0 - 1023 (these are used for major protocols, like FTP,
  HTTP etc. and are strictly regulated)
- **registered** ports: 1024 - 49151 (registration is required to use these
  port numbers, though it's not as strict as with the well-known port range)
- **ephemeral/private/dynamic** ports: 49152 - 65535 (hosts use this range when
selecting a random source port)

[^2]: IANA = Internet Assigned Numbers Authority
