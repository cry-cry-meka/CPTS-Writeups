# A little Introduction : 

## what is Enumeration ?

It stands for information gathering using Active&Passive scan methods.

>Note: As penetration testers, our goal is not just to find a single entry point, but to identify every possible path an attacker could take to gain unauthorized access to the system. For example initial entry points, escalation techniques, and post exploitation activities.

### Questions to Answer

1. What can we see ? 
2. What reasons can we have for seeing it ? 
3. what impression does the observed information give us?
4. What do we gain from it ?
5. How can we use it ?
6. What can we not see ?
7. What reasons can there be that we do not see ?
9. What image results for us from what we do not see ?

### Principles you should consider

|   | Principle |
|---|---|
| 1 | There is more than meets the eye. Consider all points of view|
| 2 | Distinguish between what we see and what we don't see|
| 3 | There are always ways to gain more information. Understand the Target|

---

# Enumeration Methodology :

> Complex processes must have a standardized methodology that helps us keep our bearings and avoid overlooking any aspect.

Most penetration testers rely on personal habits and the steps they are most familiar with. This approach is based on experience rather than a standardized methodology.

![](images/enum-method.png)

> Note: The components of each layer shown represent the main categories and not a full list of all the components to search for. Additionally, it must be mentioned here that the first and second layer (Internet Presence, Gateway) does not quite apply to the intranet, such as an Active Directory infrastructure. The layers for internal infrastructure will be covered in other modules.


### The whole enumeration process is divided into three different levels :

- Infrastructure-based enumeration	
- Host-based enumeration	
- OS-based enumeration

## Enumeration Layers

| Layer   | Name                 | Description                                                                     | Information Categories                                                                     |
|---------|----------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| 1       | 🌐 Internet Presence | Identification of internet presence and externally accessible infrastructure    | Domains, Subdomains, vHosts, ASN, Netblocks, IPs, Cloud Instances, Security Measures       |
| 2       | 🛡️ Gateway           | Identify security measures protecting external and internal infrastructure      | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Segmentation, VPN, Cloudflare                  |
| 3       | 🔌 Services          | Identify externally and internally accessible services and interfaces           | Service Type, Functionality, Configuration, Port, Version, Interface                       |
| 4       | ⚙️ Processes         | Identify internal processes, data flow, and communication paths                 | PID, Processed Data, Tasks, Source, Destination                                            |
| 5       | 🔑 Privileges        | Identify permissions and privilege levels across services                       | Groups, Users, Permissions, Restrictions, Environment                                      |
| 6       | 🖥️ OS Setup          | Identify system components and operating system configuration                   | OS Type, Patch Level, Network Config, Environment, Config Files, Sensitive Files           |

<br>

#### ***🌐 Layer No.1: Internet Presence***

<p>The first layer we have to pass is the "Internet Presence" layer, where we focus on finding the targets we can investigate. If the scope in the contract allows us to look for additional hosts, this layer is even more critical than for fixed targets only. In this layer, we use different techniques to find domains, subdomains, netblocks, and many other components and information that present the presence of the company and its infrastructure on the Internet.</p>

```The goal of this layer is to identify all possible target systems and interfaces that can be tested.```

#### ***🛡️ Layer No.2: Gateway***

<p>Here we try to understand the interface of the reachable target, how it is protected, and where it is located in the network. Due to the diversity, different functionalities, and some particular procedures, we will go into more detail about this layer in other modules.</p>

```The goal is to understand what we are dealing with and what we have to watch out for.```

#### ***🔌 Layer No.3: Accessible Services***

<p>In the case of accessible services, we examine each destination for all the services it offers. Each of these services has a specific purpose that has been installed for a particular reason by the administrator. Each service has specific functions that produce distinct outcomes. To work effectively with them, we need to know how they work. Otherwise, we need to learn to understand them.</p>

```This layer aims to understand the reason and functionality of the target system and gain the necessary knowledge to communicate with it and exploit it for our purposes effectively.```


#### ***⚙️ Layer No.4: Processes***

<p>Every time a command or function is executed, data is processed, whether entered by the user or generated by the system. This starts a process that has to perform specific tasks, and such tasks have at least one source and one target.</p>

```The goal here is to understand these factors and identify the dependencies between them.```

#### ***🔑 Layer No.5: Privileges***

<p>Each service runs through a specific user in a particular group with permissions and privileges defined by the administrator or the system. These privileges often provide us with functions that administrators overlook. This often happens in Active Directory infrastructures and many other case-specific administration environments and servers where users are responsible for multiple administration areas.</p>

```It is crucial to identify these and understand what is and is not possible with these privileges.```

#### ***🖥️ Layer No.6: OS Setup***

<p>Here we collect information about the actual operating system and its setup using internal access. This gives us a good overview of the internal security of the systems and reflects the skills and capabilities of the company's administrative teams.</p>

```The goal here is to see how the administrators manage the systems and what sensitive internal information we can glean from them.```


<br>

#### Enumeration Methodology in Practice
- We can finally imagine the entire penetration test in the form of a labyrinth where we have to identify the gaps and find the way to get us inside as quickly and effectively as possible. This type of labyrinth may look something like this:
> NOTE: The squares represent the gaps/vulnerabilities.

![](images/pentest-labyrinth.png)

<br>


# Infrastructure Based Enumeration
<br>

## Domain Information
Domain information it is not just about the subdomains but about the entire presence on the Internet. Therefore, we gather information and try to understand the company's functionality and which technologies and structures are necessary for services to be offered successfully and efficiently. ***This type of information is gathered passively without direct and active scans.***

the first thing we should do is scrutinize the company's main website. Then, we should read through the texts, keeping in mind what technologies and structures are needed for these services.

### Online Presence
Once we have a basic understanding of the company and its services, we can get a first impression of its presence on the Internet.

1. The first point of presence on the Internet may be the SSL certificate from the company's main website that we can examine. Often, such a certificate includes more than just a subdomain, and this means that the certificate is used for several domains, and these are most likely still active.

2. Another source to find more subdomains is crt.sh. This source is Certificate Transparency logs. 

#### What is a Certificate Transparency logs ?

 **It's a process that is intended to enable the verification of issued digital certificates for encrypted Internet connections.** <br>The standard (RFC 6962) provides for the logging of all digital certificates issued by a certificate authority in audit-proof logs.<br> 
 This is intended to enable the detection of false or maliciously issued certificates for a domain. SSL certificate providers like Let's Encrypt share this with the web interface crt.sh, which stores the new entries in the database to be accessed later.
 ![](images/DomInfo-2.png)

 <br>

-  We can also output the results in JSON format.

