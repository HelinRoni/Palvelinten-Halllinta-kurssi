# Voileipä  

## x) Lue ja tiivistä
https://terokarvinen.com/passwordless-sudo-with-ansible/ Passwordless Sudo with Ansible. Karvinen 2026 
  - ohjeet saada sudo salasanattomaksi ansiblen avulla
  - Määritykset playbookkiin jotta sudon saa salasanattomaksi
    
https://xkcd.com/149/ Munroe 2006.
  - Selittää logiiggaa miten ajaa ansiblea
  - kun lisää sudon ennen komentoa silloin ohjelma ajetaan kyselemättä
    
https://terokarvinen.com/passwordless-sudo/ Sudo without password. Karvinen 2026.
  - cat /etc/sudoers.d/sudoless
%sudoless ALL = (ALL) NOPASSWD: ALL  (Olennaisin tieto artikkelista. Määritys jolla saadaan sudo salasanattomaksi)
  - Määritellään sudo ilman salasanaa
    
  

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

 
## b) Antero  
Kun tein salasanatonta automaattisesti ssh;lla kirjautuvaa tunnusta ansiblella. Ensiksi minun piti katsoa, että ansible puuni rakenne oli oikean näköinen.  

<img width="416" height="362" alt="image" src="https://github.com/user-attachments/assets/54cb055f-aee1-4112-a70a-a0cda7219450" />  

Puusta piti löytyä tärkeimmät elikkä "roles" directory ja sen alta löytyvät directoryt. Joten loin ne.  
Sain luotua "roni" directoryn "roles" sisään. Sitten loin "tasks" directoryn "roni" sisään. Viimeiseksi loin "main.yml" directoryn "tasks" sisälle.  
Käytin komentoja:  
  -_$ mkdir "--"_ (Tällä luodaan directory)  
  
  -_$ nano "main.yml"_ (Tällä luodaan directory jonka sisälle pystyy kirjoittamaan sääntöjä)  

  Kun olin saanut puuni näyttämään oikealta, lisäsin säännöt main.yml sisälle. Nämä säännöt antavat minun kirjautua ilman salasanaa käyttäen ssh avaintani.  
  
  <img width="748" height="462" alt="image" src="https://github.com/user-attachments/assets/cc578fe6-93b3-410d-bad0-71923dbd287a" />  

  Sain säännöt kirjoitettua main.yml sisään kun käytin komentoa:  
  -_$ nano main.yml_   
  joka loi tämän directoryn. Se samalla vei minut suoraan kansion sisälle jossa pystyin tehdä muutoksia. Käytin teron mallia                   https://terokarvinen.com/passwordless-sudo-with-ansible/ passwordles sudo with ansible. Mallista muokkasin vain tiedot vastaamaan omiani. Elikkä kopioin          ssh avaimeni sääntöihin ja myös käyttäjäni jonka haluan toimivan ssh avaimella.  

