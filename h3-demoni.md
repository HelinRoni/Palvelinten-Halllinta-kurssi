  # Demoni   
## x) Lue ja tiivistä  
Artikkeli Karvinen 2026: Apache installed with Ansible - quick notes.  
  - Yksinkertaistettu ansible malli. Malli paketti-tiedosto-palvelin kuviosta. Muistiinpanot vain, ei tutoriaali.
  - Apache2 automaattinen lataus. Sivuja pystyy muokkaamaan normaalina käyttäjänä ilman sudoa taikka roottia.
  - Demonit niinkuin apache2 web server on jonkinverran samankaltaisia.
  - Lataa vain demoni, muokkaa määritykset ja aloita se.
    
Artikkeli Ansible Community Documentation: Handlers: running operations on change.
  - Handlereita suoritetaan vain, kun jokin tehtävä ilmoittaa niistä notify-komennolla.
  - Niitä käytetään esimerkiksi palvelun uudelleenkäynnistämiseen asetustiedoston muuttuessa.
  - Handler suoritetaan yleensä vasta, kun kaikki kyseisen vaiheen tehtävät ovat valmistuneet.
  - Sama handler suoritetaan vain kerran, vaikka useampi tehtävä ilmoittaisi siitä.
    

## Y) Etsi artikkeli keskeisestä hallinnasta.  

Linkki artikkeliin: https://news.ycombinator.com/item?id=16445612  
  - Cockpit on työkalu, jolla linux-palvelimia voi hallita selaimen kautta.
  - Keskustelussa nostetaan esiin erityisesti helppokäyttöisyys ja palvelinten hallinnan yksinkertaistaminen.
  - Sen avulla järjestelmän ylläpitäjä voi seurata ja hallita palvelimia keskitetysti ilman, että jokaiseen palvelimeen tarvitsee kirjautua erikseen
    komentorivillä.
  - Hacker Newsin keskustelussa pohdittiin myös tietoturvaa, erityisesti SSH-tunnistautumista ja salasanojen turvallisuutta.
Huomioni. Cockpit;in idea on samanlainen kuin ansiblen. Keskitetyn hallinnan ideana. Niillä helppo pitää useita palvelimia kasassa ja ajantasalla yhtäaikaisesti.


## a) Apassi. Asenna apache 2 

Kun alan asentamaan apache 2 pakettia. Ensiksi Päivitän järjestelmäni jonka jälkeen voin asentaa apache 2. Käytin tähän komentoja:  
  -_$ sudo apt update_ (päivittää järjestelmän)  
  -_$ sudo apt install apache2_ (Lataa apache2 koneelle)  
Kerkesin jo tehdä tämän kohdan ja unohdin tallentaa niin havainnollistavat kuvat jäävät uupumaan...  
Kun olen saanut apache2 palvelimelleni, rupean muokkaamaan ansible puutani. Loin hakemiston apache2 ja sen sisälle kolme hakemistoa nimiltä; files, handlers ja tasks.  
Sain luotua hakemistot komennolla:  
-_$ mkdir "apache2"_ (Elikkä mkdir (make directory) + hakemiston nimi)  
Kun sain luotua hakemistot loin hakemistojen sisälle erinimisiä tiedostoja joiden sisälle voin muokata sääntöjä ansible playbookkiin. Loin handlers ja tasks hakemistojen sisälle tiedostot "main.yml".  
Files tiedoston sisälle loin "example.com.conf" jonka sisälle määrittelin, että näitä sivuja pystyy editoimaan kukatahansa ilman root taikka sudo oikeuksia.  
Tiedostoja luodessani käytin komentoa:  
  -_$ nano "main.yml" (elikkä nano + tiedoston nimi ja pääte) Nano itsessään luo tämän tiedoston, mutta sitä pystyy myöskin käyttää jos haluaa editoida jo olemassa olevia tiedostoja.  
  
<img width="787" height="160" alt="image" src="https://github.com/user-attachments/assets/6ad08c90-a267-4f7c-8ec0-e321c6052c70" />      

Alla olevista kuvista näkyy, että apache2 on saatu ladattua onnistuneesti ja on myöskin näkyvillä etusiviulla.  