```
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .

[
  {
    "issuer_ca_id": 23451835427,
    "issuer_name": "C=US, O=Let's Encrypt, CN=R3",
    "common_name": "matomo.inlanefreight.com",
    "name_value": "matomo.inlanefreight.com",
    "id": 50815783237226155,
    "entry_timestamp": "2021-08-21T06:00:17.173",
    "not_before": "2021-08-21T05:00:16",
    "not_after": "2021-11-19T05:00:15",
    "serial_number": "03abe9017d6de5eda90"
  },
  {
    "issuer_ca_id": 6864563267,
    "issuer_name": "C=US, O=Let's Encrypt, CN=R3",
    "common_name": "matomo.inlanefreight.com",
    "name_value": "matomo.inlanefreight.com",
    "id": 5081529377,
    "entry_timestamp": "2021-08-21T06:00:16.932",
    "not_before": "2021-08-21T05:00:16",
    "not_after": "2021-11-19T05:00:15",
    "serial_number": "03abe90104e271c98a90"
  },
  {
    "issuer_ca_id": 113123452,
    "issuer_name": "C=US, O=Let's Encrypt, CN=R3",
    "common_name": "smartfactory.inlanefreight.com",
    "name_value": "smartfactory.inlanefreight.com",
    "id": 4941235512141012357,
    "entry_timestamp": "2021-07-27T00:32:48.071",
    "not_before": "2021-07-26T23:32:47",
    "not_after": "2021-10-24T23:32:45",
    "serial_number": "044bac5fcc4d59329ecbbe9043dd9d5d0878"
  },
  { ... SNIP ...
  ```

  <br>

  - If needed, we can also have them filtered by the unique subdomains.
  ```
  curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u

account.ttn.inlanefreight.com
blog.inlanefreight.com
bots.inlanefreight.com
console.ttn.inlanefreight.com
ct.inlanefreight.com
data.ttn.inlanefreight.com
*.inlanefreight.com
inlanefreight.com
integrations.ttn.inlanefreight.com
iot.inlanefreight.com
mails.inlanefreight.com
marina.inlanefreight.com
marina-live.inlanefreight.com
matomo.inlanefreight.com
next.inlanefreight.com
noc.ttn.inlanefreight.com
preview.inlanefreight.com
shop.inlanefreight.com
smartfactory.inlanefreight.com
ttn.inlanefreight.com
vx.inlanefreight.com
www.inlanefreight.com
  ```

<br>

- we can identify the hosts directly accessible from the Internet and not hosted by third-party providers.

```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done

blog.inlanefreight.com 10.129.24.93
inlanefreight.com 10.129.27.33
matomo.inlanefreight.com 10.129.127.22
www.inlanefreight.com 10.129.127.33
s3-website-us-west-2.amazonaws.com 10.129.95.250
```

<br>

Once we see which hosts can be investigated further, we can generate a list of IP addresses with a minor adjustment to the cut command and run them through Shodan.

> You can skip this if you want
>>Shodan can be used to find devices and systems permanently connected to the Internet like Internet of Things (IoT).<br> It searches the Internet for open TCP/IP ports and filters the systems according to specific terms and criteria. <br> For example, open HTTP or HTTPS ports and other server ports for FTP, SSH, SNMP, Telnet, RTSP, or SIP are searched. <br> As a result, we can find devices and systems, such as surveillance cameras, servers, smart home systems, industrial controllers, traffic lights and traffic >>controllers, and various network components.

<br>

- Shodan - IP List

1.IP Extraction
```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
```
2.Enumeration using Shodan
```
for i in $(cat ip-addresses.txt);do shodan host $i;done

10.129.24.93
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-09-01T09:02:11.370085
Number of open ports:    2

Ports:
     80/tcp nginx 
    443/tcp nginx 
    
10.129.27.33
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-08-30T22:25:31.572717
Number of open ports:    3

Ports:
     22/tcp OpenSSH (7.6p1 Ubuntu-4ubuntu0.3)
     80/tcp nginx 
    443/tcp nginx 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, -TLSv1.3, TLSv1.2
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
                
10.129.27.22
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-09-01T15:39:55.446281
Number of open ports:    8

Ports:
     25/tcp  
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3
     53/tcp  
     53/udp  
     80/tcp Apache httpd 
     81/tcp Apache httpd 
    110/tcp  
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2
    111/tcp  
    443/tcp Apache httpd 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
                Fingerprint:   RFC3526/Oakley Group 14
    444/tcp  
        
10.129.27.33
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-08-30T22:25:31.572717
Number of open ports:    3

Ports:
     22/tcp OpenSSH (7.6p1 Ubuntu-4ubuntu0.3)
     80/tcp nginx 
    443/tcp nginx 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, -TLSv1.3, TLSv1.2
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
```
<br>

- we can display all the available DNS records where we might find more hosts.
```
dig any inlanefreight.com

; <<>> DiG 9.16.1-Ubuntu <<>> any inlanefreight.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52058
;; flags: qr rd ra; QUERY: 1, ANSWER: 17, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;inlanefreight.com.             IN      ANY

;; ANSWER SECTION:
inlanefreight.com.      300     IN      A       10.129.27.33
inlanefreight.com.      300     IN      A       10.129.95.250
inlanefreight.com.      3600    IN      MX      1 aspmx.l.google.com.
inlanefreight.com.      3600    IN      MX      10 aspmx2.googlemail.com.
inlanefreight.com.      3600    IN      MX      10 aspmx3.googlemail.com.
inlanefreight.com.      3600    IN      MX      5 alt1.aspmx.l.google.com.
inlanefreight.com.      3600    IN      MX      5 alt2.aspmx.l.google.com.
inlanefreight.com.      21600   IN      NS      ns.inwx.net.
inlanefreight.com.      21600   IN      NS      ns2.inwx.net.
inlanefreight.com.      21600   IN      NS      ns3.inwx.eu.
inlanefreight.com.      3600    IN      TXT     "MS=ms92346782372"
inlanefreight.com.      21600   IN      TXT     "atlassian-domain-verification=IJdXMt1rKCy68JFszSdCKVpwPN"
inlanefreight.com.      3600    IN      TXT     "google-site-verification=O7zV5-xFh_jn7JQ31"
inlanefreight.com.      300     IN      TXT     "google-site-verification=bow47-er9LdgoUeah"
inlanefreight.com.      3600    IN      TXT     "google-site-verification=gZsCG-BINLopf4hr2"
inlanefreight.com.      3600    IN      TXT     "logmein-verification-code=87123gff5a479e-61d4325gddkbvc1-b2bnfghfsed1-3c789427sdjirew63fc"
inlanefreight.com.      300     IN      TXT     "v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com include:_spf.atlassian.net ip4:10.129.24.8 ip4:10.129.27.2 ip4:10.72.82.106 ~all"
inlanefreight.com.      21600   IN      SOA     ns.inwx.net. hostmaster.inwx.net. 2021072600 10800 3600 604800 3600

;; Query time: 332 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Mi Sep 01 18:27:22 CEST 2021
;; MSG SIZE  rcvd: 940
```

