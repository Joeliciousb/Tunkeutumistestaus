# Täysin laillinen sertifikaatti

---

## Tiivistelmät

### OWASP Top 10: Broken Access Control

- Yleisin riski OWASP Top 10: 2021 listassa
- Access Control tarkoittaa periaatetta, että käyttäjä voi toimia vain käyttäjälle tarkoitettujen oikeuksien mukaan.
- Sovelluksen tulisi varmistaa, että käyttäjä on kuka hänen luullaan olevan eikä vain olettaa niin.
- Yleisiä hyökkäyksiä ovat "force browsing" ja parametrien muokkaus
  - Esimerkiksi adminpaneelin tulisi olla näkyvä vain admin-käyttäjille, mutta tarkistaako sovellus käyttäjän oikeudet, jos navigoi /admin päätepisteeseen?
  - Jos käyttäjä muokkaa kyselyn parametreja, voiko hän saada jonkun toisen käyttäjän tiedot näkyviin.

### OWASP Top 10: Server Side Request Forgery (SSRF)

- Sijalla #10 OWASP Top 10: 2021 listassa
- Hyökkääjä voi huijata palvelimen tekemään pyyntöjän paikkoihin esimerkiksi sisäisiin verkkoihin
- Sovellus on haavoittunut, jos se hyväksyy käyttäjän syöttämän URL-osoitteen sellaisenaan
- Hyökkääjä voi hakea hyödyntää haavoittuvuutta lähettämällä pyynnön esimerkiksi localhost:28017/ tai muihin sisäverkon päätepisteisiin

### PortSwigger Academy Insecure Direct Object Reference (IDOR)

- Kun sovellus käyttää käyttäjän antamaa syötettä suoraan sellaisenaan olioiden käsittelyssä
- Esimerkiksi sivu, joka näyttää käyttäjän tiedot osoitteessa ```https://example/customer?customer_id=1```
   - Mitä jos id arvon vaihtaa, näkeekö toisen käyttäjän tiedot?
- Sama pätee myös tiedostejen lukemisessa: ```https://example/static/101.txt```

### PortSwigger Academy Path Traversal

- Hyökkäys, joka mahdollistaa tiedostojen lukemisen, joihin ei normaalisti olisi pääsyä
- Esimerkiksi HTML-sivu voi ladata kuvan koodilla ```<img src="/loadImage?filename=218.png">```
  - Mitä jos hyökkääjä muokkaa filename arvon ```../../../etc/passwd```?
  - Tämä palauttaisi tiedoston, josta hyökkääjä näkisi käyttäjien tietoja
- Paras tapa suojautua on välttää syöttämästä käyttäjän syötettä suoraan rajapintoihin

### PortSwigger Academy Server-side Request Forgery (SSRF)

- Hyökkäys, jossa hyökkääjä huijaa palvelinta lähettämään pyyntöjä kohteisiin, joihin hyökkääjä ei normaalisti pääsisi itse
- Sovellukset voivat olla turhan sinisilmäisiä pyyntöihin, jotka tulevat sisäverkosta
- Esimerkiksi palvelin lähettää pyynnön sisäverkkoon, joka hakee tuotteen myymäläsaatavuuden
  - Hyökkääjä muuttaa pyynnön sisältöä, jotta pyyntö hakeekin ```localhost/admin``` näkymän

### PortSwigger Academy Cross-site Scripting (XSS)

- Hyökkäys, jossa syötetään haitallista JavaScript-koodia käyttäjän selaimeen
- Kolme päätyyppiä: Reflected XSS, Stored XSS ja DOM-based XSS
  - Reflected XSS hyökkäyksissä haitallinen koodi toimitetaan esimerkiksi URL-osoitten kautta
  - Stored XSS hyökkäyksissä haitallinen koodi tulee suoraan palvelimelta, esimerkiksi käyttäjän kommentti keskustelupalstalla
  - DOM-based XSS 

---

## Totally legit certificate

Zapin asennus komennolla ```sudo apt-get install zaproxy```

