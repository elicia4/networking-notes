# TCP & UDP

TCP and UDP are very important Layer 4 protocols. You must be able to compare
them for the CCNA. You just need a high-level understanding of their basic
characteristics and how they are different.

Things to cover:
- Basics of Layer 4
- TCP (Transmission Control Protocol)
- UDP (User Datagram Protocol)
- Comparing TCP & UDP
- Important Port Numbers

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
destination port is very important, it identifies the application layer
protocol. 

For example, TCP port 80 is used for HTTP, which is used to access websites.
The source port is randomly selected by PC1, it helps to identify the session.
After SRV1 receives the message, it will (probably) send a reply. In its reply,
SRC and DST ports will be reversed. The source and destination port numbers
will tell PC1 that the message is from the same communication session as the
message it sent earlier.

    PC1 ---=(TCP):Src:50000 Dst:80=---> SRV1
    PC1 <---=(TCP):Src:80 Dst:50000=--- SRV1

What if PC1 opens a separate connection to SRV1? It might be using the same
destination port (80), but it's using a different source port (55000). SRV1's
response will use that source port as its destination port, so PC1 knows it's
part of that session. 

    PC1 ---=(TCP):Src:55000 Dst:80=---> SRV1
    PC1 <---=(TCP):Src:80 Dst:55000=--- SRV1

But PC1 want to access something on SRV2 at the same time. It uses a TCP
destination port of 21[^1], and randomly selects the source port 60000.

    PC1 ---=(TCP):Src:60000 Dst:21=---> SRV2
    PC1 <---=(TCP):Src:21 Dst:60000=--- SRV2

[^1]: 21 is used for FTP, the File Transfer Protocol, its purpose is to
    transfer files.

That's how ports identify the Application Layer protocol, such as HTTP or FTP,
and how they are used to manage multiple communication sessions at once.
    
The following ranges have been designated by IANA[^2] for port numbers:

- **well-known** ports: 0 - 1023 (these are used for major protocols, like FTP,
  HTTP etc. and are strictly regulated)
