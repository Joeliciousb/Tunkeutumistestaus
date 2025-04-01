# Kybertappoketju

## Tiivistelmät

### Herrasmieshakkerit - Vastaamo

Tässä podcast-jaksossa puhutaan Vastaamon tietomurrosta, jotka tapahtuivat vuonna 2018-2019. Vieraana on Keskusrikospoliisin ylikomissaario Marko Leponen, joka oli tietomurron tutkinnanjohtaja.  
Jaksossa keskustellaan tietomurron teknisestä toteutuksesta ja Aleksanteri Kivimäen virheistä jotka johtivat hänen kiinnijäämiseensä.  

### Hutchis et al 2011

Tämä teksti kertoo kybertappoketjun eri vaiheista

- Tiedustelu: Tutkitaan ja tiedustellaan kohdetta
- Aseistus: Rakennetaan paketti, joka sisältää troijalaisen
- Toimitus: Aseistettu paketti lähetetään kohteeseen
- Hyväksikäyttö: Aseistettu paketti ottaa hyökätyn koneen haltuun
- Asennus: Asennetaan haittaohjelma etäyhteyden avulla, jotta saadaan pysyvä pääsy järjestelmään
- Komento ja hallinta (C2): Järjestelmä ottaa yhteyden internettiin, jotta C2 kanava saadaan muodostettua.  
- Toimet tavoitteiden saavuttamiseksi: Kun aiemmat toimet on hoidetu, hyökkääjä voi alkaa työstämään kohti heidän tavoitteita

### € Santos et al 

- Kohteen on mahdollista havaita tiedustelu

#### nmap
- sS: tekee osan TCP yhteydestä
- vv: verbocity level
- T4: lisää nopeutta
- A: Tunnistaa käyttöjärjestelmän
- Pn: hyödyllinen online skannauksissa
- tulosteen voi tallentaa tiedostoon

#### masscan
- paljon ja nopeasti

#### EyeWitness
- kuvankaappauksia verkkosivuista

### KKO 2003:36

- A oli porttiskannannut osuuspankin tietojärjestelmiä
- Käräjäoikeus hylkäsi syytteen, sillä näyttöä ei ollut riittävästi
- Hovioikeus katsoi että näyttöä on riittävästi ja määräsi A:n korvaaman vahingot osuuspankille
- Korkeimmassa oikeudessa A todettiin syylliseksi

---

## Kali asennus

Kali pyörii Virtualboxissa. 

![Screenshot_2025-04-01_11_33_42](https://github.com/user-attachments/assets/9b75f0ea-77e8-4063-8209-1fbcdde6cb97)

--- 

## Verkosta irroittaminen

Laitteella on yhteys verkkoon. Tämä voidaan todistaa ping-komennon avulla.

![image](https://github.com/user-attachments/assets/d9c9bd2e-27af-4c9c-9a70-8a01c95a9a0d)

Verkkoyhteys saadaan katkaistua näytön oikeassa yläkulmassa olevasta työkalupalkista. 

![image](https://github.com/user-attachments/assets/385d7de1-ceef-4b48-876e-326a0772a8ec)

--

![image](https://github.com/user-attachments/assets/cdd9a4bf-c4a4-40ac-afe7-1bcc06a06e0a)

Nyt jos kokeilemme samaa `ping`-komentoa

![image](https://github.com/user-attachments/assets/be3878fc-ce59-462f-972a-98990779b81a)

Kokeillaan yhteyttä vielä selaimen kautta varmuudeksi

![image](https://github.com/user-attachments/assets/37e3e6d3-da80-46c1-86f0-67e64ab08b41)

Nettiyhteys on katkaistu

--- 

## Oman koneen porttiskannaus

![image](https://github.com/user-attachments/assets/765d3f6a-3478-4972-b405-0d46632bd0ec)

Parametrit: 
- T4: Olettaa nettiyhteyden olevan hyvä, joten skannaus on nopeampi
- A: Hakee myös tietoa käyttöjärjestelmästä
- localhost: porttiskannauksen kohde

Skannauksesta huomataan, että localhost:issa ei ole yhtään avointa porttia. 

--- 

## Demonien asennus

Kali Linux:ssa on asennettuna jo valmiiksi MySQL ja apache2. Käynnistetään daemonit. 

![image](https://github.com/user-attachments/assets/6127a59b-0e23-4aa9-bd26-06f53ad058c5)

--

![image](https://github.com/user-attachments/assets/9910894f-bc1e-4d8d-a07c-3b6bd537fe4a)

Skannataan uudelleen. 

![image](https://github.com/user-attachments/assets/2cd5c624-5da0-41fa-a9e2-46db57a19774)

Skannaus löysi 2 avointa porttia: 80 jota apache käyttää & 3306 jota mariaDB käyttää. 



## Lähteet

https://terokarvinen.com/tunkeutumistestaus/

https://www.withsecure.com/fi/whats-new/podcasts/herrasmieshakkerit

https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf

https://www.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/
