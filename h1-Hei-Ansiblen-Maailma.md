# Hei Ansiblen Maailma

## x)Lue ja tiivistä
Artikkelissa "Karvinen 2026: SSH public key - Login without password" käydään läpi mikä on SSH avain ja miten sellainen saadaan luotua. Siinä kerrotaan myös miten sen saa liitettyä omiin tiedostoihin ja miten sitä käytetään. SSH avain on kätevä tapa pitää huolta tietoturvasta ja samalla helpottaa elämää, kun ei tarvitse joka kerta näpytellä salasanaansa. 
Komentoja:  

_- $ sudo apt-get update_ (päivittää koneen uusimpaan käyttöjärjestelmään)  

_- $ sudo apt-get -y install ssh_ (lataa avoimenSSH-palvelin paketin)  

_- $ sudo systemctl enable --now ssh_ (aloittaa SSH demonin ja myöskin pistää sen käynnistymään koneen käynnistyessä)  

_- $ ssh localhost_ (luo ssh yhteyden localhostiin)  

_- $ ssh-keygen_ (generoi SSH avainparin)  

Käyn paremmin läpi miten tämä tehdään kohdassa b) Asenna SSH demoni

Artikkelissa "Karvinen 2026: Hello Ansible" käydään läpi miten asentaa ansible ja miten siellä tehdään kansioita ja rooleja. Kohdassa *d* seurasin tämän artikkelin ohjeita.
Artikkelissa näytetään, miten rooleja jaetaan siellä ja miten luodaan käyttäjiä.




## a) Asenna debian
Asensin debianin harjoituksessa h0 jonka näkee linkistä.
https://github.com/HelinRoni/Palvelinten-Halllinta-kurssi/blob/main/h0-oikeasti-netissä.md

## b)Asenna SSH demoni
### 1.
Ensiksi piti asentaa OpenSSH-serveri paketti seuraavilla komennoilla:
  _- $ sudo apt-get update_
  _- $ sudo apt-get -y install ssh_

<img width="1240" height="512" alt="demoni1" src="https://github.com/user-attachments/assets/ee3d1a24-fa2c-4d07-957d-cb0969a4b2e5" />


<img width="831" height="145" alt="image" src="https://github.com/user-attachments/assets/2e925a8b-dcbc-4653-87dd-23698684c7f3" />  

### 2.
Sitten määrittelin, että ssh aukeaa automaattisesti tietokoneen käynnistyessä komennolla:
 _- $ sudo systemctl enable --now ssh_

<img width="1238" height="67" alt="image" src="https://github.com/user-attachments/assets/e38dc11c-6593-4d45-a65b-b6cb936f9abb" />  

### 3.
Tämän jälkeen testasin ssh toimivuuden komennolla:
  _- $ ssh localhost_


<img width="1347" height="202" alt="image" src="https://github.com/user-attachments/assets/04e33160-9cbf-4db3-9527-1e6e87326b57" />  



## c)Pubkey. Automotivoi sis

Automatisoin sisäänkirjautumisen pubkeyllä, jotta minun ei tarvitse kirjoittaa salasanaa kun kirjaudun sisään sudokäyttäjältä.

<img width="1366" height="227" alt="image" src="https://github.com/user-attachments/assets/74499b65-af49-46bb-a2ef-68bf064e2e53" />


## d) Tee hei maailma ansiblella ja testaa sitä localhostin yli
### 1.
Ensiksi latasin ansiblen virtuaali palvelimelleni käyttäen komentoja:
  _- $ sudo apt-get update_
  _- $ sudo apt-get install ansible micro bash-completion tree_
    
<img width="1163" height="167" alt="image" src="https://github.com/user-attachments/assets/584dd2e5-0d75-4635-b9c3-1df0c0050427" />  

### 2.
Sitten tein kansion ansible määrityksille seuraavilla komennoilla:
  _- $ cd_
  _- $ mkdir ansible/_
  _- $ cd ansible/_

Tämän jälkeen tein listan ohjattavista host käyttäjistä komennoilla:  

  _- $ micro hosts.ini (Micro komento antaa kirjoittaa pieniä komentoja ilman että kirjoitan koko ansible kirjaa)_  
  
  _- $ cat hosts.ini (cat komennot tulostaa komennot mitä kirjoitin tiedoston sisälle) Tässä tapauksessa tuloste oli localhost_

<img width="772" height="162" alt="image" src="https://github.com/user-attachments/assets/3fd5d3d7-859c-4e36-8982-0556ceefa200" />  

### 3.
Sitten ajoin komennon kaikille host käyttäjilleni ansiblessa jotta se näyttää uptimen ja käyttäjien määrän.

Komennolla:
  _- $ ansible all -a 'uptime' -i hosts.ini¨_

<img width="1461" height="165" alt="image" src="https://github.com/user-attachments/assets/53e10dbc-d4cc-45e8-b53e-26a15e0a9a49" />

### 4.
Poistin turhat vartoitus ilmotukset syötteestä lisäämällä hosts.ini tiedostoon *[all:vars]
ansible_python_interpreter=/usr/bin/python3*
  

<img width="1002" height="211" alt="image" src="https://github.com/user-attachments/assets/f362f739-ba61-4d3b-bb71-6ddc0c8dac41" />  

### 5.
Kätevyyden nimissä muokkasin ansible.cfg tiedostoa siten, että kun haluan tulostaa uptimeni niin ei tarvitse erikseen määritellä hosts.iniä vaan se käyttää sitä suoraan.
Tein tämän lisäämällä ansible.cfg säännön [defaults]
inventory = hosts.ini


<img width="890" height="211" alt="image" src="https://github.com/user-attachments/assets/3e14d15c-e74e-4e30-b968-5ca2b4a21dc6" />

### 6.
Sitten lisäsin ensimmäisen roolin joka tulostaa syötettä ulos joka näkyy muillekkin koneille. Tein tämän komennoilla:  

  _- $ mkdir -p roles/hello/tasks/_ (Loin tällä uuden kansion)  
  
  _- $ micro roles/hello/tasks/main.yml_ (Tässä syötin kansioon sääntöjä)  
  

Syötin kansioon seuraavat säännöt/ohjeistukset:
- copy:
    dest: /tmp/hello-ansible
    content: "See you at TeroKarvinen.com!\n"


<img width="927" height="160" alt="image" src="https://github.com/user-attachments/assets/93ed4c88-dfe2-44cc-a895-88a9abf03f22" />

### 7.
Ajoin minun ansible playbookin komennolla:
  _- $ ansible-playbook site.yml_


<img width="1457" height="318" alt="image" src="https://github.com/user-attachments/assets/1205926c-9eab-4559-b5aa-db17fcb4a102" />  

### 8.
Kokeilin yhteyttä ssh;n kautta. Koneeni sai yhteyden ansiblen sivulleni. Käytin komentoa:
  _- $ ssh localhost 'cat /tmp/hello-ansible'_


<img width="910" height="47" alt="image" src="https://github.com/user-attachments/assets/fd9e868a-0a34-4e68-8d9f-7e06ff880833" />


Ansible palvelimeeni saa yhteyden muiltakin koneilta kuten ssh. Tehtävä tuli tehtyä.
Tein tehtävän tehtävänannon mukaan ja käyttäen materiaaleja sivustolta Terokarvinen.com

## Lähteet:
### SSH public key - login without password
https://terokarvinen.com/ssh-public-key-login-without-password/
### Hello Ansible
https://terokarvinen.com/hello-ansible/










