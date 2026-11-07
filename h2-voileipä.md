# Voileipä  

x) Lue ja tiivistä


## a) Sudoless  

Loin sudokäyttäjällä uuden käyttäjän nimeltä "Roni" ja loin myös uuden ryhmän nimeltä "sudoless" ja siirsin käyttäjän roni ryhmään sudoless.  
Käytin tähän komentoja:  
  - _$ sudo adduser "roni"_ *(Tällä komennolla loin uuden sudokäyttäjän)*
  - _$ sudo addgroup "sudoless"_ *(Tällä komennolla loin uuden ryhmän)*
  - _$ sudo adduser "roni" "sudoless"_ *(Tällä komennolla lisäsin käyttäjän roni ryhmään sudoless)*

<img width="811" height="111" alt="image" src="https://github.com/user-attachments/assets/8d70b1bb-7ce0-4e7a-be92-e71580142539" />  

Poistuin sudokäyttäjältä takaisin root käyttäjälle siksi aikaa kun teen muutoksia sudokäyttäjiin.  

  -_$ sudo -i_ (komento siirtää sinut pois sudokäyttäjältä takaisin root käyyäjälle)  
  
  

<img width="592" height="70" alt="image" src="https://github.com/user-attachments/assets/5004bb2a-26ff-4506-9de9-6489bdd5980e" />  

Sitten syötin säännön joka antaa sudoless ryhmälle oikeudet kirjautua sisään ilman salasanaa.  
Komennot jota käytin:  
  -_$ sudo visudo /etc/sudoers.d/sudoless_ (Komennolla menin editoriin sisään jotta pystyn syöttämään uusia sääntöjä)
  -_$ %sudoless ALL = (ALL) NOPASSWD: ALL_ (Sääntö joka syötettään editorin sisälle jotta pystyn kirautumaan sudon sisälle ilman salasanoja)

<img width="472" height="80" alt="image" src="https://github.com/user-attachments/assets/ea1d3187-0da5-49c7-b39b-4fe09769a1b6" />


