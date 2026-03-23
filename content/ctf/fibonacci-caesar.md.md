---
title: "Fibonacci Caesar"
ctf: "DDC Qualifiers"
challenge_name: "Fibonacci Caesar"
category: "Crypto"
date: 2026-03-23
tags: [ctf, writeup, Crypto]
status: draft
tools: []
techniques: []
vuln: ""
slug: "fibonacci-caesar"
---

# Fibonacci Caesar

## Challenge beskrivelse

En af de ældste former for kryptering er Cæsar-chifferen, hvor hvert bogstav forskydes af en hemmelig nøgle.

Denne gang har nogen ændret Cæsar-chifferen ved hjælp af Fibonacci-sekvensen.

Kan du dekryptere flaget?


## Challenge handout
#### Script
```python
import string
import random
import math

alphabet = 'abcdefghijklmnopqrstuvwxyz'

phi = (1 + math.sqrt(5)) / 2
psi = (1 - math.sqrt(5)) / 2
sqrt5 = math.sqrt(5)

  
def fib_closed(n):
    """
    Compute F_n using the closed-form expression.
    Valid for moderate n (educational use).
    """

    return int(round((pow(phi, n, len(alphabet)) - pow(psi, n, len(alphabet))) / sqrt5))
def fib(n, mod):
    def _fib(k):
        if k == 0:
            return (0, 1)
        a, b = _fib(k >> 1)
        c = (a * ((b << 1) - a)) % mod
        d = (a * a + b * b) % mod
        if k & 1:
            return (d, (c + d) % mod)
        else:
            return (c, d)
    return _fib(n)[0]
 
def fib_caesar_encrypt(n, text):
    a = fib(n, len(alphabet))
    b = fib(n + 1, len(alphabet))
    out = []
    for c in text:
        if c in string.whitespace:
            out.append(c)
            continue
        k = a % len(alphabet)
        a, b = b, a + b
        out.append(alphabet[(alphabet.index(c) + k) % len(alphabet)])
    return "".join(out)

key = random.randint(0, 2**128)

def main():
    # Danish text, flag is in text
    with open('flag.txt', 'rb') as f:
        text = f.read().decode("utf-8").strip()
    ciphertext = fib_caesar_encrypt(key, text)
    print(ciphertext)
    with open('encryption.txt', 'wb') as f:
        f.write(ciphertext.encode("utf-8"))  


if __name__ == '__main__':

    main()
```

## Løsning af challenge
Umiddelbart så ser nøglen ikke ud til at kunne brute-forces, da nøglen er $0 < \text{key} < 2^{128}$. Dog skal det bemærkes at der opereres over alfabetet, og derfor må det endte tal der krypteres være $\text{mod } 26$. Det vil sige at der pludseligt kun er 26 muligheder i stedet for næsten uendeligt mange. 

Ligesom i Binary Caesar, så kendes den første del af flaget nemlig "ddc":
```python
import string
import random
alphabet = 'abcdefghijklmnopqrstuvwxyz'
def fib(n, mod):
    def _fib(k):
        if k == 0:
            return (0, 1)
        a, b = _fib(k >> 1)
        c = (a * ((b << 1) - a)) % mod
        d = (a * a + b * b) % mod
        if k & 1:
            return (d, (c + d) % mod)
        else:
            return (c, d)
    return _fib(n)[0]

def fib_caesar_encrypt(n, text):
    a = fib(n, len(alphabet))
    b = fib(n + 1, len(alphabet))
    out = []
    for c in text:
        if c in string.whitespace:
            out.append(c)
            continue
        k = a % len(alphabet)
        a, b = b, a + b
        out.append(alphabet[(alphabet.index(c) + k) % len(alphabet)])
    return "".join(out)

  

key = random.randint(0, 2**128)

  

krypteret_flag = "yvp uappzp fsrjusxe njbgfe shnzkihpa hxgrbr"

#Key kan findes da vi er modulo 26, og derfor kan nøglen ikke være større, derfor kan den nemt bruteforces
for k in range(0,26):
    decrypted = fib_caesar_encrypt(k, krypteret_flag)
    if "ddc" in decrypted:
        flag = decrypted
        print(flag)
```

Dette gav flaget `ddc{pisano_sequence_solves_fibonacci_caesar}`
