# Kybertappoketju

## Tiivistelmät

### Herrasmieshakkerit - Vastaamo

Tässä podcast-jaksossa puhutaan Vastaamon tietomurrosta, joka tapahtui vuosina 2018-2019. Vieraana on Keskusrikospoliisin ylikomissaario Marko Leponen, joka oli tietomurron tutkinnanjohtaja.  
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

---

## Metasploitable 2 asennus

Ladataan Metasploitable 2. 

![image](https://github.com/user-attachments/assets/33fb2012-df1c-4e48-b9d1-d1865a951cdd)

Puretaan ladattu .zip ja avataan VirtualBox. Luodaan uusi virtuaalikone. 

![image](https://github.com/user-attachments/assets/97ae15fb-b685-4302-8df3-a436e3862b2a)

Valitaan "Use an existing virtual hard disk file" ja liitetään ladattu tiedosto. 

![image](https://github.com/user-attachments/assets/ced175ad-6720-4885-a22f-c94e23fad486)

---

## Virtuaaliverkko koneiden välille

Luodaan Host-only verkko

![Screenshot 2025-04-02 001325](https://github.com/user-attachments/assets/30f1ab68-28cd-4adb-976d-341e7c4a2f07)

--

#### Kali verkkoasetukset

Adapter1 NAT, koska haluan että voin myös tarvittaessa päästä nettiin  

![Screenshot 2025-04-01 232803](https://github.com/user-attachments/assets/63da5836-0992-46a4-8b01-fc118656b3c4)
  
Adapter2 käyttää Host-only verkkoa  

![Screenshot 2025-04-01 232820](https://github.com/user-attachments/assets/2edf1e6b-c810-4459-a4a3-980a42b15c10)

#### Metasploitable2 verkkoasetukset

Käyttää Host-only verkkoa  

![Screenshot 2025-04-01 232547](https://github.com/user-attachments/assets/c0008d80-1483-4a5b-a236-e99e3f3a0d20)

Todistus, että Kali saa yhteyden Metasploitableen, mutta ei verkkoon.

![image](https://github.com/user-attachments/assets/55deed10-27b9-4cba-98a9-9094f2a70519)

Todistus, että Metasploitable saa yhteyden Kaliin, mutta ei verkkoon. 

![screenshot](https://github.com/user-attachments/assets/5e812bda-840b-4953-a461-061bc4b8af46)

---

## Etsi Metasploitable

Porttiskannataan koko verkko

![image](https://github.com/user-attachments/assets/65b841a4-9c98-4785-89d4-30234aea86ee)

Verkosta löytyy 4 ip osoitetta. Tiesin jo, että 192.168.56.111 on oikea ip osoite, mutta sen olisi saanut selvimme myös kokeilemalla. 

![image](https://github.com/user-attachments/assets/824ba13e-bf01-4da8-b1d5-c5c158259100)  

---  

## Metasploitable porttiskannaus

Tehdään porttiskannaus komennolla ```nmap -T4 -A -p- 192.168.56.101```. 

![image](https://github.com/user-attachments/assets/55596e22-b859-4061-8a61-dafff40c796b)

Porttiskannaus paljasti ssh-avaimia

![image](https://github.com/user-attachments/assets/6d0ef140-40cc-430e-b601-dcfe3af1c94b)

ja myös avoimen MySQL portin 

![image](https://github.com/user-attachments/assets/23fd74e4-d195-4bd6-a456-d6e839b81b66)

---

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa: https://terokarvinen.com/tunkeutumistestaus/, luettu 2.4.2025

Herrasmieshakkerit, Tapaus Vastaamo, kuunneltavissa: https://www.withsecure.com/fi/whats-new/podcasts/herrasmieshakkerit, kuunneltu: 1.4.2025

Lockheed Martin Corporation, Intelligence-Driven Computer Network Defense
Informed by Analysis of Adversary Campaigns and
Intrusion Kill Chains, luettavissa https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf, luettu 1.4.2025

The Art of Hacking (Video Collection), katsottavissa https://www.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/, katsottu 2.4.2025
