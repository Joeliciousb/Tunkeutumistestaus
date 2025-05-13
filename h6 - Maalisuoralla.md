# Maalisuoralla

## Deviant Ollam - This Scam Suprised a Family Friend... Why Wasn't Their PC Protected?
Deviant Ollamin perhetuttua yritettiin huijata hyvin yksinkertaisella Microsoft-huijauksella. Kyseisessä huijauksessa uhri pyritään ohjaamaan klikkaamaan linkkejä, antamaan henkilökohtaisia tietoja tai jopa lähettämään rahaa esiintymällä Microsoftin nimissä.

Sivusto asetti selaimen koko näytön tilaan, soitti kovalla äänellä hälytysääntä ja väitti, että tietokone on lukittu turvallisuussyistä. Se kehotti käyttäjää soittamaan Microsoftin tukipuhelimeen, mutta oikeasti uhri olisi soittanut huijareille. Onneksi perhetuttu ei soittanut huijausnumeroon, vaan otti yhteyttä Deviant Ollamiin, joka sai tilanteen korjattua.

Videon pointtina on, että kaikkien tulisi käyttää uBlockia tai muuta adblockeria. Verkkosivujen ei pitäisi pystyä muuttamaan selainikkunaa näin rajusti tai toistamaan ääniä ilman käyttäjän lupaa.

## Lippuvalmistelu

Aion käyttää Kali Linuxia virtuaalikoneella. Netin saan tarvittaessa irti. En aio käyttää paikallista tekoälyä. 

## Oma korkki

Valitsin korkata tämän CTF

![image](https://github.com/user-attachments/assets/7db691dc-ecd7-40aa-9381-1ec859f8f9b6)

![image](https://github.com/user-attachments/assets/45b7b130-affc-41a2-8214-97f3153e46ed)

![image](https://github.com/user-attachments/assets/572bc4ec-c477-466f-8ebf-f4134638f5c9)

![image](https://github.com/user-attachments/assets/b8b63ded-2cfc-430c-89ad-9769c3413594)

Näyttäisi silta, että lippu on XORattu avaimella `chr(0x34) + chr(0x65) + chr(0x63) + chr(0x39)`.

![image](https://github.com/user-attachments/assets/45cf331b-5e92-43b8-a7ca-509db89c528a)
![image](https://github.com/user-attachments/assets/9f070607-d8ef-4178-8103-36bc6e02d8e7)
![image](https://github.com/user-attachments/assets/a8741471-8c96-4645-8569-d1efc4e3a7ec)
![image](https://github.com/user-attachments/assets/9d1c8c5b-68dd-4c3e-98e6-153009548efa)

ASCII taulukosta voidaan katsoa, että nuo hex arvot muodostavat `4ec9`

![image](https://github.com/user-attachments/assets/8d9dd56b-1a32-471f-9d92-a2656a0de74d)

![image](https://github.com/user-attachments/assets/21ac9019-ef0a-45b0-b979-824e5d4698f2)

## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa https://terokarvinen.com/tunkeutumistestaus/, luettu 13.5.2025

ascii-code.com, ASCII Table, luettavissa https://www.ascii-code.com/, luettu 13.5.2025

YouTube, DeviantOllam, This Scam Surprised a Family Friend... Why Wasn't Their PC Protected?, katsottavissa https://www.youtube.com/watch?v=3ljeOgMksMg, katsottu 13.5.2025

