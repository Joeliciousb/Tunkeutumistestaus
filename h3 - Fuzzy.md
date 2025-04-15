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

#### Basic Content Discovery 

Tavoitteena löytää tiedostot `class` & `development.log`. 

Ajetaan komento ```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/basic/FUZZ```

![image](https://github.com/user-attachments/assets/6af385c9-91b8-4a17-8aa8-a1795171d0b2)

---

#### Recursion

Tavoitteena löytää `/admin/users/96` tiedosto

Ajetaan komento `ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ` 
  - `-recursion` flagi tarkoittaa, että mikäli ffuf löytää hakemiston, se ajaa uuden skannauksen myös siihen hakemistoon

![image](https://github.com/user-attachments/assets/81c8ad0e-bd4d-4f6c-bb99-3a193ea1a2ee)

---

#### File extensions

Tavoitteena löytää tiedosto `users.log` tiedosto `/logs` hakemistosta

Käytetään komentoa `ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ`
  - `e .log` määrittelee, että haetaan tiedostoja joiden filetype on = log

![image](https://github.com/user-attachments/assets/80e71e3b-30ee-445d-80c7-9bbaf715b5d2)

---

#### No 404 Status

Tavoitteena löytää `secret` tiedosto

Jos ajetaan npc ffuf komento huomataan, että kaikki vastaukset ovat status 200 size 669. 

![image](https://github.com/user-attachments/assets/efe7f1bc-df6f-409d-9fa1-391d8a2ca839)

Lisätään komentoon `-fs` joka suodattaa pois tietyn kokoiset vastaukset, eli 669 tavua tässä tapauksessa

![image](https://github.com/user-attachments/assets/cdd7f728-ca7d-47be-a47c-23234b9c956f)

---

#### Param Mining

Tavoitteena on löytää oikea parametri 

![image](https://github.com/user-attachments/assets/67c01657-0b09-4b88-8b88-5585bd6b70f9)

Ajetaan komento `ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1`
  - Komento sovittaa jokaista wordlistin riviä parametriksi, (id=1, test=1, debug=1 jne.)

![image](https://github.com/user-attachments/assets/dc44e5a3-6a4c-4660-b9c7-c39af4f1183a)

---

#### Rate Limit

Tavoitteena ohittaa palvelimen rate limit ja löytää `oracle` tiedosto

Rate limit aiheuttaa sen, että jos kokeillaan komentoa `ffuf -w ~/wordlists/common.txt -u http://ffuf.test/cd/rate/FUZZ -mc 200,429`, vauhti on liian kova ja palvelin vastaa 429 statuksella, eli tilapäiset bännit

![image](https://github.com/user-attachments/assets/0551209b-6514-4039-84dc-169b66a8a921)

Tämän voi ohittaa muokkaamalla komentoa `ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://ffuf.test/cd/rate/FUZZ -mc 200,429`
  - `-t` = "Thread", eli kuinka monta pyyntöä lähetään yhtä aikaa
  - `p` = "Pause", eli ohjelma pitää määritellyn tauon jokaisen pyynnön välissä
Nyt pyyntöjä lähtee `50/s`

![image](https://github.com/user-attachments/assets/3144d115-f8c9-42db-bf1e-3bfba9d4ba07)

---

#### Virtual Host Enumeration

Tavoite löytää subdomain `redhat`

Ajetaan komento `ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost`
  - `-H "Host: FUZZ.ffuf.me"` = vaihtaa HOST-otsikon wordlistin arvoihin

![image](https://github.com/user-attachments/assets/61f80a3c-5fae-47ff-bf73-e79bcab32c87)

Vastauksilla on sama koko, eli 1495 tavua. Lisätään `-fs 1495` komentoon.

![image](https://github.com/user-attachments/assets/eec522a2-10e1-4879-babb-3dbd185fb574)

---

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa: https://terokarvinen.com/tunkeutumistestaus/, luettu 11.4.2025

Karvinen Tero, Find Hidden Web Directories - Fuzz URLs with ffuf, luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/, luettu 14.4.2025

BuilHackSecure, fuffme.md, luettavissa https://github.com/BuildHackSecure/ffufme, luettu 11.4.2025

ffuf, README.md, luettavissa: https://github.com/ffuf/ffuf/blob/master/README.md, luettu 14.4.2025