- DNS (Domain Name System) records

  - A records: We recognize the IP addresses that point to a specific (sub)domain through the A record. Here we only see one that we already know.
  - MX records: The mail server records show us which mail server is responsible for managing the emails for the company. Since this is handled by google in our case, we should note this and skip it for now.
  - NS records: These kinds of records show which name servers are used to resolve the FQDN to IP addresses. Most hosting providers use their own name servers, making it easier to identify the hosting provider.
  - TXT records: this type of record often contains verification keys for different third-party providers and other security aspects of DNS, such as SPF, DMARC, and DKIM, which are responsible for verifying and confirming the origin of the emails sent. Here we can already see some valuable information if we look closer at the results.



<br>

# Cloud Resources

The use of cloud, such as AWS, GCP, Azure, and others, is now one of the essential components for many companies nowadays. After all, all companies want to be able to do their work from anywhere, so they need a central point for all management. This is why services from Amazon (AWS), Google (GCP), and Microsoft (Azure) are ideal for this purpose.

Even though cloud providers secure their infrastructure centrally, this does not mean that companies are free from vulnerabilities. The configurations made by the administrators may nevertheless make the company's cloud resources vulnerable. This often starts with the S3 buckets (AWS), blobs (Azure), cloud storage (GCP), which can be accessed without authentication if configured incorrectly.

```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done

blog.inlanefreight.com 10.129.24.93
inlanefreight.com 10.129.27.33
matomo.inlanefreight.com 10.129.127.22
www.inlanefreight.com 10.129.127.33
s3-website-us-west-2.amazonaws.com 10.129.95.250
```

<br>

Often cloud storage is added to the DNS list when used for administrative purposes by other employees. This step makes it much easier for the employees to reach and manage them. Let us stay with the case that a company has contracted us, and during the IP lookup, we have already seen that one IP address belongs to the s3-website-us-west-2.amazonaws.com server.

However, there are many different ways to find such cloud storage. One of the easiest and most used is Google search combined with Google Dorks. For example, we can use the Google Dorks inurl: and intext: to narrow our search to specific terms. In the following example, we see red censored areas containing the company name.

<br>

- Google Search for AWS
```
intext:website inurl:amazonaws.com
```

- Google Search for Azure

```
intext:website inurl:blob.core.windows.net
```

Such content is also often included in the source code of the web pages, from where the images, JavaScript codes, or CSS are loaded. This procedure often relieves the web server and does not store unnecessary content.

![](images/cloud3.png)

Third-party providers such as domain.glass can also tell us a lot about the company's infrastructure. As a positive side effect, we can also see that Cloudflare's security assessment status has been classified as "Safe". This means we have already found a security measure that can be noted for the second layer (gateway).

![](images/cloud1.png)


Another very useful provider is GrayHatWarfare. We can do many different searches, discover AWS, Azure, and GCP cloud storage, and even sort and filter by file format. Therefore, once we have found them through Google, we can also search for them on GrayHatWarefare and passively discover what files are stored on the given cloud storage.

![](images/cloud2.png)

Many companies also use abbreviations of the company name, which are then used accordingly within the IT infrastructure. Such terms are also part of an excellent approach to discovering new cloud storage from the company. We can also search for files simultaneously to see the files that can be accessed at the same time.

![](images/ghw1.png)

Sometimes when employees are overworked or under high pressure, mistakes can be fatal for the entire company. These errors can even lead to SSH private keys being leaked, which anyone can download and log onto one or even more machines in the company without using a password.

![](images/ghw2.png)


<br>

# Staff
<br>

Searching for and identifying employees on social media platforms can also reveal a lot about the teams' infrastructure and makeup. This, in turn, can lead to us identifying which technologies, programming languages, and even software applications are being used. To a large extent, we will also be able to assess each person's focus based on their skills.

Employees can be identified on various business networks such as LinkedIn or Xing. Job postings from companies can also tell us a lot about their infrastructure and give us clues about what we should be looking for.


#### LinkedIn - Job Post

```
Required Skills/Knowledge/Experience:

* 3-10+ years of experience on professional software development projects.

« An active US Government TS/SCI Security Clearance (current SSBI) or eligibility to obtain TS/SCI within nine months.
« Bachelor's degree in computer science/computer engineering with an engineering/math focus or another equivalent field of discipline.
« Experience with one or more object-oriented languages (e.g., Java, C#, C++).
« Experience with one or more scripting languages (e.g., Python, Ruby, PHP, Perl).
« Experience using SQL databases (e.g., PostgreSQL, MySQL, SQL Server, Oracle).
« Experience using ORM frameworks (e.g., SQLAIchemy, Hibernate, Entity Framework).
« Experience using Web frameworks (e.g., Flask, Django, Spring, ASP.NET MVC).
« Proficient with unit testing and test frameworks (e.g., pytest, JUnit, NUnit, xUnit).
« Service-Oriented Architecture (SOA)/microservices & RESTful API design/implementation.
« Familiar and comfortable with Agile Development Processes.
« Familiar and comfortable with Continuous Integration environments.
« Experience with version control systems (e.g., Git, SVN, Mercurial, Perforce).

Desired Skills/Knowledge/ Experience:

« CompTIA Security+ certification (or equivalent).
« Experience with Atlassian suite (Confluence, Jira, Bitbucket).
« Algorithm Development (e.g., Image Processing algorithms).
« Software security.
« Containerization and container orchestration (Docker, Kubernetes, etc.)
« Redis.
« NumPy.
```

From a job post like this, we can see, for example, which programming languages are preferred: Java, C#, C++, Python, Ruby, PHP, Perl. It also required that the applicant be familiar with different databases, such as: PostgreSQL, Mysql, and Oracle. In addition, we know that different frameworks are used for web application development, such as: Flask, Django, ASP.NET, Spring.

> You might also find sensitive information

<br>

# Host Based Enumeration
<br>

## FTP

### What is FTP ?

> FTP stands for File Transfer Protocol. It's a way to transfer files between computers over the internet.

**How it works:**
<br>
-  FTP runs on the application layer of the TCP/IP protocol stack.<br>
-  Two channels are opened in an FTP connection: a control channel and a data channel.<br>
-  The control channel is used to send commands and receive status codes, while the data channel is used for file transfers.

#### Types of FTP connections:

1. Active FTP: Client establishes the connection via TCP port 21, but can be blocked by firewalls.
2. Passive FTP: Server announces a port for the client to establish the data channel, avoiding firewall issues.

#### 3 FTP commands and status codes:

- Clients use commands to upload or download files, organize directories, etc.
- Servers respond with status codes indicating whether the command was successful.

Security considerations:

FTP is a clear-text protocol that can be sniffed if network conditions are right.
Servers may offer anonymous FTP with limited user options, but this poses security risks.