![image](https://github.com/user-attachments/assets/3f318257-9ce5-43f8-8ff7-b77e9fb0c282)

Avataan Zap ja mennään ```Tools > Options```

![image](https://github.com/user-attachments/assets/39ddf72a-7177-4a7f-9ac4-24c0d8fcd15e)

Haetaan sertifikaatti

![image](https://github.com/user-attachments/assets/a6c566c3-3362-4cb4-9c5d-1c6c7bdd966c)

Tallennetaan sertifikaatti

![image](https://github.com/user-attachments/assets/cf696bb8-badd-48f7-9e83-65f523175450)

Mennään selaimen asetuksiin

![image](https://github.com/user-attachments/assets/f9a4e130-b0bb-4d52-88cf-4132c656d0ee)

Importataan tallennettu sertifikaatti

![image](https://github.com/user-attachments/assets/66d5ee05-be93-463b-9213-8a6639ce5206)

![image](https://github.com/user-attachments/assets/6410617d-0163-4819-a711-bcd7778024cb)

![image](https://github.com/user-attachments/assets/8d936617-c3b5-45b8-9345-fda672193bbf)

Laitetaan ZAP vielä käsittelemään kuvia asetuksista: ```Tools > Options > Display```.

![image](https://github.com/user-attachments/assets/fc7c24a2-861f-4151-a992-50715041fc14)

Vaihdetaan selaimen proxy asetuksia

![image](https://github.com/user-attachments/assets/21e3c173-4a84-4a9f-8013-4d6ee8e3898d)

![image](https://github.com/user-attachments/assets/d782ffe3-8a49-485e-a824-889b5e3f3c4f)

Nyt näemme, että proxy kaappaa liikennettä

![image](https://github.com/user-attachments/assets/751176c2-6059-4b77-a998-6d03a1c47fbe)

---

## FoxyProxy

Asennetaan FoxyProxy extension

![image](https://github.com/user-attachments/assets/5431a040-8053-480c-ae0f-97b7dce9b9a8)

Muokataan ```proxies > localhost```. Lisätään sinne kaksi patternia:
- Ensimmäisen tyyppi on `Reg Exp` ja pattern `https://portswigger.net`
- Toisen tyyppi on `Wildcard` ja pattern `*://localhost/*`

![image](https://github.com/user-attachments/assets/19494831-a4b1-47b6-bdc8-b9addef6429e)

Laitetaan FoxyProxyn asetus "Proxy by Patterns" päälle.

![image](https://github.com/user-attachments/assets/4db41f1b-f400-4a8a-8ed3-b286e5e2d5d5)

Nyt proxyyn tulee näkyviin liikkeet vain osoitteessa ```https://portswigger.net``` tai ```localhost```. 

---

## PortSwigger Labs

### Cross Site Scripting (XSS)

#### Reflected XSS

Tehtävässä täytyy kutsua `alert()` metodia Cross-site scripting avulla. Vihjataan, että haavoittuvuus on search toiminnossa

![image](https://github.com/user-attachments/assets/767b3f4a-34a9-41f7-b2df-0c6fd0472bda)

Malliympäristö on sivu jossa ihmiset voivat julkaista blogeja ja jättää niihin kommentteja

![image](https://github.com/user-attachments/assets/ab38f60e-d9b0-4d1d-a564-fe3003ff3d48)

![image](https://github.com/user-attachments/assets/25bfe410-604a-408d-b6d3-5fa441fd6601)

Kokeilin syöttää script tägin hakukentän kautta

![image](https://github.com/user-attachments/assets/186d81f8-7b28-42e5-b0b5-0a13ae4d1a02)

Hyökkäys toimi, ja sivusto renderöi alert() metodin.

 ![image](https://github.com/user-attachments/assets/cbe69631-bef6-4d41-aea9-c526ed6c2847)

 ![image](https://github.com/user-attachments/assets/1f183cf6-1611-457d-9d8a-d6d4b003606c)

 ---- 

#### Stored XSS

Tässä tehtävässä pitää kutsua alert() metodia sivuston kommenttikentän avulla. 

![image](https://github.com/user-attachments/assets/bf76ebe3-d37f-4995-b02a-bb0706b7f708)

Sivusto on täysin sama kuin edellisessä tehtävässä, joten aion kokeilla jättää kommentin mihin olen upottanut `<script></script>` tägin.

![image](https://github.com/user-attachments/assets/c586d3e6-3bf1-4575-8116-a37b3f0e554a)

Kommentin postaamisen jälkeen sivustolle meneminen aiheuttaa alert() metodin

![image](https://github.com/user-attachments/assets/cbe69631-bef6-4d41-aea9-c526ed6c2847)

![image](https://github.com/user-attachments/assets/5b829874-bbe1-4d07-af9c-9727b24b0500)

----

### Path traversal

#### Simple case

Tässä tehtävässä täytyy saada passwd tiedosto

![image](https://github.com/user-attachments/assets/3c1cab2b-c58d-4e3d-9cbc-c6f88a41a951)

Sivu näyttää tältä

![image](https://github.com/user-attachments/assets/98092491-d7d7-4ce6-86bd-f32b4166a9a7)

![image](https://github.com/user-attachments/assets/815a1bab-2bc7-4969-b4c6-177a013ce733)

Pyyntö Zapissa, kun mennään valitun tuotteen sivulle

![image](https://github.com/user-attachments/assets/037fcdc5-4ac1-4b34-87c9-a6d25bbf2d56)

Eli sivusto lähettää GET-pyynnön osoitteeseen `https://0ac900c2043704e7805a145f00f3006f.web-security-academy.net/image?filename=68.jpg`  
Tästä herää heti ajatus, että mitä jos lähetän uuden pyynnön, jossa olen muokannut filepathia. 

Avataan pyyntö Zapin requester välilehdelle

![image](https://github.com/user-attachments/assets/e0faf478-468f-422d-8246-3e4e825188bd)

Kokeillaan lähettää pyyntö, jossa on muokattu filepath

![image](https://github.com/user-attachments/assets/336ba117-55d7-48dc-acc5-b7d710da3474)

![image](https://github.com/user-attachments/assets/0ad49239-b32f-4ed4-9b58-9d4e55f6a6c7)

Tiesin, että minulla on oikea idea, mutta filepath / syntaksi oli vain väärä, joten kokeilin seuraavat 10 minuuttia lähettää pyyntöjä eri filepath arvoilla kunnes löysin oikean: 

`filename=../../../etc/passwd`

![image](https://github.com/user-attachments/assets/cae1dd51-9bc7-42ba-9814-376889ab8da4)

![image](https://github.com/user-attachments/assets/5e5c047e-8b57-4cf5-8f17-495b526b3106)

----

#### Traversal sequences blocked with absolute path bypass

Tässäkin tehtävässä pitää saada passwd tiedosto

![image](https://github.com/user-attachments/assets/6044c302-02e3-4ec4-8c4c-66a17b76661e)

Sivusto näyttää tältä

![image](https://github.com/user-attachments/assets/7270056f-1593-48ce-a7ea-ee7f439e97e5)

Zapissa nähdään miten kuvat haetaan

![image](https://github.com/user-attachments/assets/17462276-93c5-4406-9368-3a843cf033d4)

Jos kokeilee viimetehtävän ratkaisua

![image](https://github.com/user-attachments/assets/4dccfeaf-4e0c-49e5-9781-6dc6467f57ff)

Lähdin hakemaan tietoa ja löysin sopivan tiedon OWASP sivuilta

![image](https://github.com/user-attachments/assets/f3afa4c3-0b70-4a88-b0e9-38dca92d8fd3), (OWASP, Path Traversal)

Kokeilin tätä ratkaisua, jossa `filename=/etc/passwd`

![image](https://github.com/user-attachments/assets/be458b71-f49e-4c4a-8634-86f7800cc443)

![image](https://github.com/user-attachments/assets/9fee6836-1159-4f55-87fe-92dc57c3014f)

----

#### traversal sequences stripped non-recursively

Edelleen haetaan passwd tiedostoa, nyt varoitetaan että pathtraversal sequences ei toimi suoraan

![image](https://github.com/user-attachments/assets/eea1aae9-aef5-4c44-b818-ac5caa8320a7)

Sivusto näyttää tältä

![image](https://github.com/user-attachments/assets/c9d9118e-561f-40e4-84e7-ca2439c72059)

Avataan pyyntö joka hakee kuvan requester välilehteen

![image](https://github.com/user-attachments/assets/27aa9031-ebfd-4fc9-ad4f-395e38a85a8d)

Aiemman tehtävän ratkaisu ei toimi

![image](https://github.com/user-attachments/assets/8d99360a-4c43-4078-b852-f6e2cc19efbb)

Ensimmäisenä ideana halusin kokeilla kirjoittaa pathtraversalin toisessa muodossa

![image](https://github.com/user-attachments/assets/9dff95ac-15f6-4917-8a25-247b595f24b8) (OWASP, Path Traversal)

Pyyntö on sama kuin `filename=/../../../etc/passwd`

![image](https://github.com/user-attachments/assets/4dab09db-e3b1-4797-93e5-90555142c77a)

Kokeilin montaa eri variaatiota tästä, mutta ei mitään. 

Tuli seinä vastaan ja katsoin malliratkaisun. 

Tehtävän nimi kertoi, että käyttäjän syöte filteröidään vain kerran (non-recursive). Ohjelma filteröi `../` ja `/` merkit pois, joten tavoite on, että filteröinnin jälkeen meillä olisi `/../../../etc/passwd`. 

Joten kokeillaan lähettää pyyntö jossa filename on `....//....//....//etc//passwd`, sillä `....//` > `../`. 

![image](https://github.com/user-attachments/assets/01032c8c-9fa1-41cf-ab42-46bf1e0259cc)

![image](https://github.com/user-attachments/assets/4b82d5f6-9061-4c9b-9566-c20d41cae905)

### Insecure Direct Object Reference (IDOR)

Eli pitä saada käyttäjän Carlos salasana ja kirjautua hänen käyttäjälleen. Sivusto hakee livechattien logeja staattisteun URLien kautta

![image](https://github.com/user-attachments/assets/49de14b7-dd88-463b-b3ca-3c5b36183906)

![image](https://github.com/user-attachments/assets/96d41048-1fc9-4a59-8171-c1f1ec9151ce)

![image](https://github.com/user-attachments/assets/f1c1456b-f372-4efa-9493-9e47a99e5c6a)

Näyttäisi siltä, että .txt tiedostojen nimeäminen menee järjestyksessä, joten kokeillaan hakea transcript/1.txt tiedosto.

Tämä on keskustelu, jossa paljastetaan jonkun salasana. 

![image](https://github.com/user-attachments/assets/d60da28d-5382-4707-9296-7920a8554e59)

![image](https://github.com/user-attachments/assets/3c5a6720-7f3a-4e14-9be2-aec46740f8e5)

![image](https://github.com/user-attachments/assets/4456e5c4-0621-4341-a2b7-011c65702c29)

### Server Side Request Forgery (SSRF)

Tässä tehtävässä pitää poistaa Carloksen käyttäjä admin paneelin kautta

![image](https://github.com/user-attachments/assets/004299a8-18f2-44f1-af6d-c2d5f773c1c3)

Tehtävänannossa kerrotaan, että stockcheck feature lähettää kutsun sisäverkkoon

![image](https://github.com/user-attachments/assets/418dac6d-2dcb-48d7-8895-ace5529a8447)

Stock check pyyntö näyttää tältä zapissa

![image](https://github.com/user-attachments/assets/eb0b121f-b763-4b14-92f0-6fdfea302629)

kiinnostavaa on `stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D3%26storeId%3D1` , joka on url

Käännetään se luettavampaan muotoon työkalun avulla

![image](https://github.com/user-attachments/assets/628221f7-7c47-4314-b2ce-98f213cc680c)

Eli pyyntö tehdään verkkoon `http://stock.weliketoshop.net:8080`. Yritetään päästä admin paneeliin muuttamalla stockApi arvo: `https://localhost/admin`

Saimme admin sivun auki

![image](https://github.com/user-attachments/assets/4f0ed112-35f8-4a1f-bcc3-9bff8a3238d1)

Kun katsoo sivua tarkemmin, näkyy että siellä on delete endpointti josta voi poistaa käyttäjiä

![image](https://github.com/user-attachments/assets/28c09509-5aaf-4897-a663-e514a62a5c69)

Lähetetään uusi pyyntö osoitteeseen `https://localhost/admin/delete?username=carlos`

![image](https://github.com/user-attachments/assets/3346e751-92cd-4d7e-8cc1-3e1a20b81b69)

![image](https://github.com/user-attachments/assets/61e62a0c-9d3d-44ca-98c9-76fcb75839fb)

--- 

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa https://terokarvinen.com/tunkeutumistestaus/, luettu 4.4.2025

OWASP Top 10: 2021, Broken Access Control, luettavissa https://owasp.org/Top10/A01_2021-Broken_Access_Control/, luettu 4.4.2025

OWASP Top 10: 2021, Server Side Request Forgery (SSRF), luettavissa https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/, luettu 4.4.2025

PortSwigger, Web Security Academy, Insecure Direct Object Reference, luettavissa https://portswigger.net/web-security/access-control/idor, luettu 4.4.2025

PortSwigger, Web Security Academy, Path Traversal, luettavissa https://portswigger.net/web-security/file-path-traversal, luettu 4.4.2025

PortSwigger, Web Security Academy, Server Side Request Forgery, luettavissa https://portswigger.net/web-security/ssrf, luettu 4.4.2025

PortSwigger, Web Security Academy, Cross-site Scripting, luettavissa: https://portswigger.net/web-security/cross-site-scripting, luettu 4.4.2025

FoxyProxy, URL Patterns, luettavissa https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/, luettu 4.4.2025

OWASP, Path traversal, luettavissa https://owasp.org/www-community/attacks/Path_Traversal, luettu 5.4.2025

URL Decoder, luettavissa https://www.urldecoder.org/, luettu 5.4.2025






