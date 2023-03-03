
# Metasploit: Introduction

Metasploit is the most widely used exploitation framework. Metasploit is a powerful tool that can support all phases of a penetration testing engagement, from information gathering to post-exploitation.

Metasploit has two main versions:

* Metasploit Pro: The commercial version that facilitates the automation and management of tasks. This version has a graphical user interface (GUI).
* Metasploit Framework: The open-source version that works from the command line. This room will focus on this version, installed on the AttackBox and most commonly used penetration testing Linux distributions.

The Metasploit Framework is a set of tools that allow information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While the primary usage of the Metasploit Framework focuses on the penetration testing domain, it is also useful for vulnerability research and exploit development.

The main components of the Metasploit Framework can be summarized as follows;

* msfconsole: The main command-line interface.
* Modules: supporting modules such as exploits, scanners, payloads, etc.
* Tools: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing. Some of these tools are msfvenom, pattern_create and pattern_offset. We will cover msfvenom within this module, but pattern_create and pattern_offset are tools useful in exploit development which is beyond the scope of this module.

This room will cover the main components of Metasploit while providing you with a solid foundation on how to find relevant exploits, set parameters, and exploit vulnerable services on the target system. Once you have completed this room, you will be able to navigate and use the Metasploit command line comfortably.

# Main components of Metasploit

While using the Metasploit Framework, you will primarily interact with the Metasploit console. You can launch it from the AttackBox terminal using the `msfconsole` command. The console will be your main interface to interact with the different modules of the Metasploit Framework. Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

Before diving into modules, it would be helpful to clarify a few recurring concepts: vulnerability, exploit, and payload.

* Exploit: A piece of code that uses a vulnerability present on the target system.
* Vulnerability: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
* Payload: An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

Modules and categories under each one are listed below. These are given for reference purposes, but you will interact with them through the Metasploit console (msfconsole).

### Auxiliary: Any supporting module, such as scanners, crawlers and fuzzers, can be found here.

###  auxiliary.PNG

![App Screenshot](https://github.com/rohit00712/Jr-Penetration-Tester/blob/main/Metasploit/1.%20Metasploit-Introduction/images/auxiliary.PNG)

### Encoders: Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.

Signature-based antivirus and security solutions have a database of known threats. They detect threats by comparing suspicious files to this database and raise an alert if there is a match. Thus encoders can have a limited success rate as antivirus solutions can perform additional checks.

###  encoders.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

### Evasion: While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software.

On the other hand, “evasion” modules will try that, with more or less success.

###  evasion.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

### Exploits: Exploits, neatly organized by target system.

###  exploits.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

### NOPs: NOPs (No OPeration) do nothing, literally.

They are represented in the Intel x86 CPU family they are represented with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.

###  nops.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

### Payloads: Payloads are codes that will run on the target system.

Exploits will leverage a vulnerability on the target system, but to achieve the desired result, we will need a payload. Examples could be; getting a shell, loading a malware or backdoor to the target system, running a command, or launching calc.exe as a proof of concept to add to the penetration test report. Starting the calculator on the target system remotely by launching the calc.exe application is a benign way to show that we can run commands on the target system.

Running command on the target system is already an important step but having an interactive connection that allows you to type commands that will be executed on the target system is better. Such an interactive command line is called a "shell". Metasploit offers the ability to send different payloads that can open shells on the target system.

###  payloads.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

You will see three different directories under payloads: singles, stagers and stages.

* `Singles:` Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
* `Stagers:` Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
* `Stages:` Downloaded by the stager. This will allow you to use larger sized payloads.

Metasploit has a subtle way to help you identify single (also called “inline”) payloads and staged payloads.

* `generic/shell_reverse_tcp`
* `windows/x64/shell/reverse_tcp`

Both are reverse Windows shells. The former is an inline (or single) payload, as indicated by the `“_”` between `“shell”` and `“reverse”`. While the latter is a staged payload, as indicated by the `“/”` between `“shell”` and `“reverse”`.

### Post: Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.

###  post.PNG

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

# msfconsole 

`ls` -  which lists the contents of the folder from which Metasploit was launched using the `msfconsole` command.

`ping` - `ping -c 1 8.8.8.8` -c is use to send only one ping

`help <cmd>` -  the help command can be used on its own or for a specific command.

`history` -  to see commands you have typed earlier.

`show payloads` - to list all the available payloads.

`back` - You can leave the context using the `back` command.

`info` - Further information on any module can be obtained by typing the `info` command within its context.

`info exploit/windows/smb/ms17_010_eternalblue` - Alternatively, you can use the `info` command followed by the module’s path from the msfconsole prompt.

`search ms17-010` - You can conduct searches using CVE numbers, exploit names (eternalblue, heartbleed, etc.), or target system.

`search type:auxiliary telnet` - You can direct the search function using keywords such as type and platform.

# Working with modules

`show options` - to view the options available on the module.

`set <cmd>` - to set the value

`unset <cmd>` - to unset the value

`unset` - to unset all parameters to default again.

`setg` - to set the value globally.

`unsetg` - to unset the global value.

`back` - to leave the exploit content.

`exploit` - to run the exploit.

`exploit -z` - to run and background the session.

`check` - Some modules support the check option. This will check if the target system is vulnerable without exploiting it.

`background` - to background the session.

`sessions` - list all the available sessions.

`sessions -i 2` - to interact with sessions.