Tämän jälkeen minun piti vielä käydä lisäämässä vielä arvo "true" site.yml sisälle. Tämän tein komennoilla:  
  -_$ micro site.yml_ (Komentti antaa tehdä pieniä sääntö muutoksia kansion sisälle ilman, että kirjoitan koko ansible playbookin kerralla)  
  -_ctrl + s_ (Editorin sisällä jotta tekemäni muutokset tallentuvat playbookin sisälle  
  -_ctrl + q_ (Editorin sisällä jotta pääsen pois editorista, kun olen lisännyt haluamani säännöt)  

  <img width="205" height="120" alt="image" src="https://github.com/user-attachments/assets/05ced9b3-b935-45de-8135-84c5a8da0988" />  

  ## c) asenna kaksi pakettia ansiblella  

  Kun asentaa paketteja ansiblella, se tulee tehdä muokkaamalla playbookin sääntöjä esimerkiksi main.yml sisällä. Kuvasta näkee mitä sääntöjä käytin jotta sain 
  asennettua paketit.   
  Kuva selitettynä:  
  *ansible.builtin.apt*: .apt käyttää debianin/ubuntun paketin hallintaa  
  *name:* Kertoo asennettavan paketin  
  *state: present:* Varmistaa, että paketti on asennettu  
  *become: true:* suorittaa tehtävän pääkäyttäjän oikeuksilla  
  
<img width="328" height="282" alt="image" src="https://github.com/user-attachments/assets/99ea6471-64a5-4d1b-8a26-9f9b4ff4071a" />  

Sitten pitää ajaa komento:  
  -_$ ansible-playbook site.yml --ask-become-pass_ (Komento ajaa playbookin)  
  Kuvasta näkyy, että changes on 2. Tämä tarkoittaa, että kirjoittamani lataus säännöt ovat päivittyneet oikein sinne ja toimivat kuin kuuluu.  
  
<img width="1113" height="285" alt="image" src="https://github.com/user-attachments/assets/acbe36cd-cc80-4a3e-be69-bdec2e3b2a19" />  

Komennolla:
  -_$"git" --version  
  Saan tarkastettua gitin version. Git on toinen paketeista jonka asensin. Ajoin myös komennon curlilla jolloin tulostus näytti myös senkin. Tämä tarkoittaa, että paketit on asennettu onnistuneesti ansiblea käyttäen.  
  
<img width="1457" height="243" alt="image" src="https://github.com/user-attachments/assets/c12122cb-c122-4b9a-af67-df71ac61042c" />  

## d) Files  

Kun lähdin luomaan uutta tiedostoa ansiblella niin aloitin sen menemällä main.yml directoryn sisälle muokkaamaan playbookkia.  
Sääntöjen/määritysten avaus:  
  -copy: luo taikka kopioi tiedoston  
  
  -dest: määrittää sijainnin tiedostolle  
  
  -content: kirjoittaa sisällön tiedostoon  
  
  -owner: määrittää tiedoston omistajan  
  
  -group: määrittää ryhmän joka omistaa tiedoston  
  
  -mode: "0600" määrittää käyttöoikeudet.  
  
  "0600" kertoo, että omistajalla on luku sekä kirjoitus oikeudet.  
  Symbolisessa muodossa 0600 olisi -rw------- joka kertoo että omistajalla on luku sekä kirjoitus oikeudet kun taas ryhmällä taikka muilla ei ole oikeuksia.  
  Symbolit tulevat linux logiigasta RWX. R = 4 = Reading, W = 2 = Writing, X = 1 = Executing.
  

<img width="651" height="266" alt="image" src="https://github.com/user-attachments/assets/eb458dc7-480e-4281-904e-044eccf30338" />  

Sitten ajoin komennon:  
  -_$ ansible-playbook site.yml --ask-become-pass_ (Tämä komento käskee suorittamaan playbookin kohteesta site.yml) (--ask-become-pass_ lisää siihen että kysyy sudosalasanaa tarvittaessa)

<img width="1397" height="202" alt="image" src="https://github.com/user-attachments/assets/a961161f-808f-4a69-a56d-91033871abe3" />  

Sitten tarkistin löytääkö luomaani tiedostoa myöskin ssh yhteyden kautta. Käytin komentoa:  
  - ls -l /home/ronihelin/testfile.txt
    
Kuvasta selviää, että ssh yhteyden kautta luomani tiedosto löytyy joka tarkoittaa, että sen tekeminen onnistui oikein. Kuvasta näkee myös tiedoston käyttöoikeudet.

<img width="948" height="47" alt="image" src="https://github.com/user-attachments/assets/c0b27259-1520-4b87-84e2-51d9c59d8ce2" />  

## e) Näytä esimerkki ansbilen käskystä  

Komennolla: _ansible-inventory --graph_  
Saa tulostettua graafisen kuvan missä ansible sijaitsee. Minun kuvassa näkyy, että se sijaisee local hostilla
<img width="897" height="87" alt="image" src="https://github.com/user-attachments/assets/9ce03444-c5af-4e72-86aa-bd79c51c9781" />  


# Lähteet
Tehtävän e) komento:  
https://www.geeksforgeeks.org/devops/basic-ansible-commands/  
https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa  
https://terokarvinen.com/passwordless-sudo/  
https://xkcd.com/149/  
https://terokarvinen.com/passwordless-sudo-with-ansible/  
https://chatgpt.com/  
Tekoälyä käytetty ongelmanratkaisuun.







    



