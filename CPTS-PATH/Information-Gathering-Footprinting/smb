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
