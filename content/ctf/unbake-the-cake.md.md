---
title: "Unbake The Cake"
ctf: "DDC Qualifiers"
challenge_name: "Unbake The Cake"
category: "Rev"
date: 2026-03-23
tags: [ctf, writeup, Rev]
status: draft
tools: []
techniques: []
vuln: ""
slug: "unbake-the-cake"
---

# Unbake The Cake

## Challenge beskrivelse

Jeg fandt denne lækre ASCII-kage online, og jeg vil virkelig gerne prøve opskriften selv, men der mangler en ingrediens!  
Hjælp mig venligst med at finde den hemmelige string!


## Challenge handout
#### Recipe.py
```python
def mix(a,b):
    """
    Procedure for mixing
    Take 2 letters of our dough string.
    The 2 letters' positions within the dough are swapped, so a ends up where b was and vice versa.
    """
    doughmix = list(dough)
    doughmix[a], doughmix[b] = doughmix[b], doughmix[a]
    doughmix = "".join(doughmix)
    return doughmix
"""

------------------

Flag cake recipe!

------------------
Ingredients:
* Flour
* Sugar
* Flag
* Milk
* Eggs
"""
# We start our recipe out with an empty dough
dough = ""
# Add flour, sugar and salt to the dough
dough += "flour"
dough += "sugar"
dough += "salt"
# Mix it all around until combined
dough = mix(0,2)
dough = mix(3,5)
dough = mix(2,9)
# Now with the dry ingredients properly mixed, we prepare the flag
#with open("flag.txt", "r") as f:
   #flag = f.read()
# Now make sure to remove "DDC{" and "}"!
# Wouldn't want that ending up in our cake, would we?
#flag = flag.replace("DDC{", "").replace("}", "")
flag = "lets_go_unbaking"
# Now add the flag into the dry ingredients and mix it lightly
dough += flag
dough = mix(5,18)
dough = mix(13,22)
dough = mix(2,23)
dough = mix(6,19)
dough = mix(15,17)
# Add all of the wet ingredients
dough += "milk"
dough += "eggs"
# Mix it all up
dough = mix(3,32)
dough = mix(29,31)
dough = mix(17,28)
dough = mix(8,28)
dough = mix(12,23)
dough = mix(21,24)
# Put the dough into the oven for 30 minutes, and out comes your delicious flag cake!
#with open("Cake.txt", "w") as f:
print(
f"""

                   )\

                  (__)

                   /\

                  [[]]

               @@@[[]]@@@

         @@@@@@@@@[[]]@@@@@@@@@

     @@@@@@@      [[]]      @@@@@@@

 @@@@@@@@@        [[]]        @@@@@@@@@

@@@@@@@           [[]]           @@@@@@@

!@@@@@@@@@                    @@@@@@@@@!

!    @@@@@@@                @@@@@@@    !

!        @@@@@@@@@@@@@@@@@@@@@@        !

!              @@@@@@@@@@@             !

!             ______________           !

!  {dough}  !

!             --------------           !

!!!!!!!                          !!!!!!!

     !!!!!!!                !!!!!!!

         !!!!!!!!!!!!!!!!!!!!!!!

""")
```

#### Cake.txt:
```

                   )                  (__)
                   /                  [[]]
               @@@[[]]@@@
         @@@@@@@@@[[]]@@@@@@@@@
     @@@@@@@      [[]]      @@@@@@@
 @@@@@@@@@        [[]]        @@@@@@@@@
@@@@@@@           [[]]           @@@@@@@
!@@@@@@@@@                    @@@@@@@@@!
!    @@@@@@@                @@@@@@@    !
!        @@@@@@@@@@@@@@@@@@@@@@        !
!              @@@@@@@@@@@             !
!             ______________           !
!  oligrunggflerk__oluubnstagmiagekss  !
!             --------------           !
!!!!!!!                          !!!!!!!
     !!!!!!!                !!!!!!!
         !!!!!!!!!!!!!!!!!!!!!!!

```

## Løsning af challenge
Dette er en ret simpel opgave, I og med at den blot mikser to ting sammen. Gøres dette i omvendt rækkefølge, så fås det originale flag. 

Solve script: 
```python
# Det er blot at gøre hver operation i omvendt rækkefølge og så fås flaget ved at fjerne "floursugar" og "milkeggs"

def mix(a,b):
    doughmix = list(dough)
    doughmix[a], doughmix[b] = doughmix[b], doughmix[a]
    doughmix = "".join(doughmix)
    return doughmix
    
dough = "oligrunggflerk__oluubnstagmiagekss"
dough = mix(21,24)
dough = mix(12,23)
dough = mix(8,28)
dough = mix(17,28)
dough = mix(29,31)
dough = mix(3,32)
dough = mix(15,17)
dough = mix(6,19)
dough = mix(2,23)
dough = mix(13,22)
dough = mix(5,18)
dough = mix(2,9)
dough = mix(3,5)
dough = mix(0,2)
flag = "DDC{"+dough.replace("floursugar","").replace("milkeggs","")+"}"
print(flag)
```
Dette gav flaget: 
DDC{lets_go_unbaking}
