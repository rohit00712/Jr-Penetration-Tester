
# Nmap Advanced Port Scans

##  TCP Null Scan, FIN Scan, and Xmas Scan

**NULL SCAN :**

`sudo nmap -sN MACHINE_IP`

Nmap’s perspective, a lack of reply in a null scan indicates that either the port is open or a firewall is blocking the packet.

### null_scan_open.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/null_scan_open.png)

However, we expect the target server to respond with an RST packet if the port is closed. Consequently, we can use the lack of RST response to figure out the ports that are not closed: open or filtered.

### null_scan_close.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/null_scan_close.png)

**FIN SCAN :**

The FIN scan sends a TCP packet with the FIN flag set. You can choose this scan type using the `-sF` option. Similarly, no response will be sent if the TCP port is open. Again, Nmap cannot be sure if the port is open or if a firewall is blocking the traffic related to this TCP port.

### fin_scan_open.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/fin_scan_open.png)

However, the target system should respond with an RST if the port is closed. Consequently, we will be able to know which ports are closed and use this knowledge to infer the ports that are open or filtered. It's worth noting some firewalls will 'silently' drop the traffic without sending an RST.

### fin_scan_close.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/fin_scan_close.png)

**Xmas SCAN :**

The Xmas scan gets its name after Christmas tree lights. An Xmas scan sets the FIN, PSH, and URG flags simultaneously. You can select Xmas scan with the option `-sX`.

Like the Null scan and FIN scan, if an RST packet is received, it means that the port is closed. Otherwise, it will be reported as open|filtered.

The following two figures show the case when the TCP port is open and the case when the TCP port is closed.

### xmas_scan_open.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/xmas_scan_open.png)

### xmas_scan_close.png

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Internet%20Security/Nmap%20Advanced%20Port%20Scans/images/xmas_scan_close.png)

# TCP MAIMON SCAN

Uriel Maimon first described this scan in 1996. In this scan, the FIN and ACK bits are set. The target should send an RST packet as a response. However, certain BSD-derived systems drop the packet if it is an open port exposing the open ports. This scan won’t work on most targets encountered in modern networks

Most target systems respond with an RST packet regardless of whether the TCP port is open. In such a case, we won’t be able to discover the open ports. The figure below shows the expected behaviour in the cases of both open and closed TCP ports.

### maimon_scan.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

The console output below is an example of a TCP Maimon scan against a Linux server. As mentioned, because open ports and closed ports are behaving the same way, the Maimon scan could not discover any open ports on the target system.

This type of scan is not the first scan one would pick to discover a system; however, it is important to know about it as you don’t know when it could come in handy.








## Screenshots

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

