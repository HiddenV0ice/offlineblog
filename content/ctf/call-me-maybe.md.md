---
title: "Call Me Maybe"
ctf: "DDC Qualifiers"
challenge_name: "Call Me Maybe"
category: "Rev"
date: 2026-03-23
tags: [ctf, writeup, Rev]
status: draft
tools: []
techniques: []
vuln: ""
slug: "call-me-maybe"
---

# Call Me Maybe

## Challenge beskrivelse

Du er lige startet i praktik hos **NoTech**, en banebrydende startup inden for cybersikkerhed.  
Allerede på din allerførste dag kommer din manager forbi dit skrivebord og skubber et USB-drev hen over bordet.

> "Analytikeren før dig efterlod den her.  
> Det er en form for låst terminal; ingen her kender adgangskoden.  
> Vi har prøvet alt. Tror du, du kan knække den, rookie?"*

Hun blinker og går videre. Du sætter USB’en i og finder én enkelt fil: `call_me_maybe`.  
Du kører den. Den beder om en adgangskoden. Du har den ikke.  
Men der er noget ved navnet, der nager dig… _Call Me Maybe_… calls… maybe?

Hvad hvis hemmeligheden ikke ligger _inde i_ programmet, men i det, programmet kalder?


## Løsning af challenge
Der blev hintet meget til at skulle bruge `ltrace` derfor blev dette prøvet:
```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ ltrace ./call_me_maybe 
setvbuf(0x7f1b9d8165c0, nil, 2, 0)                                                                                                                = 0
putchar(10, 0, 125, 0
)                                                                                                                            = 10
puts("  \342\225\224\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220"...  ╔══════════════════════════════════════════╗
)             = 135
puts("  \342\225\221      NoTech Security Termi"...  ║      NoTech Security Terminal v2.4       ║
)                                                                                              = 51
puts("  \342\225\221        Authentication Requ"...  ║        Authentication Required           ║
)                                                                                              = 51
puts("  \342\225\232\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220\342\225\220"...  ╚══════════════════════════════════════════╝
)             = 135
putchar(10, 0x7f1b9d816643, 0x7f1b9d817790, 0x7f1b9d817790
)                                                                                       = 10
puts("  STATUS: 1 classified message w"...  STATUS: 1 classified message waiting
)                                                                                                       = 39
puts("  CLEARANCE: Agent-level passphr"...  CLEARANCE: Agent-level passphrase required

)                                                                                                       = 46
printf("  Enter passphrase: "  Enter passphrase: )                                                                                                                    = 20
fgets(kenneth
"kenneth\n", 256, 0x7f1b9d8158e0)                                                                                                           = 0x7ffcbe03a4f0
strcspn("kenneth\n", "\n")                                                                                                                        = 7
strcmp("kenneth", "DDC{ltr4c3_my_l1br4ry_c4lls}")                                                                                                 = 39
puts("\n  [ACCESS DENIED]"
  [ACCESS DENIED]
)                                                                                                                       = 19
puts("  Invalid passphrase. Terminal l"...  Invalid passphrase. Terminal locked.

)                                                                                                       = 40
puts("  Hint: Maybe you should trace t"...  Hint: Maybe you should trace the calls...
)                                                                                                       = 45
+++ exited (status 0) +++
```
Dette gav flaget:
DDC{ltr4c3_my_l1br4ry_c4lls}