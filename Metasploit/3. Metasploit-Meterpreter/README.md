
# Introduction of Meterpreter

Meterpreter is a Metasploit payload that supports the penetration testing process with many valuable components. Meterpreter will run on the target system and act as an agent within a command and control architecture. You will interact with the target operating system and files and use Meterpreter's specialized commands.

Meterpreter has many versions which will provide different functionalities based on the target system.

### How does Meterpreter work?

Meterpreter runs on the target system but is not installed on it. It runs in memory and does not write itself to the disk on the target. This feature aims to avoid being detected during antivirus scans. By default, most antivirus software will scan new files on the disk (e.g. when you download a file from the internet) Meterpreter runs in memory (RAM - Random Access Memory) to avoid having a file that has to be written to the disk on the target system (e.g. meterpreter.exe). This way, Meterpreter will be seen as a process and not have a file on the target system.

Meterpreter also aims to avoid being detected by network-based IPS (Intrusion Prevention System) and IDS (Intrusion Detection System) solutions by using encrypted communication with the server where Metasploit runs (typically your attacking machine). If the target organization does not decrypt and inspect encrypted traffic (e.g. HTTPS) coming to and going out of the local network, IPS and IDS solutions will not be able to detect its activities.

While Meterpreter is recognized by major antivirus software, this feature provides some degree of stealth.

`meterpreter >`

`getpid` - to get the process ID of the meterpreter on the target system.

`ps` - running process.

Even if we were to go a step further and look at DLLs (Dynamic-Link Libraries) used by the Meterpreter process (PID 1304 in this case), we still would not find anything jumping at us (e.g. no meterpreter.dll)

### 1.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

Techniques and tools that can be used to detect Meterpreter are beyond the scope of this room. This section aimed to show you how stealthy Meterpreter is running; remember, most antivirus software will detect it.

It is also worth noting that Meterpreter will establish an encrypted (TLS) communication channel with the attacker's system.

## Meterpreter Flavors
As discussed in the previous Metasploit rooms, linked below, Metasploit payloads can be initially divided into two categories; inline (also called single) and staged.

As you will remember, staged payloads are sent to the target in two steps. An initial part is installed (the stager) and requests the rest of the payload. This allows for a smaller initial payload size. The inline payloads are sent in a single step. Meterpreter payloads are also divided into stagged and inline versions. However, Meterpreter has a wide range of different versions you can choose from based on your target system. 

The easiest way to have an idea about available Meterpreter versions could be to list them using msfvenom, as seen below. 

We have used the `msfvenom --list payloads` command and grepped "meterpreter" payloads (adding `| grep meterpreter` to the command line), so the output only shows these. You can try this command on the AttackBox. 

`msfvenom --list payloads | grep meterpreter`

## Meterpreter commands

`meterpreter > help` - to display all the meterpreter available commands.

## Post-Exploitation with Meterpreter

`getuid` -  it will display the user with which Meterpreter is currently running. This will give you an idea of your possible privilege level on the target system (e.g. Are you an admin level user like NT AUTHORITY\SYSTEM or a regular user?)

`ps` - display all the process.

`migrate` - to migrate from another process.

`hashdump` - to dump all the hashes.

`search -f flag2.txt` - to search the file.

`shell` - to interact with target system shell.

`load kiwi` - to add the module.

