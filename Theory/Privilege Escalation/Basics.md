# Privilege Escalation Basics Cheat Sheet

## 1. System Information

uname -a  
hostnamectl  
cat /etc/os-release

## 2. User & Groups

whoami  
id  
groups  
cat /etc/passwd  
cat /etc/group  
sudo -l

## 3. Home & Interesting Files

ls -la /home  
ls -la ~/.ssh  
cat ~/.bash_history  
find / -type f -name "*.txt" 2>/dev/null

## 4. Sudo Misconfigurations

sudo -l  
sudo -u root <command>  
GTFOBins: [https://gtfobins.github.io](https://gtfobins.github.io)

## 5. SUID / SGID Binaries

find / -perm -4000 2>/dev/null  
find / -6000 2>/dev/null  
strings <binary>

## 6. Capabilities

getcap -r / 2>/dev/null

## 7. Installed Packages

dpkg -l  
rpm -qa  
snap list  
flatpak list

## 8. Running Processes

ps aux  
ps -ef  
top  
systemctl --type=service --state=running

## 9. Cron Jobs

crontab -l  
ls -la /etc/cron.*  
cat /etc/crontab

## 10. Writable / World-Writable Paths

find / -writable -type d 2>/dev/null  
find / -perm -222 -type d 2>/dev/null

## 11. PATH Abuse

echo $PATH  
find / -writable -type d 2>/dev/null

## 12. Passwords & Credentials

grep -Ri "password" /etc 2>/dev/null  
grep -Ri "passwd" /var/www 2>/dev/null  
cat /var/www/html/*.php

## 13. SSH Keys

ls -la ~/.ssh  
cat ~/.ssh/id_rsa  
cat ~/.ssh/authorized_keys

## 14. Kernel Exploits

uname -r  
searchsploit <kernel-version>

## 15. Networking

ip a  
ip r  
ss -tunlp  
netstat -ano

## 16. Docker / LXC Escapes

docker ps  
docker images  
groups | grep docker

## Auto-enumeration Tools

linpeas.sh  
linEnum.sh  
linux-smart-enumeration