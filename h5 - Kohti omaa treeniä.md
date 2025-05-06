# Kohti omaa treeniä

## Tiivistelmät

### Start Your Research with a Review Article

- Katsausartikkeli kokoaa yhteen tärkeimmät tiedot, joiden avulla pääsee sisälle aiheeseen tehokkaasti
- Laadulla on väliä (JUFO 1-3)
- Käytä uusia artikkeleita (alle 2 vuotta vanhoja)
- Google Scholar on hyvä paikka etsiä artikkeleita ("review" hakusanaksi)

### Vapaavalintainen katsausartikkeli

---

## HTB

### Dancing

Ensimmäisenä täytyy ladata openvpn config tiedosto

![image](https://github.com/user-attachments/assets/85b01da1-1567-46c7-9352-61014bd12013)

Tämän jälkeen voidaan yhdistää komennolla `sudo openvpn Downloads/starting_point_Joelicious.ovpn`

![image](https://github.com/user-attachments/assets/2e786798-9f4a-43ed-9dc6-f906e65abca0)

![image](https://github.com/user-attachments/assets/b4d2a9a1-3b7e-45e0-a143-682036bc165e)

---

#### Task 1

SMB ei ollut entuudestaan tuttu, mutta löysin oikean Wikipedia-sivun[^3] hakemalla `SMB protocol`.

Oikea vastaus on siis `Server Message Block`

![image](https://github.com/user-attachments/assets/c8536ad0-61bd-4d17-87f8-075dc4e4131f)

---

#### Task 2

Käytettävän portin löysin myös Wikipediasta 

![image](https://github.com/user-attachments/assets/5ea945a1-31ce-4f03-a405-3074735af82c)

---

#### Task 3

Tätä varten ajoin porttiskannauksen kommennolla `nmap -A -p445 10.129.203.149`. 

![image](https://github.com/user-attachments/assets/18e7f1ef-bf84-42d0-b447-af2779886e6c)

Skannauksen tuloksesta nähdään, että service on `microsoft-ds`

![image](https://github.com/user-attachments/assets/93080a4b-b82b-484a-8d55-e8939e7d6c02)

---

#### Task 4

`smbclient` työkalu ei ole entuudestaan tuttu, joten katsotaan vähän dokumentaatiota ajamalla `smbclient --help`

![image](https://github.com/user-attachments/assets/67982cf0-a5ae-4df8-aeef-0daa0fa9d4c2)

`-L` Näyttää oikealta 

![image](https://github.com/user-attachments/assets/f997222b-747d-404b-bdb9-f1ecac124f65)

---

#### Task 5

Käytetään äskeisen tehtävän komentoa: `smbclient -L 10.129.203.149`

![image](https://github.com/user-attachments/assets/79aa5cc1-db7b-43a5-94c7-5a97f159dfa3)

Tulosteesta nähdään, että on 4 SMB-jaettua kansiota: 

- `ADMIN$`
- `C$`
- `IPC$`
- `WorkShares`

![image](https://github.com/user-attachments/assets/cfec0b11-4d7f-4a42-b815-94733c05f009)

---

#### Task 6

Tehtävän vihje paljastaa, että sharet `ADMIN$, C$, IP$` luodaan aina, mutta `WorkShares` on adminin luoma kansio. 

En ollut ihan täysin tyytyväinen, joten päätin yrittää yhdistää sinne. Onneksi löysin sopivan ohjeen[^4], joka kertoi oikean komennon.

`smbclient \\\\10.129.203.149\\WorkShares`

![image](https://github.com/user-attachments/assets/7f9c9cbe-ca07-4105-9594-65a7fe5013b8)

Pääsin sisään. 

![image](https://github.com/user-attachments/assets/b76d7b2a-683b-442e-81ac-cc05b7503c21)

---

#### Task 7

Vihje ei auttanut yhtään, mutta aiemmin mainitsemani dokumentaatio kertoi, että `get` komennolla voidaan napata tiedostoja. 

`help` komento kertoo myös samaa

![image](https://github.com/user-attachments/assets/6bd4c277-93ef-4853-8127-2b4cfca4802d)

![image](https://github.com/user-attachments/assets/10c863c6-0115-4d54-b7f6-686a03a650e1)

---

#### Task 8

Selailen hetken hakemistoja, kunnes löydän `flag.txt` tiedoston

Ladataan tiedosto `get flag.txt` kommennolla

![image](https://github.com/user-attachments/assets/6bbf21a4-bddd-4385-9a5d-e8b9ce7ff210)

Tiedosto löytyy hakemistosta, josta käynnisti `smb` yhteyden

![image](https://github.com/user-attachments/assets/33f7cbaf-d13e-4a1a-9345-428584ad39cb)

![image](https://github.com/user-attachments/assets/0c7b8524-68cc-4fe5-af87-f12031458c42)

---

### Responder

#### Task 1

![image](https://github.com/user-attachments/assets/3257e868-bfdb-4f1f-8752-b64f44fe41f0)

Käydään katsomassa :) 

![image](https://github.com/user-attachments/assets/a4f29c9b-dbc6-48aa-9c11-9ca4cd553638)

![image](https://github.com/user-attachments/assets/be6b1247-d427-4778-9364-8c6230fdbed3)

---

#### Task 2

![image](https://github.com/user-attachments/assets/2edef82f-958b-424b-ab0a-cd56a7399a6b)

Tajusin tässä kohtaa, että verkkosivun kuuluisi varmaankin ladata, mutta näin ei käy minulla. Hetken pohdinnan jälkeen lunttaan mallivastauksesta[^5] komennon `echo "10.129.15.254 unika.htb" | sudo tee -a /etc/hosts`, jonka jälkeen sivu aukeaa selaimessa. 

![image](https://github.com/user-attachments/assets/a0629d7f-892e-447a-a56f-d084e3065f1d)

URL-kentästä nähdään, että kieli on `php`. 

![image](https://github.com/user-attachments/assets/946ff9d5-c4e0-459f-bc1d-83927732ea95)

![image](https://github.com/user-attachments/assets/f35f540a-c23b-4119-8d72-e26a8dc6fb03)

---

#### Task 3

![image](https://github.com/user-attachments/assets/4acfa653-caa2-4b8d-b1ee-3cc36988f1b5)

Tämänkin näkee helposti URL-kentästä kun vaihtaa sivuston kieltä

![image](https://github.com/user-attachments/assets/cdafab17-978d-4118-ab64-f0d57475cdd7)

![image](https://github.com/user-attachments/assets/ac9a8522-dea3-4e21-a17e-bd78b7b5417a)

---

#### Task 4

![image](https://github.com/user-attachments/assets/53bc9016-ee55-4247-a19b-24b045990637)

Vastauskenttä paljastaa oikean vastauksen

![image](https://github.com/user-attachments/assets/983f532f-34b8-445e-b4dc-0d37817b988f)

---

#### Task 5

![image](https://github.com/user-attachments/assets/129f2736-e1c9-443d-a2d9-4400b909bca7)

Tässäkin tehtävässä tuo vastauskenttä paljastaa oikean vastauksen

![image](https://github.com/user-attachments/assets/e2d4f389-f151-4406-ad97-1136e6f92bda)

---

#### Task 6

![image](https://github.com/user-attachments/assets/9536dd12-0235-4698-a725-cc28b153700e)

Googlaamalla selvisi, että NTLM (New Technology Lan Manager) on `suite of Microsoft security protocols intended to provide authentication, integrity, and confidentiality to users.`[^6]

![image](https://github.com/user-attachments/assets/2590a358-82ee-44ad-a73d-c752c9bcacea)

---

#### Task 7

![image](https://github.com/user-attachments/assets/9788704c-37bc-47c1-b013-fc13173dc5e4)

Responder-työkalu ei ole tuttu entuudestaan, mutta `--help` auttaa

![image](https://github.com/user-attachments/assets/4aeab244-8b35-4ae7-ba6b-b76339a91524)

---

#### Task 8

John the Ripper on jo vanha tuttu kurssilta

![image](https://github.com/user-attachments/assets/715648bc-e44b-42d4-b5c2-3f611f854f91)

---

#### Task 9

![image](https://github.com/user-attachments/assets/2ef02305-e7b6-4ef8-8f03-fa141e93cae0)



## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa https://terokarvinen.com/tunkeutumistestaus/, luettu 5.5.2025

Karvinen Tero, Start Your Research with a Review Article, luettavissa https://terokarvinen.com/review-article/, luettu 5.5.2025

HackTheBox, Starting-point, luettavissa https://app.hackthebox.com/starting-point, luettu 6.5.2025

[^3]: Wikipedia, Server Message Block, luettavissa https://en.wikipedia.org/wiki/Server_Message_Block, luettu 6.5.2025

[^4]: LearnLinux, Using smbclient, luettavissa https://www.learnlinux.org.za/courses/build/net-admin/ch08s02.html, luettu 6.5.2025

[^5]: HackTheBox, Responder writeup, luettavissa blob:https://app.hackthebox.com/4352371d-4fea-4d46-afde-89295f31c507, luettu 6.5.2025

[^6]: Wikipedia, NTLM, luettavissa https://en.wikipedia.org/wiki/NTLM, luettu 6.5.2025