### What is TFTP?

Trivial File Transfer Protocol (TFTP) is a simpler protocol compared to FTP that allows file transfers between client and server processes.

#### TFTP Commands:

| Command | Description |
| --- | --- |
| `connect` | Sets the remote host, and optionally the port, for file transfers. |
| `get`   | Transfers a file or set of files from the remote host to the local host. |
| `put`   | Transfers a file or set of files from the local host onto the remote host. |
| `quit`  | Exits tftp. |
| `status`| Displays the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| `verbose`| Turns verbose mode, which displays additional information during file transfer, on or off. |


## Default Configuration

One of the most used FTP servers on Linux-based distributions is vsFTPd. The default configuration of vsFTPd can be found in `/etc/vsftpd.conf` , and some settings are already predefined by default. This is the [man page](http://vsftpd.beasts.org/vsftpd_conf.html) .

- Install vsFTPd

```
sudo apt install vsftpd
```

- vsFTPd Config File

```
cat /etc/vsftpd.conf | grep -v "#"
```
| **Setting** | **Description** |
| --- | --- |
| `listen=NO` | Run from inetd or as a standalone daemon? |
| `listen_ipv6=YES` | Listen on IPv6? |
| `anonymous_enable=NO` | Enable Anonymous access? |
| `local_enable=YES` | Allow local users to login? |
| `dirmessage_enable=YES` | Display active directory messages when users go into certain directories? |
| `use_localtime=YES` | Use local time? |
| `xferlog_enable=YES` | Activate logging of uploads/downloads? |
| `connect_from_port_20=YES` | Connect from port 20? |
| `secure_chroot_dir=/var/run/vsftpd/empty` | Name of an empty directory for secure chrooting |
| `pam_service_name=vsftpd` | This string is the name of the PAM service vsftpd will use. |
| `rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem` | Location of the RSA certificate to use for SSL encrypted connections |
| `rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key` | Location of the private key for SSL encrypted connections |
| `ssl_enable=NO` | Enable SSL encryption? |

<br>

In addition, there is a file called /etc/ftpusers that we also need to pay attention to, as this file is used to deny certain users access to the FTP service. In the following example, the users guest, john, and kevin are not permitted to log in to the FTP service, even if they exist on the Linux system.
- FTPUSERS
```
cat /etc/ftpusers

guest
john
kevin
```
<br>

## Dangerous Settings

There are many different security-related settings we can make on each FTP server. These can have various purposes, such as testing connections through the firewalls, testing routes, and authentication mechanisms. One of these authentication mechanisms is the anonymous user. This is often used to allow everyone on the internal network to share files and data without accessing each other's computers. With vsFTPd, the optional settings that can be added to the configuration file for the anonymous login look like this:

| Setting	                      | Description                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------------- |
| `anonymous_enable=YES`        |	Allowing anonymous login?                                                          |
| `anon_upload_enable=YES`      |	Allowing anonymous to upload files?                                                |
| `anon_mkdir_write_enable=YES` |	Allowing anonymous to create new directories?                                      |
| `no_anon_password=YES`        |	Do not ask anonymous for password?                                                 |
| `anon_root=/home/username/ftp`|	Directory for anonymous.                                                           |
| `write_enable=YES`            |	Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |



## Anonymous Login

Just use `anonymous` for username and password that's it

```
ftp 10.129.14.136

Connected to 10.129.14.136.
220 "Welcome to the HTB Academy vsFTP service."
Name (10.129.14.136:cry0l1t3): anonymous

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.


ftp> ls

200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
226 Directory send OK.

```
<br>

## vsFTPd Status

To get the first overview of the server's settings, we can use the following command:

```
ftp> status

Connected to 10.129.14.136.
No proxy connection.
Connecting using address family: any.
Mode: stream; Type: binary; Form: non-print; Structure: file
Verbose: on; Bell: off; Prompting: on; Globbing: on
Store unique: off; Receive unique: off
Case: off; CR stripping: on
Quote control characters: on
Ntrans: off
Nmap: off
Hash mark printing: off; Use of PORT cmds: on
Tick counter printing: off

```

## vsFTPd Detailed Output

Some commands should be used occasionally, as these will make the server show us more information that we can use for our purposes. These commands include debug and trace.

```
ftp> debug

Debugging on (debug=1).


ftp> trace

Packet tracing on.


ftp> ls

---> PORT 10,10,14,4,188,195
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
226 Directory send OK.
```


|Setting	              |Description                                                                       |
|-----------------------|----------------------------------------------------------------------------------|
|dirmessage_enable=YES 	| Show a message when they first enter a new directory?                            |
|chown_uploads=YES	    |Change ownership of anonymously uploaded files?                                   |
|chown_username=username|	User who is given ownership of anonymously uploaded files.                       |
|local_enable=YES 	    | Enable local users to login?                                                     |
|chroot_local_user=YES	| Place local users into their home directory?                                     |
|chroot_list_enable=YES |	Use a list of local users that will be placed in their home directory?           |
|hide_ids=YES	          | All user and group information in directory listings will be displayed as "ftp". |
|ls_recurse_enable=YES  | Allows the use of recurse listings.                                              |


In the following example, we can see that if the hide_ids=YES setting is present, the UID and GUID representation of the service will be overwritten, making it more difficult for us to identify with which rights these files are written and uploaded.

- Hiding IDs - YES

```
ftp> ls

---> TYPE A
200 Switching to ASCII mode.
ftp: setsockopt (ignored): Permission denied
---> PORT 10,10,14,4,223,101
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
-rw-rw-r--    1 ftp     ftp      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 ftp     ftp           41 Sep 14 16:45 Important Notes.txt
-rw-------    1 ftp     ftp            0 Sep 15 14:57 testupload.txt
226 Directory send OK.12
```

This setting is a security feature to prevent local usernames from being revealed. With the usernames, we could attack the services like FTP and SSH and many others with a brute-force attack in theory. However, in reality, fail2ban solutions are now a standard implementation of any infrastructure that logs the IP address and blocks all access to the infrastructure after a certain number of failed login attempts.

Another helpful setting we can use for our purposes is the ls_recurse_enable=YES. This is often set on the vsFTPd server to have a better overview of the FTP directory structure, as it allows us to see all the visible content at once.

- Recursive Listing

```
ftp> ls -R

---> PORT 10,10,14,4,222,149
200 PORT command successful. Consider using PASV.
---> LIST -R
150 Here comes the directory listing.
.:
-rw-rw-r--    1 ftp      ftp      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 ftp      ftp           41 Sep 14 16:45 Important Notes.txt
-rw-------    1 ftp      ftp            0 Sep 15 14:57 testupload.txt

./Clients:
drwx------    2 ftp      ftp          4096 Sep 16 18:04 HackTheBox
drwxrwxrwx    2 ftp      ftp          4096 Sep 16 18:00 Inlanefreight

./Clients/HackTheBox:
-rw-r--r--    1 ftp      ftp         34872 Sep 16 18:04 appointments.xlsx
-rw-r--r--    1 ftp      ftp        498123 Sep 16 18:04 contract.docx
-rw-r--r--    1 ftp      ftp        478237 Sep 16 18:04 contract.pdf
-rw-r--r--    1 ftp      ftp           348 Sep 16 18:04 meetings.txt

./Clients/Inlanefreight:
-rw-r--r--    1 ftp      ftp         14211 Sep 16 18:00 appointments.xlsx
-rw-r--r--    1 ftp      ftp         37882 Sep 16 17:58 contract.docx
-rw-r--r--    1 ftp      ftp            89 Sep 16 17:58 meetings.txt
-rw-r--r--    1 ftp      ftp        483293 Sep 16 17:59 proposal.pptx

./Documents:
-rw-r--r--    1 ftp      ftp         23211 Sep 16 18:05 appointments-template.xlsx
-rw-r--r--    1 ftp      ftp         32521 Sep 16 18:05 contract-template.docx
-rw-r--r--    1 ftp      ftp        453312 Sep 16 18:05 contract-template.pdf

./Employees:
226 Directory send OK.
```
<br>

## Download a File

Downloading files from such an FTP server is one of the main features, as well as uploading files created by us. This allows us, for example, to use LFI vulnerabilities to make the host execute system commands. Apart from the files, we can view, download and inspect. Attacks are also possible with the FTP logs, leading to Remote Command Execution (RCE). This applies to the FTP services and all those we can detect during our enumeration phase.


```
ftp> ls

200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rwxrwxrwx    1 ftp      ftp             0 Sep 16 17:24 Calendar.pptx
drwxrwxrwx    4 ftp      ftp          4096 Sep 16 17:57 Clients
drwxrwxrwx    2 ftp      ftp          4096 Sep 16 18:05 Documents
drwxrwxrwx    2 ftp      ftp          4096 Sep 16 17:24 Employees
-rwxrwxrwx    1 ftp      ftp            41 Sep 18 15:58 Important Notes.txt
226 Directory send OK.


ftp> get Important\ Notes.txt

local: Important Notes.txt remote: Important Notes.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for Important Notes.txt (41 bytes).
226 Transfer complete.
41 bytes received in 0.00 secs (606.6525 kB/s)


ftp> exit

221 Goodbye.
```

- Read file

```
ls | grep Notes.txt

'Important Notes.txt'
```

## Download All Available Files

We also can download all the files and folders we have access to at once. This is especially useful if the FTP server has many different files in a larger folder structure. However, this can cause alarms because no one from the company usually wants to download all files and content all at once.

```
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136

--2021-09-19 14:45:58--  ftp://anonymous:*password*@10.129.14.136/                                         
           => ‘10.129.14.136/.listing’                                                                     
Connecting to 10.129.14.136:21... connected.                                                               
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD not needed.
==> PORT ... done.    ==> LIST ... done.                                                                 
12.12.1.136/.listing           [ <=>                                  ]     466  --.-KB/s    in 0s       
                                                                                                         
2021-09-19 14:45:58 (65,8 MB/s) - ‘10.129.14.136/.listing’ saved [466]                                     
--2021-09-19 14:45:58--  ftp://anonymous:*password*@10.129.14.136/Calendar.pptx   
           => ‘10.129.14.136/Calendar.pptx’                                       
==> CWD not required.                                                           
==> SIZE Calendar.pptx ... done.                                                                                                                            
==> PORT ... done.    ==> RETR Calendar.pptx ... done.       

...SNIP...

2021-09-19 14:45:58 (48,3 MB/s) - ‘10.129.14.136/Employees/.listing’ saved [119]

FINISHED --2021-09-19 14:45:58--
Total wall clock time: 0,03s
Downloaded: 15 files, 1,7K in 0,001s (3,02 MB/s)
```

Once we have downloaded all the files, wget will create a directory with the name of the IP address of our target. All downloaded files are stored there, which we can then inspect locally.

```
tree .

.
└── 10.129.14.136
    ├── Calendar.pptx
    ├── Clients
    │   └── Inlanefreight
    │       ├── appointments.xlsx
    │       ├── contract.docx
    │       ├── meetings.txt
    │       └── proposal.pptx
    ├── Documents
    │   ├── appointments-template.xlsx
    │   ├── contract-template.docx
    │   └── contract-template.pdf
    ├── Employees
    └── Important Notes.txt

5 directories, 9 files
```

Next, we can check if we have the permissions to upload files to the FTP server. Especially with web servers, it is common that files are synchronized, and the developers have quick access to the files. FTP is often used for this purpose, and most of the time, configuration errors are found on servers that the administrators think are not discoverable. The attitude that internal network components cannot be accessed from the outside means that the hardening of internal systems is often neglected and leads to misconfigurations.

The ability to upload files to the FTP server connected to a web server increases the likelihood of gaining direct access to the webserver and even a reverse shell that allows us to execute internal system commands and perhaps even escalate our privileges.

## Upload a File
```
touch testupload.txt
```

With the PUT command, we can upload files in the current folder to the FTP server.

```
ftp> put testupload.txt 

local: testupload.txt remote: testupload.txt
---> PORT 10,10,14,4,184,33
200 PORT command successful. Consider using PASV.
---> STOR testupload.txt
150 Ok to send data.
226 Transfer complete.


ftp> ls

---> TYPE A
200 Switching to ASCII mode.
---> PORT 10,10,14,4,223,101
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
-rw-------    1 1002     133             0 Sep 15 14:57 testupload.txt
226 Directory send OK.

```

# Footprinting the Service

### Nmap FTP Scripts

```
sudo nmap --script-updatedb

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:49 CEST
NSE: Updating rule database.
NSE: Script Database updated successfully.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.28 seconds
```

All the NSE scripts are located on the Pwnbox in /usr/share/nmap/scripts/, but on our systems, we can find them using a simple command.

```
find / -type f -name ftp* 2>/dev/null | grep scripts

/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-brute.nse
```

As we already know, the FTP server usually runs on the standard TCP port 21, which we can scan using Nmap. We also use the version scan (-sV), aggressive scan (-A), and the default script scan (-sC) against our target 10.129.14.136.

### nmap scan
```
sudo nmap -sV -p21 -sC -A 10.129.14.136

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-16 18:12 CEST
Nmap scan report for 10.129.14.136
Host is up (0.00013s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rwxrwxrwx    1 ftp      ftp       8138592 Sep 16 17:24 Calendar.pptx [NSE: writeable]
| drwxrwxrwx    4 ftp      ftp          4096 Sep 16 17:57 Clients [NSE: writeable]
| drwxrwxrwx    2 ftp      ftp          4096 Sep 16 18:05 Documents [NSE: writeable]
| drwxrwxrwx    2 ftp      ftp          4096 Sep 16 17:24 Employees [NSE: writeable]
| -rwxrwxrwx    1 ftp      ftp            41 Sep 16 17:24 Important Notes.txt [NSE: writeable]
|_-rwxrwxrwx    1 ftp      ftp             0 Sep 15 14:57 testupload.txt [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.4
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
```

The default script scan is based on the services' fingerprints, responses, and standard ports. Once Nmap has detected the service, it executes the marked scripts one after the other, providing different information. For example, the ftp-anon NSE script checks whether the FTP server allows anonymous access. If so, the contents of the FTP root directory are rendered for the anonymous user.

The ftp-syst, for example, executes the STAT command, which displays information about the FTP server status. This includes configurations as well as the version of the FTP server. Nmap also provides the ability to trace the progress of NSE scripts at the network level if we use the --script-trace option in our scans. This lets us see what commands Nmap sends, what ports are used, and what responses we receive from the scanned server.

### Nmap Script Trace

```
sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:54 CEST                                                                                                                                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.14.136:21]                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 16 [10.129.14.136:21]             
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 24 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 32 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #1 [10.129.14.136:21] (timeout: 7000ms) EID 42
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #2 [10.129.14.136:21] (timeout: 9000ms) EID 50
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #3 [10.129.14.136:21] (timeout: 7000ms) EID 58
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #4 [10.129.14.136:21] (timeout: 11000ms) EID 66
NSE: TCP 10.10.14.4:54226 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54228 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54230 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54232 > 10.129.14.136:21 | CONNECT
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 50 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 58 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSE: TCP 10.10.14.4:54228 < 10.129.14.136:21 | 220 Welcome to HTB-Academy FTP service.
```

The scan history shows that four different parallel scans are running against the service, with various timeouts. For the NSE scripts, we see that our local machine uses other output ports (54226, 54228, 54230, 54232) and first initiates the connection with the CONNECT command. From the first response from the server, we can see that we are receiving the banner from the server to our second NSE script (54228) from the target FTP server. If necessary, we can, of course, use other applications such as netcat or telnet to interact with the FTP server.

### Service Interaction

```
nc -nv 10.129.14.136 21
```

```
telnet 10.129.14.136 21
```

It looks slightly different if the FTP server runs with TLS/SSL encryption. Because then we need a client that can handle TLS/SSL. For this, we can use the client openssl and communicate with the FTP server. The good thing about using openssl is that we can see the SSL certificate, which can also be helpful.

```
openssl s_client -connect 10.129.14.136:21 -starttls ftp

CONNECTED(00000003)                                                                                      
Can't use SSL_get_servername                        
depth=0 C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
verify error:num=18:self signed certificate
verify return:1

depth=0 C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
verify return:1
---                                                 
Certificate chain
 0 s:C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
 
 i:C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
---
 
Server certificate

-----BEGIN CERTIFICATE-----

MIIENTCCAx2gAwIBAgIUD+SlFZAWzX5yLs2q3ZcfdsRQqMYwDQYJKoZIhvcNAQEL
...SNIP...

```

This is because the SSL certificate allows us to recognize the hostname, for example, and in most cases also an email address for the organization or company. In addition, if the company has several locations worldwide, certificates can also be created for specific locations, which can also be identified using the SSL certificate.

# Questions

<details>
<summary>Click to show solution</summary>

#### Question 1
![](images/Pic.png) <br>
`InFreight FTP v1.1`

#### Question 2
![](images/pic2.png) <br>
`HTB{b7skjr4c76zhsds7fzhd4k3ujg7nhdjre}`

</details>

<br>

# SMB


## What is SMB ?

Server Message Block (SMB) is a client–server communication protocol used to manage access to shared resources on a network. These resources can include files, directories, printers, and other network services. SMB also enables communication between different system processes.

- SMB is also supported on Linux and Unix systems through the open-source project Samba, enabling cross-platform file and resource sharing.

> Originally introduced with early Microsoft products such as LAN Manager and LAN Server for OS/2, SMB has since become a core component of the Windows operating system family. One of its key strengths is backward compatibility—newer Windows systems can seamlessly communicate with older versions.


## How SMB Works

SMB allows a client to connect to another device on the same network and access shared resources. For this to happen:

1. The target system must run an SMB server service.
2. The client sends a request to access a shared resource.
3. The server processes the request and responds accordingly.

Before any data transfer occurs, the client and server must establish a connection by exchanging a series of messages.

## SMB and TCP/IP

In modern IP networks, SMB operates over the TCP protocol. The connection process includes:

1. A three-way handshake to establish a reliable connection between client and server.
2. Controlled data transmission managed by TCP to ensure integrity and order.

## Shares and Access Control

An SMB server can expose portions of its local file system as shared resources (shares). The structure presented to the client may differ from the server’s actual file system layout.

Access to these shares is controlled using Access Control Lists (ACLs), which define permissions such as:

- Read
- Execute
- Full access

Permissions can be assigned to individual users or groups, allowing fine-grained control over resource access. These ACLs are applied at the share level and may differ from the permissions configured locally on the server.

## Samba

### Overview

As mentioned earlier, Samba is an open-source implementation of the SMB protocol designed for Unix-based operating systems. It enables Linux and Unix systems to communicate and share resources with Windows systems.<br>
Samba implements the Common Internet File System (CIFS), which is a dialect (variant) of SMB originally developed by Microsoft. Because of this, Samba is often referred to as SMB/CIFS, and it can communicate effectively with Windows environments.

### SMB, CIFS, and Versions

CIFS corresponds largely to SMB version 1 (SMBv1), which is now considered outdated. Modern systems use newer SMB versions that provide better performance and security.


### Key differences:
- CIFS / SMBv1:
  - Older and less secure
  - Often uses NetBIOS over TCP/IP (ports 137, 138, 139)
- Modern SMB (SMBv2 and SMBv3):
  - Improved performance and security
  - Uses direct TCP communication over port 445


## SMB Versions Overview
|SMB      | Version	                   | Supported Systems	Key Features                     |
|---------|----------------------------|-----------------------------------------------------|
|CIFS	    | Windows NT 4.0             |	Communication via NetBIOS                          |
|SMB 1.0	| Windows 2000               |	Direct TCP connection                              |
|SMB 2.0	| Windows Vista, Server 2008 |	Performance improvements, message signing, caching |
|SMB 2.1	| Windows 7, Server 2008 R2  | Improved locking mechanisms                         |
|SMB 3.0	|  Windows 8, Server 2012	   | Multichannel, encryption, remote storage            |
|SMB 3.0.2|	Windows 8.1, Server 2012   | R2	Minor improvements                               |
|SMB 3.1.1|	Windows 10, Server 2016	   | Integrity checking, AES-128 encryption              |

### Samba Capabilities

Samba has evolved significantly over time:
- Samba 3:
  - Can function as a full member of an Active Directory domain
- Samba 4:
  - Can act as an Active Directory Domain Controller

Samba operates using background services (daemons):
  - smbd:
    - Handles file sharing and authentication
  - nmbd:
    - Manages NetBIOS-related services such as name resolution

> These daemons are controlled by the SMB service.

## Workgroups and NetBIOS

In SMB networks, systems are typically organized into workgroups, which are logical groupings of computers that share resources.

The underlying communication can rely on NetBIOS, originally developed by IBM as an API for network communication.

Key concepts:
- Each device on the network requires a unique name
- This name is registered using:
- Local name registration, or
- A NetBIOS Name Server (NBNS)
- NBNS was later extended into Windows Internet Name Service (WINS)

## Default Configuration

As we can imagine, Samba offers a wide range of settings that we can configure. Again, we define the settings via a text file where we can get an overview of some of the settings. These settings look like the following when filtered out:

```
cat /etc/samba/smb.conf | grep -v "#\|\;" 

[global]
   workgroup = DEV.INFREIGHT.HTB
   server string = DEVSMB
   log file = /var/log/samba/log.%m
   max log size = 1000
   logging = file
   panic action = /usr/share/samba/panic-action %d

   server role = standalone server
   obey pam restrictions = yes
   unix password sync = yes

   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .

   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes

[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700

[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
```

We see global settings and two shares that are intended for printers. The global settings are the configuration of the available SMB server that is used for all shares. In the individual shares, however, the global settings can be overwritten, which can be configured with high probability even incorrectly. Let us look at some of the settings to understand how the shares are configured in Samba.

| Setting                          | Description                                                                 |
|----------------------------------|-----------------------------------------------------------------------------|
| [sharename]                      | The name of the network share.                                              |
| workgroup = WORKGROUP/DOMAIN     | Workgroup that will appear when clients query.                              |
| path = /path/here/               | The directory to which user is to be given access.                          |
| server string = STRING           | The string that will show up when a connection is initiated.                |
| unix password sync = yes         | Synchronize the UNIX password with the SMB password?                        |
| usershare allow guests = yes     | Allow non-authenticated users to access defined share?                      |
| map to guest = bad user          | What to do when a user login request doesn't match a valid UNIX user?       |
| browseable = yes                 | Should this share be shown in the list of available shares?                 |
| guest ok = yes                   | Allow connecting to the service without using a password?                   |
| read only = yes                  | Allow users to read files only?                                             |
| create mask = 0700               | What permissions need to be set for newly created files?                    |

## Dangerous Settings
| Setting                     | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| browseable = yes            | Allow listing available shares in the current share?                        |
| read only = no              | Allow creation and modification of files (not read-only).                   |
| writable = yes              | Allow users to create and modify files?                                     |
| guest ok = yes              | Allow connecting to the service without using a password?                   |
| enable privileges = yes     | Honor privileges assigned to specific SID?                                  |
| create mask = 0777          | Permissions assigned to newly created files (very permissive).              |
| directory mask = 0777       | Permissions assigned to newly created directories (very permissive).        |
| logon script = script.sh    | Script executed when a user logs in.                                        |
| magic script = script.sh    | Script executed when the file/script is closed.                             |
| magic output = script.out   | Location where output of the magic script is stored.                        |


## Example Insecure Share Configuration

To understand how risky settings affect enumeration, we can create a share like [notes] and apply permissive options.

```
[notes]
    comment = CheckIT
    path = /mnt/notes/

    browseable = yes
    read only = no
    writable = yes
    guest ok = yes

    enable privileges = yes
    create mask = 0777
    directory mask = 0777
```
This configuration allows:
- An attacker can easily discover, access, modify, and download files during enumeration.
1. Anyone (even unauthenticated users) to access the share
2. Full read/write permissions
3. Listing and browsing of files
4. Very weak file permissions (0777)

### Restart Samba
```
sudo systemctl restart smbd
```

### SMBclient - Connecting to the Share
Now we can display a list `(-L)` of the server's shares with the smbclient command from our host. <br>
We use the so called null session `(-N)`, which is anonymous access without the input of existing users or valid passwords.
```
smbclient -N -L //10.129.14.128

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        home            Disk      INFREIGHT Samba
        dev             Disk      DEVenv
        notes           Disk      CheckIT
        IPC$            IPC       IPC Service (DEVSM)
SMB1 disabled -- no workgroup available
```

We can see that we now have five different shares on the Samba server from the result.<br>
These are already included by default in the basic setting, as we have already seen.<br>
- print$ 
- IPC$<br>
Since we deal with the [notes] share, let us log in and inspect it using the same client program. If we are not familiar with the client program, we can use the help command on successful login, listing all the possible commands we can execute.

```
smbclient //10.129.14.128/notes

Enter WORKGROUP\<username>'s password: 
Anonymous login successful
Try "help" to get a list of possible commands.


smb: \> help

?              allinfo        altname        archive        backup         
blocksize      cancel         case_sensitive cd             chmod          
chown          close          del            deltree        dir            
du             echo           exit           get            getfacl        
geteas         hardlink       help           history        iosize         
lcd            link           lock           lowercase      ls             
l              mask           md             mget           mkdir          
more           mput           newer          notify         open           
posix          posix_encrypt  posix_open     posix_mkdir    posix_rmdir    
posix_unlink   posix_whoami   print          prompt         put            
pwd            q              queue          quit           readlink       
rd             recurse        reget          rename         reput          
rm             rmdir          showacls       setea          setmode        
scopy          stat           symlink        tar            tarmode        
timeout        translate      unlock         volume         vuid           
wdel           logon          listconnect    showconnect    tcon           
tdis           tid            utimes         logoff         ..             
!            


smb: \> ls

  .                                   D        0  Wed Sep 22 18:17:51 2021
  ..                                  D        0  Wed Sep 22 12:03:59 2021
  prep-prod.txt                       N       71  Sun Sep 19 15:45:21 2021

                30313412 blocks of size 1024. 16480084 blocks available
```

Once we have discovered interesting files or folders, we can download them using the `get` command. <br>Smbclient also allows us to execute local system commands using an exclamation mark at the beginning `(!<cmd>)` without interrupting the connection.

### Download Files from SMB

```
smb: \> get prep-prod.txt 

getting file \prep-prod.txt of size 71 as prep-prod.txt (8,7 KiloBytes/sec) 
(average 8,7 KiloBytes/sec)


smb: \> !ls

prep-prod.txt


smb: \> !cat prep-prod.txt

[] check your code with the templates
[] run code-assessment.py
[] …
```


### Monitoring SMB Connections with `smbstatus`

Administrators can use the `smbstatus` command to **monitor active SMB connections**. It provides key information, including:

- **Connected users**  
- **Client IP addresses / hosts**  
- **Accessed shares**  
- **Protocol version, encryption, and signing details**  

### Domain-Level Authentication

When Samba is part of a **Windows domain**:

- A **domain controller** authenticates users and manages passwords.  
- Credentials are stored in:
  - `NTDS.dit` (user and password database)  
  - `SAM` (Security Authentication Module)  
- Users authenticate once, then can access shared resources across the domain.

### Example `smbstatus` Output

```
root@samba:~# smbstatus

Samba version 4.11.6-Ubuntu
PID     Username     Group        Machine                                   Protocol Version  Encryption           Signing
-----------------------------------------------------------------------------------------------------------------
75691   sambauser    samba        10.10.14.4 (ipv4:10.10.14.4:45564)      SMB3_11           -                    -

Service      pid     Machine       Connected at                     Encryption   Signing
---------------------------------------------------------------------------------------------
notes        75691   10.10.14.4   Do Sep 23 00:12:06 2021 CEST     -            -

No locked files
```

## Footprinting the Service

#### Nmap
```
sudo nmap 10.129.14.128 -sV -sC -p139,445

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 15:15 CEST
Nmap scan report for sharing.inlanefreight.htb (10.129.14.128)
Host is up (0.00024s latency).

PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2
MAC Address: 00:00:00:00:00:00 (VMware)

Host script results:
|_nbstat: NetBIOS name: HTB, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-09-19T13:16:04
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.35 seconds
```

The Remote Procedure Call (RPC) is a concept and, therefore, also a central tool to realize operational and work-sharing structures in networks and client-server architectures. The communication process via RPC includes passing parameters and the return of a function value.

We can see from the results that it is not very much that Nmap provided us with here. Therefore, we should resort to other tools that allow us to interact manually with the SMB and send specific requests for the information. One of the handy tools for this is rpcclient. This is a tool to perform MS-RPC functions.

### RPCclient
```
rpcclient -U "" 10.129.14.128

Enter WORKGROUP\'s password:
rpcclient $>
```
<br>
The rpcclient offers us many different requests with which we can execute specific functions on the SMB server to get information. A complete list of all these functions can be found on the man page of the rpcclient.

<br>

| Query | Description |
|------|------------|
| srvinfo | Server information |
| enumdomains | Enumerate all domains in the network |
| querydominfo | Provides domain, server, and user information |
| netshareenumall | Enumerates all available shares |
| netsharegetinfo <share> | Provides information about a specific share |
| enumdomusers | Enumerates all domain users |
| queryuser <RID> | Provides information about a specific user |

### RPCClient - Enumeration

```
rpcclient $> srvinfo

DEVSMB Wk Sv PrQ Unx NT SNT DEVSM
platform_id : 500
os version : 6.1
server type : 0x809a03

rpcclient $> enumdomains

name:[DEVSMB] idx:[0x0]
name:[Builtin] idx:[0x1]

rpcclient $> querydominfo

Domain: DEVOPS
Server: DEVSMB
Comment: DEVSM
Total Users: 2
Total Groups: 0
Total Aliases: 0
Sequence No: 1632361158
Force Logoff: -1
Domain Server State: 0x1
Server Role: ROLE_DOMAIN_PDC
Unknown 3: 0x1

rpcclient $> netshareenumall

netname: print$
remark: Printer Drivers
path: C:\var\lib\samba\printers

netname: home
remark: INFREIGHT Samba
path: C:\home\

netname: dev
remark: DEVenv
path: C:\home\sambauser\dev\

netname: notes
remark: CheckIT
path: C:\mnt\notes\

netname: IPC$
remark: IPC Service (DEVSM)
path: C:\tmp

rpcclient $> netsharegetinfo notes

netname: notes
remark: CheckIT
path: C:\mnt\notes
type: 0x0
perms: 0
max_uses: -1
num_uses: 1
```

### Information Exposure

These examples demonstrate how much information can be exposed to anonymous users. Once anonymous access is allowed, even a small misconfiguration can lead to excessive permissions or visibility, putting the entire network at risk.

Additionally, attackers can discover valid usernames and attempt brute-force attacks. Weak passwords, often caused by poor security practices, make such attacks more effective.

### RPCClient - User Enumeration

```
rpcclient $> enumdomusers

user:[mrb3n] rid:[0x3e8]
user:[cry0l1t3] rid:[0x3e9]

rpcclient $> queryuser 0x3e9

User Name : cry0l1t3
Full Name : cry0l1t3
Home Drive : \devsmb\cry0l1t3
Profile Path: \devsmb\cry0l1t3\profile
user_rid : 0x3e9
group_rid: 0x201

rpcclient $> queryuser 0x3e8

User Name : mrb3n
Home Drive : \devsmb\mrb3n
Profile Path: \devsmb\mrb3n\profile
user_rid : 0x3e8
group_rid: 0x201
```

### RPCClient - Group Information

```
rpcclient $> querygroup 0x201

Group Name: None
Description: Ordinary Users
Group Attribute:7
Num Members:2
```

### Brute Forcing User RIDs

Sometimes not all commands are available due to permission restrictions. However, `queryuser <RID>` is often allowed. This makes it possible to brute-force RIDs to discover valid users.

```
for i in $(seq 500 1100); do
rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)"
| grep "User Name|user_rid|group_rid" && echo ""
done

User Name : sambauser
user_rid : 0x1f5
group_rid: 0x201

User Name : mrb3n
user_rid : 0x3e8
group_rid: 0x201

User Name : cry0l1t3
user_rid : 0x3e9
group_rid: 0x201
```

### Impacket - samrdump.py

```
samrdump.py 10.129.14.128
```

This tool retrieves user and domain information similarly to `rpcclient`.

### SMBMap

```
smbmap -H 10.129.14.128
```

### CrackMapExec

```
crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```

### Enum4Linux-ng Installation

```
git clone https://github.com/cddmp/enum4linux-ng.git

cd enum4linux-ng
pip3 install -r requirements.txt
```

### Enum4Linux-ng Enumeration

```
./enum4linux-ng.py 10.129.14.128 -A
```

# Questions

<details>
<summary>Click to show solution</summary>

#### Question 1
- It didn't show the whole version idk why.
![](images/pic8.png) <br>
`Samba smbd 4.6.2`

#### Question 2
![](images/pic4.png) <br>
`sambashare`

#### Question 3
![](images/pic3.png) <br>
`HTB{o873nz4xdo873n4zo873zn4fksuhldsf}`

#### Question 4
![](images/pic5.png) <br>
`DEVOPS`

#### Question 5
![](images/pic6.png) <br>
`InFreight SMB v3.1`

#### Question 6
![](images/pic7.png) <br>
`/home/sambauser`

</details>