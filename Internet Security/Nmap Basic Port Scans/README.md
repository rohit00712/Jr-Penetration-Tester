
# Nmap Basic Port Scans

## TCP And UDP Port

At the risk of oversimplification, we can classify ports in two states:

Open port indicates that there is some service listening on that port.
Closed port indicates that there is no service listening on that port.
However, in practical situations, we need to consider the impact of firewalls. For instance, a port might be open, but a firewall might be blocking the packets. Therefore, Nmap considers the following six states:

* **Open:** indicates that a service is listening on the specified port.
* **Closed:** indicates that no service is listening on the specified port, although the port is accessible. By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.
* **Filtered:** means that Nmap cannot determine if the port is open or closed because the port is not accessible. This state is usually due to a firewall preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host.
* **Unfiltered:** means that Nmap cannot determine if the port is open or closed, although the port is accessible. This state is encountered when using an ACK scan -sA.
* **Open|Filtered:** This means that Nmap cannot determine whether the port is open or filtered.
* **Closed|Filtered:** This means that Nmap cannot decide whether a port is closed or filtered.

## TCP Flags

Nmap supports different types of TCP port scans. To understand the difference between these port scans, we need to review the TCP header. The TCP header is the first 24 bytes of a TCP segment. The following figure shows the TCP header as defined in RFC 793. This figure looks sophisticated at first; however, it is pretty simple to understand. In the first row, we have the source TCP port number and the destination port number. We can see that the port number is allocated 16 bits (2 bytes). In the second and third rows, we have the sequence number and the acknowledgement number. Each row has 32 bits (4 bytes) allocated, with six rows total, making up 24 bytes.

## tcp_header.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

In particular, we need to focus on the flags that Nmap can set or unset. We have highlighted the TCP flags in red. Setting a flag bit means setting its value to 1. From left to right, the TCP header flags are:

* **URG:** Urgent flag indicates that the urgent pointer filed is significant. The urgent pointer indicates that the incoming data is urgent, and that a TCP segment with the URG flag set is processed immediately without consideration of having to wait on previously sent TCP segments.
* **ACK:** Acknowledgement flag indicates that the acknowledgement number is significant. It is used to acknowledge the receipt of a TCP segment.
* **PSH:** Push flag asking TCP to pass the data to the application promptly.
* **RST:** Reset flag is used to reset the connection. Another device, such as a firewall, might send it to tear a TCP connection. This flag is also used when data is sent to a host and there is no service on the receiving end to answer.
* **SYN:** Synchronize flag is used to initiate a TCP 3-way handshake and synchronize sequence numbers with the other host. The sequence number should be set randomly during TCP connection establishment.
* **FIN:** The sender has no more data to send.

##  TCP Connect Scan

`nmap -sT 10.10.223.164`

## sT_scan.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

It is important to note that if you are not a privileged user (root or sudoer), a TCP connect scan is the only possible option to discover open TCP ports.

## TCP SYN Scan

`nmap -sS TARGET_IP`

## tcpsyn_scan.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## UDP Scan

`nmap -sU TARGET_IP`

## udp_scan.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## udp_scan2.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

# Summary

This room covered three types of scans.

| **Port Scan Type** | **Example Command**                |
| :-------- | :------------------------- |
| TCP Connect Scan | `nmap -sT 10.10.223.164` |
| TCP SYN Scan | `sudo nmap -sS 10.10.223.164` |
| UDP Scan | `sudo nmap -sU 10.10.223.164` |

These scan types should get you started discovering running TCP and UDP services on a target host.

| Option |    Purpose               |
| :--------  |:------------------------- |
| -p- | all ports |
| -p1-1023 | scan ports 1 to 1023 |
| -F | 100 most common ports  |
| -r | scan ports in consecutive order  |
| -T<0-5> | -T0 being the slowest and T5 the fastest  |
| --max-rate 50 | rate <= 50 packets/sec |
| --min-rate 15 | rate >= 15 packets/sec |
| --min-parallelism 100 | at least 100 probes in parallel |





