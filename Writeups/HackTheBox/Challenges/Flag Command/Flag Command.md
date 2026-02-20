
You have to go to the url http:/83.136.254.36:33529

![](Screenshots/Pasted%20image%2020250114171828.png)
Then inspect all the page with ctrl+shift+i, and you will realise that there are other referenced js: 
![](Screenshots/Pasted%20image%2020250114171338.png)
view-source:http://83.136.254.36:33529/,  view-source:http://83.136.254.36:33529/static/terminal/js/main.js, view-source:http://83.136.254.36:33529/static/terminal/js/game.js

In main.js there is a method fetchOptions(): const fetchOptions = () => {
    fetch('/api/options')
        .then((data) => data.json())
        .then((res) => {
            availableOptions = res.allPossibleCommands;

        })
        .catch(() => {
            availableOptions = undefined;
        })
}

![](Screenshots/Pasted%20image%2020250114171525.png)
![](Screenshots/Pasted%20image%2020250114171734.png)
We have to write the combination: "HEAD NORTH", "FOLLOW A MYSTERIOUS PATH", "SET UP CAMP" and "Blip-blop, in a pickle with a hiccup! Shmiggity-shmack"

And we have the flag captured: ![[Screenshots/Pasted image 20250114171828.png]]
HTB{D3v3l0p3r_t00l5_4r3_b35t__t0015_wh4t_d0_y0u_Th1nk??}