
# Active Reconnaissance

we focus on active reconnaissance and the essential tools related to it. We learn to use a web browser to collect more information about our target. Moreover, we discuss using simple tools such as `ping`, `traceroute`, `telnet`, and `nc` to gather information about the network, system, and services.

# Web browser

The web browser can be a convenient tool, especially that it is readily available on all systems. There are several ways where you can use a web browser to gather information about a target.

On the transport level, the browser connects to:

TCP port 80 by default when the website is accessed over HTTP
TCP port 443 by default when the website is accessed over HTTPS
Since 80 and 443 are default ports for HTTP and HTTPS, the web browser does not show them in the address bar. However, it is possible to use custom ports to access a service. For instance, https://127.0.0.1:8834/ will connect to 127.0.0.1 (localhost) at port 8834 via HTTPS protocol. If there is an HTTPS server listening on that port, we will receive a web page.

* **Developer Tools** : `Ctrl+Shift+I` on a PC

There are also plenty of add-ons for Firefox and Chrome that can help in penetration testing. Here are a few examples:

* **FoxyProxy** lets you quickly change the proxy server you are using to access the target website. This browser extension is convenient when you are using a tool such as Burp Suite or if you need to switch proxy servers regularly. You can get FoxyProxy for Firefox from here.

* **User-Agent Switcher and Manager** gives you the ability to pretend to be accessing the webpage from a different operating system or different web browser. In other words, you can pretend to be browsing a site using an iPhone when in fact, you are accessing it from Mozilla Firefox. You can download User-Agent Switcher and Manager for Firefox here.

* **Wappalyzer** provides insights about the technologies used on the visited websites. Such extension is handy, primarily when you collect all this information while browsing the website like any other user. A screenshot of Wappalyzer is shown below. You can find Wappalyzer for Firefox here.

Over time, you might find a few extensions that fit perfectly in your workflow.

# Ping

Ping should remind you of the game ping-pong (table tennis). You throw the ball and expect to get it back. The primary purpose of ping is to check whether you can reach the remote system and that the remote system can reach you back. In other words, initially, this was used to check network connectivity; however, we are more interested in its different uses: checking whether the remote system is online.

`ping MACHINE_IP` or `ping HOSTNAME`

`ping -c 10 MACHINE_IP` : for Linux_Machine

`ping -n 10 MACHINE_IP` : for MS Windows_Machine

# Traceroute

As the name suggests, the traceroute command traces the route taken by the packets from your system to another host. The purpose of a traceroute is to find the IP addresses of the routers or hops that a packet traverses as it goes from your system to a target host. This command also reveals the number of routers between the two systems. It is helpful as it indicates the number of hops (routers) between your system and the target host. However, note that the route taken by the packets might change as many routers use dynamic routing protocols that adapt to network changes.

`traceroute MACHINE_IP` : On Linux and MAC 

`tracert MACHINE_IP` : On MS Windows_Machine 

# Telnet

The TELNET (Teletype Network) protocol was developed in 1969 to communicate with a remote system via a command-line interface (CLI). Hence, the command `telnet` uses the TELNET protocol for remote administration. The default port used by `telnet` is 23. From a security perspective, telnet sends all the data, including usernames and passwords, in cleartext. Sending in cleartext makes it easy for anyone, who has access to the communication channel, to steal the login credentials. The secure alternative is SSH (Secure SHell) protocol.

However, the telnet client, with its simplicity, can be used for other purposes. Knowing that telnet client relies on the TCP protocol, you can use Telnet to connect to any service and grab its banner. Using `telnet 10.10.82.204 PORT`, you can connect to any service running on TCP and even exchange a few messages unless it uses encryption.

Let’s say we want to discover more information about a web server, listening on port 80. We connect to the server at port 80, and then we communicate using the HTTP protocol. You don’t need to dive into the HTTP protocol; you just need to issue `GET / HTTP/1.1`. To specify something other than the default index page, you can issue `GET /page.html HTTP/1.1`, which will request `page.html`. We also specified to the remote web server that we want to use HTTP version 1.1 for communication. To get a valid response, instead of an error, you need to input some value for the host `host: example` and hit enter twice. Executing these steps will provide the requested index page.

`telnet 10.10.82.204 80`

# Netcat

Netcat or simply `nc` has different applications that can be of great value to a pentester. Netcat supports both TCP and UDP protocols. It can function as a client that connects to a listening port; alternatively, it can act as a server that listens on a port of your choice. Hence, it is a convenient tool that you can use as a simple client or server over TCP or UDP.

First, you can connect to a server, as you did with Telnet, to collect its banner using `nc 10.10.82.204 PORT`, which is quite similar to our previous `telnet 10.10.82.204 PORT`. Note that you might need to press SHIFT+ENTER after the GET line.

`nc 10.10.82.204 80`

In the terminal shown above, we used netcat to connect to 10.10.82.204 port 80 using `nc 10.10.82.204 80`. Next, we issued a get for the default page using `GET / HTTP/1.1`; we are specifying to the target server that our client supports HTTP version 1.1. Finally, we need to give a name to our host, so we added on a new line, `host: netcat`; you can name your host anything as this has no impact on this exercise.

Based on the output `Server: nginx/1.6.2` we received, we can tell that on port 80, we have Nginx version 1.6.2 listening for incoming connections.

You can use netcat to listen on a TCP port and connect to a listening port on another system.

On the server system, where you want to open a port and listen on it, you can issue `nc -lp 1234` or better yet, `nc -vnlp 1234`, which is equivalent to `nc -v -l -n -p 1234`, as you would remember from the Linux room. The exact order of the letters does not matter as long as the port number is preceded directly by `-p`.

| option | Meaning     | 
| :-------- | :------- | 
| -l | Listen mode |
| -p | Specify the Port number          |
| -n |  Numeric only; no resolution of hostnames via DNS        |
| -v |  Verbose output (optional, yet useful to discover any bugs)         |
| -vv | Very Verbose (optional)         |
| -k |   Keep listening after client disconnects        |

Notes:

* the option `-p` should appear just before the port number you want to listen on.
* the option `-n` will avoid DNS lookups and warnings.
* port numbers less than 1024 require root privileges to listen on.







## API Reference

#### Get all items

```http
  GET /api/items
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Your API key |

#### Get item

```http
  GET /api/items/${id}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id`      | `string` | **Required**. Id of item to fetch |

#### add(num1, num2)

Takes two numbers and returns the sum.

