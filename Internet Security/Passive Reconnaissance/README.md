
# Passive Reconnaissance

In this room, after we define passive reconnaissance and active reconnaissance, we focus on essential tools related to passive reconnaissance. We will learn three command-line tools:

* `whois` to query WHOIS servers
* `nslookup` to query DNS servers
* `dig` to query DNS servers

We use `whois` to query WHOIS records, while we use `nslookup` and `dig` to query DNS database records. These are all publicly available records and hence do not alert the target.

We will also learn the usage of two online services:

* `DNSDumpster`
* `Shodan.io`
These two online services allow us to collect information about our target without directly connecting to it.

# Passive Versus Active Recon

Before the dawn of computer systems and networks, in the Art of War, Sun Tzu taught, “If you know the enemy and know yourself, your victory will not stand in doubt.” If you are playing the role of an attacker, you need to gather information about your target systems. If you are playing the role of a defender, you need to know what your adversary will discover about your systems and networks.

Reconnaissance (recon) can be defined as a preliminary survey to gather information about a target. It is the first step in The Unified Kill Chain to gain an initial foothold on a system. We divide reconnaissance into:

    1. Passive Reconnaissance

    2. Active Reconnaissance

In passive reconnaissance, you rely on publicly available knowledge. It is the knowledge that you can access from publicly available resources without directly engaging with the target. Think of it like you are looking at target territory from afar without stepping foot on that territory.

### passive.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

Passive reconnaissance activities include many activities, for instance:

* Looking up DNS records of a domain from a public DNS server.
* Checking job ads related to the target website.
* Reading news articles about the target company.

Active reconnaissance, on the other hand, cannot be achieved so discreetly. It requires direct engagement with the target. Think of it like you check the locks on the doors and windows, among other potential entry points

### active.png

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

Examples of active reconnaissance activities include:

* Connecting to one of the company servers such as HTTP, FTP, and SMTP.
* Calling the company in an attempt to get information (social engineering).
* Entering company premises pretending to be a repairman.

Considering the invasive nature of active reconnaissance, one can quickly get into legal trouble unless one obtains proper legal authorisation.

#  Whois

WHOIS is a request and response protocol that follows the RFC 3912 specification. A WHOIS server listens on TCP port 43 for incoming requests. The domain registrar is responsible for maintaining the WHOIS records for the domain names it is leasing. The WHOIS server replies with various information related to the domain requested. Of particular interest, we can learn:

* Registrar: Via which registrar was the domain name registered?
* Contact info of registrant: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
* Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
* Name Server: Which server to ask to resolve the domain name?

`whois DOMAIN_NAME`

`whois tryhackme.com`

# nslookup and dig

In the previous task, we used the WHOIS protocol to get various information about the domain name we were looking up. In particular, we were able to get the DNS servers from the registrar.

Find the IP address of a domain name using `nslookup`, which stands for Name Server Look Up. You need to issue the command `nslookup DOMAIN_NAME`, for example, `nslookup tryhackme.com`. Or, more generally, you can use `nslookup OPTIONS DOMAIN_NAME SERVER`. These three main parameters are:

* OPTIONS contains the query type as shown in the table below. For instance, you can use `A` for IPv4 addresses and `AAAA` for IPv6 addresses.
* `DOMAIN_NAME` is the domain name you are looking up.
* SERVER is the DNS server that you want to query. You can choose any local or public DNS server to query. Cloudflare offers `1.1.1.1` and `1.0.0.1`, Google offers `8.8.8.8` and `8.8.4.4`, and Quad9 offers `9.9.9.9` and `149.112.112.112`. There are many more public DNS servers that you can choose from if you want alternatives to your ISP’s DNS servers.

| Query Type |        Result                               |
| :-------- | :-------------------------------------| 
| **A** | IPv4 Addresses |
| **AAA** | IPV6 Addresses |
| **CNAME** | Canonical Name |
| **MX** | Mail Servers |
| **SOA** | Start of Authority |
| **TXT** | TXT Records |

`nslookup -type=A tryhackme.com 1.1.1.1`

`nslookup -type=MX tryhackme.com`

`nslookup -type=TXT tryhackme.com`

For more advanced DNS queries and additional functionality, you can use `dig`, the acronym for “Domain Information Groper,” if you are curious. Let’s use `dig` to look up the MX records and compare them to `nslookup`. We can use `dig DOMAIN_NAME`, but to specify the record type, we would use `dig DOMAIN_NAME TYPE`. Optionally, we can select the server we want to query using `dig @SERVER DOMAIN_NAME TYPE`.

* SERVER is the DNS server that you want to query.
* DOMAIN_NAME is the domain name you are looking up.
* TYPE contains the DNS record type, as shown in the table provided earlier.

`dig tryhackme.com MX`

`dig @1.1.1.1 tryhackme.com MX`

# DNSDumpster

DNS lookup tools, such as nslookup and dig, cannot find subdomains on their own. The domain you are inspecting might include a different subdomain that can reveal much information about the target. For instance, if tryhackme.com has the subdomains wiki.tryhackme.com and webmail.tryhackme.com, you want to learn more about these two as they can hold a trove of information about your target. There is a possibility that one of these subdomains has been set up and is not updated regularly. Lack of proper regular updates usually leads to vulnerable services. But how can we know that such subdomains exist?

```https
https://dnsdumpster.com/
```
# Shodan.io

When you are tasked to run a penetration test against specific targets, as part of the passive reconnaissance phase, a service like Shodan.io can be helpful to learn various pieces of information about the client’s network, without actively connecting to it. Furthermore, on the defensive side, you can use different services from Shodan.io to learn about connected and exposed devices belonging to your organization.

Shodan.io tries to connect to every device reachable online to build a search engine of connected “things” in contrast with a search engine for web pages. Once it gets a response, it collects all the information related to the service and saves it in the database to make it searchable. Consider the saved record of one of tryhackme.com’s servers.

`https://www.shodan.io/`








## Screenshots

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


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

