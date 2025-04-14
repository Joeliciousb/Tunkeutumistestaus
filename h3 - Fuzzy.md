# Fuzzy

## Tiivistelmät

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

Tutoriaalissa Karvinen esittelee ffuf työkalun, kertoo miten se ladataan ja opettaa asentamaan maaliympäristön johon voi testata työkalua. 
Tutoriaalissa on myös kaksi harjoitusta, jotka on tarkoitus tehdä itse. 

### ffuf - README.md

ffuf (Fuzz Faster U Fool) on suosittu työkalun fuzzaamiseen. Työkalun GitHub sivulla on README.md tiedosto, jossa esittellään ffuf, sen asennusohjeet sekä työkalun käyttöä esimerkkien avulla. 

## Fuzzzz

Latasin binäärin `dirfuzt-1` Karvisen sivuilta (Find Hidden Web Directories - Fuzz URLs with ffuf) ja käynnistin maaliympäristön

![image](https://github.com/user-attachments/assets/5ebeb73d-43bd-4aae-9fe9-9619ffd19561)

![image](https://github.com/user-attachments/assets/be81ff22-428f-4fe5-981e-b8c6357f35c2)

Ensimmäisenä testin fuzzata wordlist flagin avulla: ```ffuf -w common.txt -u http:127.0.0.2:8000/FUZZ```

![image](https://github.com/user-attachments/assets/9ef96558-ca61-4f10-9c47-8019afed5b80)

Huomataan, että suurin osa vastauksista on turhia ja ne halutaan suodattaa pois. Kätevästi epätoivottujen löytöjen koko on 154 tavua, joten lisätään filter flag `-fs 154`.

![image](https://github.com/user-attachments/assets/e458ae2a-b56b-4f32-841d-bf74f8109f0d)

Nyt nähdään vain muutama tulos, joista yksi on `wp-admin` osoite. Avataan selaimessa. 

![image](https://github.com/user-attachments/assets/17d5b746-eb84-4e3e-8e81-8b1d287a806e)

---

## Ffufme

### Maaliympäristö

Seurasin `FFUF Me - Target Practice For FFUF` asennusohjeita GitHubista. 

Ensimmäisenä asensin Dockerin kommennolla `sudo apt install docker.io`

Tämän jälkeen kloonasin ffufme repon

![image](https://github.com/user-attachments/assets/289f8eff-7c78-4784-ade7-267e4a988380)

Seuraavaksi ajoin komennot `sudo docker build -t ffufme .` > `sudo docker run -d -p 80:80 ffufme`

![image](https://github.com/user-attachments/assets/7e1d1b66-3c17-4fbc-9265-9f0dd479cf9b)

Testataan menemällä `http://localhost`

![image](https://github.com/user-attachments/assets/e3cd9027-f99d-40bd-80ad-12a50319493d)

Maaliympäristön asennus onnistui

Ladataan vielä tarvittavat worldlistit. Kopioidaan komennot sivustolta ja ajetaan ne

![image](https://github.com/user-attachments/assets/ef21c74e-9bdd-4ff6-8ace-f05bb5452b75)

```
cd ~
mkdir wordlists
cd wordlists
wget http://localhost/wordlist/common.txt
wget http://localhost/wordlist/parameters.txt
wget http://localhost/wordlist/subdomains.txt
```

![image](https://github.com/user-attachments/assets/52dd68ee-ad9c-4ea2-9e69-1436e9ab3ae5)

### Ffufme harjoitukset

---

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa: https://terokarvinen.com/tunkeutumistestaus/, luettu 11.4.2025

Karvinen Tero, Find Hidden Web Directories - Fuzz URLs with ffuf, luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/, luettu 14.4.2025

BuilHackSecure, fuffme.md, luettavissa https://github.com/BuildHackSecure/ffufme, luettu 11.4.2025

ffuf, README.md, luettavissa: https://github.com/ffuf/ffuf/blob/master/README.md, luettu 14.4.2025
