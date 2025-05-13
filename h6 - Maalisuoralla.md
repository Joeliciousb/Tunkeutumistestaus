# Maalisuoralla

## Deviant Ollam - 

## Lippuvalmistelu

Aion käyttää Kali Linuxia virtuaalikoneella. Netin saan tarvittaessa irti. En aio käyttää paikallista tekoälyä. 

## Oma korkki

Valitsin korkata tämän harjoituksen

![image](https://github.com/user-attachments/assets/286e119c-1e92-462f-a4aa-49f578b0211a)

### Task 1 

![image](https://github.com/user-attachments/assets/6465169b-ff8b-4924-8289-d35a2ad314cf)

Ajetaan porttiskanni komennolla `nmap 10.129.24.252`

![image](https://github.com/user-attachments/assets/905ade6d-90ab-4f14-89cd-a78659bcc2cf)

Nähdään kaksi avointa tcp porttia, 22 & 80. 

![image](https://github.com/user-attachments/assets/63105442-73b6-497b-9328-131a17b961ae)

### Task 2

![image](https://github.com/user-attachments/assets/7549e0ef-fa52-43ce-b991-5c04115914be)

Mennään katsomaan miltä sivusto näyttää

![image](https://github.com/user-attachments/assets/01e9745a-e95c-4ff3-acd7-41e915753590)

![image](https://github.com/user-attachments/assets/186ab62a-b5cb-4c9e-8f48-59d0e74515f8)

![image](https://github.com/user-attachments/assets/8ae4c308-01a6-4b4c-800d-b9259ed87fc9)

### Task 3 

![image](https://github.com/user-attachments/assets/fc352216-2e78-4ca5-a750-a3a6530f36fe)

En tiedä, joten googletan `linux hostname resolution` ja löydän sopivan artikkelin[^1] jossa mainitaan `etc/hosts`

![image](https://github.com/user-attachments/assets/8256cfe3-2a1d-424f-a1ea-b8b313de2760)

![image](https://github.com/user-attachments/assets/27fba477-433d-4c42-878d-a9810341fdd5)

### Task 4

![image](https://github.com/user-attachments/assets/9febce97-003f-48b0-9ca6-989fd76806f9)

Kokeillaan `ffuf` työkalua. 

![image](https://github.com/user-attachments/assets/e0234d94-59fd-4566-a348-9215707d9b9f)


## Lähteet

Karvinen Tero, Tunkeutumistestaus, luettavissa https://terokarvinen.com/tunkeutumistestaus/, luettu 13.5.2025

[^1]: Medium, Yuminlee2, Linux Networking: DNS, luettavissa https://yuminlee2.medium.com/linux-networking-dns-7ff534113f7d, luettu 13.5.2025
