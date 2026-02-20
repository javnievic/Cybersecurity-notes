If we observe the code we can see this function: 

```javascript
const IsPalinDrome = (string) => {

if (string.length < 1000) {
	return 'Tootus Shortus';
}

for (const i of Array(string.length).keys()) {

	const original = string[i];
	const reverse = string[string.length - i - 1];
	
	if (original !== reverse || typeof original !== 'string') {
		return 'Notter Palindromer!!';
	
	}

}

return null;

}
```

So if we try a long palindrome, to outsmart the first validation, a 413 nginx error appeared:
![](Screenshots/Pasted%20image%2020251116003447.png)

So we can try to avoid it, removing the "" of the string, and try it with numbers: 
![](Screenshots/Pasted%20image%2020251116004039.png)
This is not valid, as it has to be with strings: 
```javascript
if (original !== reverse || typeof original !== 'string') {
	...
```


I tried `"palindrome":"a".repeat(1000);` but this does not worked with a 500 error.
Another payloads proved: 
["a"]
{"length":1000}

The dict guess is not a bad one, so I continue with this, we first bypassed the lenght validation, now we have to bypass the palindrome one, so we have to simulate this: 
```javascript
for (const i of Array(string.length).keys()) {
	const original = string[i];
	const reverse = string[string.length - i - 1];

if (original !== reverse || typeof original !== 'string') {
```

So if we simulate with the dict retrieval we will bypass it.
First of all `Array(string.length).keys()` only returns [ 0 ] so it will iterate only with i = 0. Knowing this, we have to achieve an "**original**" and "**reverse**"  that have to be equals. So we got it, we have only to add 2 more entries to the dict with "0" and "999" as the keys and their values have to be strings and equal, for example: 
`{"length": "1000", "0": "a", "999": "a" }`

![](Screenshots/Pasted%20image%2020251116175121.png)
