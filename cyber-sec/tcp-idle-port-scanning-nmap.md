---
description: 'https://nmap.org/book/idlescan.html'
---

# TCP Idle Port scanning \(NMAP\)

While idle scanning is more complex than any of the techniques discussed so far, you don't need to be a TCP/IP expert to understand it. It can be put together from these basic facts:

* One way to determine whether a TCP port is open is to send a SYN \(session establishment\) packet to the port. The target machine will respond with a SYN/ACK \(session request acknowledgment\) packet if the port is open, and RST \(reset\) if the port is closed. This is the basis of the previously discussed SYN scan.
* A machine that receives an unsolicited SYN/ACK packet will respond with a RST. An unsolicited RST will be ignored.
* Every IP packet on the Internet has a fragment identification number \(IP ID\). Since many operating systems simply increment this number for each packet they send, probing for the IPID can tell an attacker how many packets have been sent since the last probe.

By combining these traits, it is possible to scan a target network while forging your identity so that it looks like an innocent zombie machine did the scanning.