<img width="1110" height="355" alt="image" src="https://github.com/user-attachments/assets/867edf35-b22e-4923-8bd0-eefcb2ccd8e9" />  
<img width="1917" height="925" alt="image" src="https://github.com/user-attachments/assets/24d096cf-7d94-4a88-92e3-ba8c3421ad43" />

## b) Moottorix. Asenna nginx.  

Ensiksi kun lähdin asentamaan nginx sammutamme apache2 käytöstä komennolla:  
  -_$ sudo systemctl stop apache2_ (komento pistää apache2 pois käytöstä palvelimella kunnes se käynnistetään uudelleen)  
  -_$ sudo systemctl disable apache2_ (komennolla estetään apache2 automaattinen käynnistyminen)  
Tämän jälkeen tarkistan vielä apache2 tilanteen, että menihän se oikeasti pois päältä nyt. Tämän tarkistan komennolla:  
  -_$ sudo systemctl status apache2_ (komento tarkistaa onko apache2 käynnissä vai ei)  
  
<img width="1462" height="387" alt="image" src="https://github.com/user-attachments/assets/1d7a4452-6f35-416a-9d55-24b9c0f0467d" />        

Tämän jälkeen kun apache2 on pois päältä valmistaudun lataamaan nginx. Käytän komentoja:  
  -_$ sudo apt udpate_ (komento päivittää järjestelmän)  
  
  -_$ sudo apt install nginx_ (Komento lataa paketin)  
Kun pakettin on ladannut teen samanlaisen puu rakenteen tähän kuin tehtävässä a.  
Käytin komentoja:  
  -_$ mkdir nginx_ 
  -_$ nano main.yml_  
Tämän jälkeen loin samanlaiset tiedostot puunsisälle ja annoin samat määritykset kuin tehtävässä a. Tehtävän a ja b verkkosivu määritykset katsoin mallia Tero Karvisen artikkelista "Apache installed with Ansible - quick notes".  
Linkki kyseiseen artikkeliin: https://terokarvinen.com/apache-ansible/    
Tämän jälkeen tarkistin vielä toimiiko nginx kunnolla laittamalla verkkoselaimeen ip osoitteen.  

## c) Automoottorix. Automatisoi Nginx asennus ansiblella.  

Kun lähden muokkaamaan Nginx;in asennusta. Teen melkein kaikki muutokset vain (b) kohdan hakemistoon ja tiedostojen sisälle.  
Menin nginx/tasks/main.yml ja lisäsin tälläisen pätkän tekstiä sinne. Määritykset tarkistaa, että onko nginx asennettu ja jos ei ole niin asentaa, määriteliin tiedoston omistaja sekä käyttöoikeudet, poistin alkuperäisen nginx sivuston tieltä häiritsemästä jottei se vaikuta tähän ja varmistaa, että nginx käynnistyy itse tietokoneen käynnistäessä.  

<img width="656" height="623" alt="image" src="https://github.com/user-attachments/assets/63488cc0-9534-4fb1-bc6d-cade573a60a2" />  

Sitten määrittelin mistä sivuni löytää ja sen asetukset. Tein nämä muutokset nginx/files/example.com.conf sisälle.  


<img width="448" height="257" alt="image" src="https://github.com/user-attachments/assets/0c18b92d-efd2-48d1-b2f7-fbbaf9c3ac44" />     

Sitten vain testailin toimiko automoottorix. Katsoin toimiiko se palvelimen sisällä komennolla:  
  -_$ curl http://localhost_ (Näyttää miltä localhost html näyttää)  
  
Ja katsoin myös omassa selaimessa toimiiko nginx ansennus ja automatio niinkuin kuuluu. Laitoin selaimeen http://(ip osoitteeni) ja sivulle pompahti se mitä kuulluukin.


<img width="812" height="222" alt="image" src="https://github.com/user-attachments/assets/d8e23736-2904-4b94-a163-27dd4a48e091" />  
<img width="322" height="172" alt="image" src="https://github.com/user-attachments/assets/295a1f37-e654-4cce-8343-69e9527cd4b1" />   

# Lähteet:    
Hacker news:  

https://news.ycombinator.com/    
Karvinen 2026:  

https://terokarvinen.com/apache-ansible/    
Ansible Community Documentation:  

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html   

https://chatgpt.com/  












