---
title: "Alog"
ctf: "DDC Qualifiers"
challenge_name: "Alog"
category: "Forensics"
date: 2026-03-23
tags: [ctf, writeup, Forensics]
status: draft
tools: []
techniques: []
vuln: ""
slug: "alog"
---

# Alog

## Challenge beskrivelse
Vi har en mistanke om, at nogen har lavet nogle mærkelige forespørgsler til vores server.  

Kan du finde flaget?


## Løsning af challenge
Okay først tjekker jeg den givet fil som viser sig og være en log fil jeg spotter hurtigt noget bekendt tekst altså DDC{ jeg tager et nærmere kig og ser det er en del af et flag "DDC{Tim3_Tr4v3l" så kigger jeg lidt videre i denne log fil og få linjer længere nede ser jeg endnu en del "L0g_An4lys1s}" bum nu har jeg begge dele så jeg sætter dem sammen og fås flaget:
DDC{Tim3_Tr4v3l_L0g_An4lys1s}