- **registered** ports: 1024 - 49151 (registration is required to use these
  port numbers, though it's not as strict as with the well-known port range)
- **ephemeral/private/dynamic** ports: 49152 - 65535 (hosts use this range when
selecting a random source port)

[^2]: IANA = Internet Assigned Numbers Authority.

### TCP

- TCP is connection-oriented.
    - Before sending data to the host, the two hosts communicate to establish a
      connection. Once the connection is established, the data exchange begins.
- TCP provides reliable communication.
    - The destinataion host must acknowledge that it received each TCP segment.
    - If a segment isn't acknowledged, it's sent again.
- TCP provides sequencing.
    - Sequence numbers in the TCP header allow destination hosts to put
      segments in the correct order even if they arrive out of order.
- TCP provides flow control.
    - The destination host can tell the source host to increase/decrease the
      rate that data is sent.

TCP header consists of the following parts:

1. 0-15: source port (16)
2. 16-31: destination port (16)
3. 32-63: sequence number (32)
1. 64-95: acknowledgement number (if ACK is set) (32)
1. 96-99: data offset (4)
1. 100-102: reserved (3)
1. 103-111: flags (9)
    1. NS
    1. CWR
    1. ECE
    1. URG
    1. ACK
    1. PSH
    1. RST
    1. SYN
    1. FIN
1. 112-127: windows size (16)
1. 128-143: checksum (16)
1. 140-160: urgent pointer (if URG is set) (16)
1. 160-...: Options (if data offset > 5. Padded at the end with "0" bytes if
   necessary.)

Explaining the header:

1. There are **2^16** = 65536 available port numbers in total.
1. Sequence acknowledgement numbers provide sequencing and reliable
   communication.
1. **ACK**, **SYN**, and **FIN** flags are used to establish and terminate
   connections.
1. The window size field is used for flow control, adjusting the rate at which
   data is sent.
    
The method TCP uses to establish connections is called the TCP Three-Way
Handshake. It has that name because it involves three messages sent between the 
hosts. 

Let's say PC1 wants to access a webpage on SRV1 using HTTP. First it must
establish a TCP connection, to do it uses ACK (meaning acknowledgement) and SYN
(meaning synchronization) flags.

1. First PC1 will send a TCP segment with the SYN flag set. 
2. Next, SRV1 will reply by sending a TCP segment back with the SYN and ACK
   flags set. 
3. Finally, PC1 will send a TCP segment with the ACK bit set. 

After this process is done, the connection is established. The real data
exchange can now begin. The first three messages are just to establish the
connection.

TCP has to follow a proccess to terminate connections as well. It is less
famous than the three-way handshake. It is called a four-way handshake, the
process uses the **FIN** and **ACK** flags.

1. PC1 sends a TCP segment to SRV1 with the FIN flag set.
2. SRV1 responds with an ACK.
1. SRV1 then also sends its own FIN.
1. Finally, PC1 sends its own ACK in response to SRV1's FIN, and the connection
   is terminated.


Now let us see how TCP uses the sequence and acknowledgement fields of the
header to provide reliable communication and sequencing. Let's look at an exchange between two PCs.

When PC1 sends the three-way handshake's SYN message, it sets a random initial
sequence number, 10 for example. When SYN-ACK is sent back, it sets its own
random initial sequence number, let's say 50. Not only that, it also
acknowledges that it received PC1's segment with a sequence number of 10 by
setting its own acknowledgement field to 11. That's because TCP uses something
called "forward acknowledgement". Instead of acknowledging with an ACK field of
10, it tells PC1 the sequence number of the next segment is expects to receive. 
Finally, PC1 responds with a sequence number of 11 and using forward
acknowledgement it sets a value of 51 in the ack field. PC2 replies with an seq
number 51 and again uses forward acknowledgement by setting a value of 12 to
the ack field. Then the exchange continues.

**PC1 --> PC2**

1. PC1: Seq 10
2. PC2: Seq 50, Ack 11
3. PC1: Seq 11, Ack 51
4. PC2: Seq 51, Ack 12
5. PC1: Seq 12, Ack 52
6. PC2: Seq 52, Ack 13
1. **...**

Things to remember:

- Hosts set a random initial sequence number.
- Forward acknowledgement is used to indicate the sequence number of the next
segment the host expects to receive.

Obviously, these sequence numbers also allow hosts to know the correct order of
segments, even if for some reason they arrive out of order. But what about "if
a segment isn't acknowledged, it is sent again"?

**PC1 --> PC2**
1. Seq: 20
2. Ack: 21
3. Seq: 21, this segment DOES NOT reach PC2
4. Seq: 21, after a certain amount of time passes with no Ack, PC1 resends the
   segment. This is called TCP retransmission.
5. Ack: 22

Let us also see how TCP provides flow control:

- Acknowledging every single segment, no matter what size, is inefficient.
- The TCP header's **Windows Size** field allows more data to be sent before an
  acknowledgement is required. For example, a host sends 3 segments 20-22, and
  then an Ack is sent back to it with sequence number 23.

```
-> Seq: 20
-> Seq: 21
-> Seq: 22
<- Ack: 23
```

- A 'sliding window' can be used to dynamically adjust how large the window
  size is. It is increased as much as possible until a segment is dropped, then
  the window size backs down to a more reasonable level, and slowly increases
  again.

NOTE: In all of these examples, very simple sequence numbers are used, in
reality they are a lot bigger and do not increase by 1 with each message.

### UDP

- UDP is not connection-oriented.
    - The sending host does not establish a connection with the destination
      host before sending data. The data is simply sent.
- UDP does not provide reliable communication.
    - When UDP is used, acknowledgements are not sent for received segments. If
      a segment is lost, UDP has no mechanism to re-transmit it. Segments are
      sent 'best-effort'.
- UDP does not provide sequencing.
    - There is no sequence number field in the UDP header. If segments arrive
      out of order, UDP has no mechanism to put them back in order.
- UDP does not provide flow control.
    - UDP has no mechanism like TCP's windows size to control the flow of data.

Let's take a look at the UDP header:

1. 0-15: Source port (16)
1. 16-31: Destination port (16)
1. 32-47: Length (16)
1. 48-63: Checksum (16)

The differences in the contents of the headers are what makes the functionality
in TCP possible.

### Comparing TCP & UDP

- TCP provides more features than UDP, but at the cost of additional
  **overhead**.
- For applications that require reliable communications (for example
  downloading a file), TCP is preferred.
- For apps like real-time voice and video, UDP is preferred.
- There are some apps that use UDP, but provide reliability and other TCP
  functionality within the app itself.
- There are apps that use both TCP & UDP, depending on the situation(like DNS).

| TCP | UDP |
| :-------: | :-------: |
| Connection-oriented | Connectionless |
| Reliable | Unreliable |
| Sequencing | No sequencing |
| Flow control | No flow control |
| Use for downloads, file sharing, etc. | Used for VoIP, live video, etc |

### Important Port Numbers

1. TCP
    1. FTP data (20) - File Transfer Protocol
    1. FTP control (21)
    1. SSH (22) - used to connect to CLI of nodes, encrypted
    1. Telnet (23) - same function as SSH, but it's not encrypted
    1. SMTP (25) - used for sending email
    1. HTTP (80) - used for accessing webpages
    1. POP3 (110) - Post Office Protocol 3 - used for retreiving emails
    1. HTTPS (443) - HTTP Secure
1. UDP
    1. DHCP server (67) - Dynamic Host Configuration Protocol - allows hosts to
    set their IP addresses automatically and other things.
    1. DHCP client (68)
    1. TFTP (69) - Trivial FTP
    1. SNMP agent (161) - Simple Network Management Protocol
    1. SNMP manager (162)
    1. Syslog (514)
1. TCP & UDP
    1. DNS (53)

That's pretty much everything you need for the CCNA exam. You only need to be
able to compare TCP to UDP (exam topic 1.5), so focus on that.
