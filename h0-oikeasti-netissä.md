# h0 Oikeasti Netissä 

x) Lue ja tiivistä
- Artikkelissa Teoriasta käytäntöön pilvipalvelimen avulla (h4) käydään läpi verkko palvelimen konfiguroinnista.
- Virtuaalipalvelin pohjaksi valittiin upcloud.
- Operating sytemiksi valittiin Debian GNU/linux 13 (Trixie), koska se on kurssilla käytettävä käyttöjärjestelmä.
- Virtuaali palvelimeen saa yhteyden ssh yhteydellä.
- Yhteyden saa komennolla ssh root@(palvelimen ip osoite), elikkä root + @ + palvelimen ip osoite.
- Artikkelissa käydään läpi myös miten käyttäjä luodaan koti hakemistoon.
- Sitten käydään palomuurin asennus läpi
  Huomio ensiksi pitäää käyttää komentoa apt-get update ja sitten apt-get install uwf.

- Artikkelissa Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi. Käydään läpi miten hankitaan julkinen domanin nimi namecheapin kautta.
- Kassalla otetaan vielä domain privacy suojaus päälle ilmaiseksi.
- Laitetaan osoite osoittamaan omaan koneeseen.
  Huomio valitaan aleksipartanen.space ja advanced DNS. HOST RECORDS osoiosta nähdään että siinä on turha CNAME tietue.


  a) Asenna Debian 13 virtuaalikoneellesi
  <img width="623" height="142" alt="Tehtävä h0a" src="https://github.com/user-attachments/assets/57a4daf5-2b57-48d8-b105-587878447865" />
  kuvan nimestä näkee että, Debian 13 on ladattu. Sekä, että olen vuokrannut virtuaalipalvelimen.

  c) asensin palomuurin ja loin sudo käyttäjän
  <img width="487" height="63" alt="palomuuri1" src="https://github.com/user-attachments/assets/e9a93d5b-1df9-4cb9-8ae5-2a1e80dddce5" />

 d.) asensin apache webbipalvelimen käytin komentoja sudo apt install apache2 -y, sudo apt update.
 <img width="1313" height="881" alt="apache kuva" src="https://github.com/user-attachments/assets/e0e319ef-c418-4334-8bfc-bdb48050520a" />

 

  
