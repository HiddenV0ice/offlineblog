---
title: "Binary Caesar"
ctf: "DDC Qualifiers"
challenge_name: "Binary Caesar"
category: "Crypto"
date: 2026-03-23
tags: [ctf, writeup, Crypto]
status: draft
tools: []
techniques: []
vuln: ""
math: true
slug: "binary-caesar"
---

# Binary Caesar

## Challenge beskrivelse

En af de ældste former for kryptering er Cæsar-cifret, hvor hvert bogstav forskydes med en hemmelig nøgle.  
Denne gang har nogen modificeret Cæsar-cifret ved at bruge XOR i stedet for addition.

Kan du dekryptere flaget?

Husk at lægge resultatet ind i flagformatet. (ddc{eksempel_flag} -> DDC{eksempel_flag})


## Challenge handout
### Script

```python 
import string
import random
alphabet = 'abcdefghijklmnopqrstuvwxyzæøå{}_'
# Rotate each character by the index of the key character
def xor(a, b):
    # Ignore spaces
    if a in string.whitespace:
        return a
    return alphabet[(alphabet.index(a) ^ alphabet.index(b))]
    
def caesar_encrypt(key, text):

    ciphertext = ""

    for i in range(len(text)):

        ciphertext += xor(text[i], key)

    return ciphertext
key = 'a'
while (key == 'a'):
    key = random.choice(alphabet)
def main():
	 # Danish text, flag is in text
    with open('flag.txt', 'rb') as f:
        text = f.read().decode("utf-8").strip()
    ciphertext = caesar_encrypt(key, text)
    with open('encryption.txt', 'wb') as f:
        f.write(ciphertext.encode("utf-8"))
if __name__ == '__main__':
    main()
```

## Løsning af challenge

Det binære talsystem kan beskrives som et endeligt felt(Finite Field red.) Dette kaldes Galois Field og er beskrevet som et lukket felt af et primtal $p$ og betegnes som $GF(p)$. Da 2 er et primtal, så betydes dette altså at der opereres over $GF(2)$. Dette giver egenskaber som at addition er logisk ækvivalent med XOR operatoren i $GF(2)$. Dette er grundet hver addition vil være modulus 2. 

Scriptet finder kun en tilfældig key én gang. Det vil sige at der blot kan bruteforces, da det vides at flaget starter med "d". Dernæst ved vi også addition svarer til XOR'ing i $GF(2)$ og derfor kan vi blot XOR "p" med 

Dette kan gøres i Python således: 
```python 
import string
alphabet = 'abcdefghijklmnopqrstuvwxyzæøå{}_'
key0 = "p"

def xor(a, b):
    # Ignore spaces
    if a in string.whitespace:
        return a
    return alphabet[(alphabet.index(a) ^ alphabet.index(b))]
 
# Key'en er statisk og kun et bogstav, derfor kan vi blot finde denne således:
for bogstav in alphabet:
    if xor(key0,bogstav) == "d":
		key_found = bogstav,alphabet.find(bogstav)
	    print(key_found) #giver m, 12
```

Dette giver nøglen "m". Dertil kan der blot XOR'es med "m" for hver karakter i det krypteret flag således: 

```python
import string

alphabet = 'abcdefghijklmnopqrstuvwxyzæøå{}_'
key0 = "p"

# XOR er generelt cyklisk. Det vil sige at: E ^ K = CE, Hvor E er vores ikke-krypteret element, K er key'en, og CE er det krypteret element.
# Vi kan løse vores "ligning, ved at bytte rundt på CE og E således: CE ^ K = E, så vi skal blot finde K og XOR det krypteret element med K og så får vi flaget.  
def xor(a, b):
    # Ignore spaces
    if a in string.whitespace:
        return a
    return alphabet[(alphabet.index(a) ^ alphabet.index(b))]

# Key'en er statisk og kun et bogstav, derfor kan vi blot finde denne således:
for bogstav in alphabet:
    if xor(key0,bogstav) == "d":
        key_found = bogstav
        print(key_found) #giver m, 12

flag = ""
enc_flag = "pporkmhce}taii_}tomi}m{s"
for enc in enc_flag:
    flag += xor(enc,key_found)

print(flag)
```

Dette gav flaget `ddc{galois_meets_caesar}`