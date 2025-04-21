# Leviämässä

## Tiivistelmät

### Karvinen 2022: Cracking Passwords with Hashcat

Tässä tutoriaalissa Karvinen opettaan hashattujen salasanojen avaamista Hashcatin avulla.

- Salasanoja ei säilötä niiden alkuperäisessä muodossa, vaan järjestelmät tallentavat niistä "hash" arvon
  - Hash arvoa ei voi muuttaa takaisin alkuperäiseksi salasanaksi, mutta voidaan tutkia kaikki sanakirjan sanat ja katsoa mistä muodostuu sama hash arvo
- Aluksi täytyy tunnistaa minkä tyyppinen hash on kysesssä. Tämän voi tehdä komennolla `hashid -m`

### Karvinen 2023: Crack File Password With John

Tässä tutoriaalissa Karvinen esittelee John The Ripper työkalun. John The Ripper on tehokas työkalu salasanojen murtamiseen.

- helppo käyttää
- voi murtaa monta formaattia

### Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials

- Käyttäjänä suojautuminen: Vahvat salasanat, 2FA
- Helppoja tapoja: Käyttää laitteiden / ohjelmien default salasanoja (osa käyttäjistä liian laiskoja vaihtamaan) tai MITM hyökkäykset, jossa kuunnellaan oman hotspotin liikennettä
- Salasanojen murtaminen helpompaa kuin ennen
  - Nopeammat prosessorit, heikot algoritmit, hyvät sanakirjat

### Kennedy et al 2025: Metasploit: File-Format Exploits (Wrapping Up loppuun)

Tekstissä kerrotaan, miten haavoittuvuus Wordissä mahdollisti etäkoodin suorittamisen. 

Esimerkissä hyökkäys valmistellaan metasploitin avulla, jossa uhrin koneelle annetaan haitallinen .docx tiedosto. 

Kun uhri avaa .docx tiedoston, hänen koneensa avaa meterpreter shellin ja ottaa yhteyden hyökkääjän koneeseen (`reverse_tcp`)

### Singh 2025: The Ultimate Kali Linux Book: Understanding Active Directory

Active Directory on Microsoft Windows serverin hakemistopalvelu, jonka avulla käyttäjät voivat kirjautua verkkoon yhdellä tunnuksella

Keskisiä käsitteitä:

- Forest - Korkein hallinnollinen taso, koostuu yhdestä tai useammasta domainista
- Domain - kokonaisuus, jossa käyttäjiä, ryhmiä tai ja laitteita
- Eri trust-mallit:
  - One-way trust
  - Two-way trust
  - Transitive trust
  - Non-transitive trust
  - Forest trust
- NTLM v2 hash
- LDAP-protokolla

---

## Hashcat

### Asennus

Hashcat ja Hashid on jo valmiiksi asennettuna Kali Linuxiin

Ladataan rockyou sanakirja komennoilla

```
wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
tar xf rockyou.txt.tar.gz
rm rockyou.txt.tar.gz
```

![image](https://github.com/user-attachments/assets/2b352ec1-cf9e-4dce-b6cc-dec532469ef8)

### Salasanan murtaminen

Valitsin esimerkkisalasanaksi `password123`

Hashaan sen md5 algoritmin kanssa: ```echo -n "password123" | md5sum```

Saadaan hash `482c811da5d5b4bc6d497ffa98491e38`

![image](https://github.com/user-attachments/assets/7dd09d03-ec47-44b8-b514-f6ba8b2dcb93)

Harjoituksen vuoksi yritetään tunnistaa minkä tyyppinen hash meillä on. 

Ajetaan komento `hashid -m 482c811da5d5b4bc6d497ffa98491e38`

hashid epäilee, että hash olisi md2, md5, md4, double md5 yms. tyyppiä. hashid ei voi olla varma hashin tyypistä, sillä moni muu algoritmi luo myös 32-merkkisen heksadesimaalihajautuksen. 

![image](https://github.com/user-attachments/assets/a02e5f37-2b83-43f5-a201-1ac13afa4469)

Onneksi tiedän, että hajautukseen on käytetty md5 algoritmiä, joten minun ei tarvitse kokeilla eri komentoja. 

hashcat komento: 
```
hashcat -m 0 '482c811da5d5b4bc6d497ffa98491e38' rockyou.txt -o solved 
```

Katsotaan output tiedoston sisältö komennolla `cat solved`

![image](https://github.com/user-attachments/assets/6bb2d0e3-68c4-44e9-b345-ecb091f6ba69)

---

## John The Ripper

### Asennus 

Valmiiksi asennettuna Kali Linuxiin

### Esimerkkitiedoston murtaminen

Murretaan Karvisen John The Ripper tutoriaalissa käyttämää .zip tiedostoa. 

`wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip`

Ajetaan komento `zip2john tero.zip > tero.zip.hash`, jonka jälkeen voimme murtaa salasanan kommennolla `john tero.zip.hash`

![image](https://github.com/user-attachments/assets/a792612d-2bf7-4d8f-b0fb-5c0c264b84ee)

Salasana löytyi: `butterfly`

![image](https://github.com/user-attachments/assets/33ec473c-1b26-4f9a-90dd-e7e5be8eec8a)

---

## Tiedosto

Halusin testata murtaa .pdf tiedoston salauksen. Etsin netistä salattuja .pdf tiedostoja ja löysin tämän: 



![image](https://github.com/user-attachments/assets/e43306a0-7305-4b4b-8f7d-ad96cfb5a44f)


---

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa https://terokarvinen.com/tunkeutumistestaus/, luettu 21.4.2025

Karvinen Tero, Cracking Passwords with Hashcat, luettavissa https://terokarvinen.com/2022/cracking-passwords-with-hashcat/, luettu 21.4.2025

Karvinen Tero, Crack File Password With John, luettavissa https://terokarvinen.com/2023/crack-file-password-with-john/, luettu 21.4.2025

O'Reilly, Security Penetration - Testing The Art of Hacking, katsotttavissa https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_01_00/ (maksumuurin takana), katsottu 21.4.2025

O'Reilly, Metasploit 2nd edition, Client-Side Attacks, luettavissa https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#toc-link_128 (maksumuurin takana), luettu 21.4.2025

O'Reilly, The Ultimate Kali Linux Book Third Edition, Understanding Active Directory, luettavissa https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-271 (maksumuurin takana), luettu 21.4.2025
