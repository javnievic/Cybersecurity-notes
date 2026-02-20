How many TCP ports are open?
![](Screenshots/Pasted%20image%2020250113194956.png)
There are 3 open ports

![](Screenshots/Pasted%20image%2020250113195509.png)

Are you able to get to other users' scans?
![](Screenshots/Pasted%20image%2020250113200658.png)
yes

What is the ID of the PCAP file that contains sensative data?
0

Which application layer protocol in the pcap file can the sensetive data be found in?
ftp

![](Screenshots/Pasted%20image%2020250113203348.png)

Submit the flag located in the nathan user's home directory.
![](Screenshots/Pasted%20image%2020250114001512.png)

## Privilege escalation
Privilege escalation through binaries:
`bash linpeas.sh`

![](Screenshots/Pasted%20image%2020250114011735.png)
![[Screenshots/Pasted image 20250114011606.png]]

We escalate privileges taking advantage of python3.8 capabilities that has the setiud
![[Screenshots/Pasted image 20250114012408.png]]
![](Screenshots/Pasted%20image%2020250114013155.png)