# h2 Karjaa
## x) Lue ja tiivistä.

<b>Slater 2017: [What is the definition of "cattle not pets"?](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654)</b>
- Biasin määritelmän mukaan lemmikit ovat palvelimia tai palvelinpareja, joilla ei ole varaa olla alhaalla. Usein manuaalisesti rakennettuja.
- Karjalla Bias taas viittaa palvelimeen tai palvelinpariin, jotka ovat rakennettu automatiikalla ja luotu kestämään useammankin palvelimen alhaallaolon. Tyypillisesti vikatilanteessa ei tarvita ihmisen toimia, vaan automatiikka korjaa tilanteen.
- Karjaa voi siis vikatilanteessa korvata uudella, lemmikkiä ei. 

<b>Karvinen 2017: [Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/)</b>
- Vagrant asentaa uuden virtuaalikoneen automaattisesti, jonka jälkeen siihen on mahdollista luoda SSH-yhteys.
- Vagrantin ja virtuaalikoneen asennus: ``$ sudo apt-get -y install vagrant virtualbox``
- Uuden koneen alustaminen: ``$ vagrant init bento/palvelin``
- Vagrantin käynnistys: ``$ vagrant up``
- SSH-yhteyden luominen: ``$ vagrant ssh``
- Virtuaalikoneen ja sen tiedostojen tuhoaminen komennolla ``$ vagrant destroy``
  
<b>Karvinen 2023: [Salt Vagrant - automatically provision one master and two slaves](https://terokarvinen.com/2023/salt-vagrant/)</b>
- Artikkelissa käsitellään kolmen virtuaalikoneen luomista valmiissa verkossa Saltin avulla.
- Koneiden asennuksen ja käynnistyksen jälkeen kirjaudutaan herra-koneelle ja hyväksytään orja-koneiden lähettämät avaimet.
- Orja-koneista kerätään tietoa  (``$ sudo salt '*' grains.items``)
- Tavoitteena idempotentti. Artikkelissa esimerkkejä komennoista
- Infra as code, artikkelin esimerkkikomennot sls. tiedostoon.
- Lopussa ohje koneiden tuhoamiseen (``$ vagrant destroy``)
  
## a) Asenna Vagrant
Asensin Vagrantista Windowsin AMD64 2.4.0-version [tästä](https://developer.hashicorp.com/vagrant/downloads) linkistä. Käytössäni on Acer Swift 3-kannettava, jonka käyttöjärjestelmänä toimii Windows 11 HOME versio 22H2.
Asennus käynnistyi käyttöehtojen hyväksymisen jälkeen. Asennuksessa kesti muutamia minuutteja, jonka jälkeen suoritin ohjelman suositteleman järjestelmän uudelleenkäynnistyksen.

<img width="384" alt="Näyttökuva 2023-11-06 004817" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7bef90a7-e408-4daf-a459-0d6f396893ce">
<br>
<img width="281" alt="Näyttökuva 2023-11-06 004832" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/28a32146-5b5a-4fd6-b05c-a1fb9cf5b042">

## b) Yksi maankiertäjä.
Asensin Vagrantin omalle kannettavalleni, joten aloitin tehtävän avaamalla Windowsin komentokehotteen ja tarkistamalla, että Vagrant oli asentunut komennolla ``$ vagrant -v``. Komento kertoi version olevan asentamani 2.4.0, joten asennus oli onnistunut. 

<img width="536" alt="Näyttökuva 2023-11-06 012837" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/04d4d1dd-ccae-4f07-8961-2e4e3cbc8fa4">

Tämän jälkeen vuorossa oli koneen luominen komennolla ``$ vagrant init bento/ubuntu-22.04``. Komento asentaa Ubuntun 22.04 käyttöjärjestelmän, joka on Ubuntun viimeisin LTS-käyttöjärjestelmä. Koneen luomisen jälkeen käynnistin Vagrantin komennolla ``$ vagrant up``. Käynnistyksessä meni muutamia minuutteja. 

<img width="529" alt="Näyttökuva 2023-11-06 013612" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/21aa6190-836e-4ea3-87d3-ed4ad27a5cf2">

Käynnistyksen jälkeen SSH-yhteyden luominen komennolla ``$ vagrant ssh``. SSH-yhteys onnistui ja olin sisällä koneessa. 
Tarkistin myös, että virtuaalikone oli automaattisesti luotu VirtualBoxiin. Alla olevassa kuvassa SSH-yhteys muodostettu, ja vasemmalla voi nähdä Vagrantin luoman koneen VirtualBoxin valikossa.

<img width="960" alt="Näyttökuva 2023-11-06 014007" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b9578dae-193d-4926-9227-3fad00422998">

<img width="465" alt="Näyttökuva 2023-11-06 021101" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/fa9b372c-f089-495a-905d-f70b94f582ca">


Testasin vielä nettiyhteyden toimivuuden komennolla ``$ ping 8.8.8.8`` ja kuten kuvasta näkyy, nettiyhteys toimii. Lopuksi poistuin koneesta ``exit``-komennolla, ja poistin koneen komennolla ``$ vagrant destroy``. 

## c) Oma orjansa.
Loin uuden koneen samalla tavalla kuin edellisessä tehtävässä, ja SSH-yhteyden luomisen jälkeen asensin herran komennolla ``$ sudo apt-get install salt-master`` ja orjan komennolla  ``$ sudo apt-get install salt-minion``. 
Asennuksen jälkeen tarkistin että asennetut palvelut ovat toivotusti päällä komennolla ``$ sudo systemctl status salt-master`` ja ``$ sudo systemctl status salt-minion``. En ollut täysin varma komennon toimivuudesta, mutta muistan käyttäneeni niitä tarkistaakseni, onko Apache2 päällä, joten laitoin komennon myös tässä tapauksessa testiin. 
Komento toimi ja kertoi molempien palveluiden olevan päällä.

<img width="456" alt="Näyttökuva 2023-11-06 022737" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/33100d8a-7a5a-4c84-bdd1-e6463d1a41b9">

<br>
<img width="459" alt="Näyttökuva 2023-11-06 022806" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/dc04a527-cfc5-4603-a35b-d612ec4cbfe5">

## d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli. 
Aloitin tehtävän luomalla uuden hakemiston <i>essi_vagrant</i>, ja loin sille uuden koneen. Avasin Vagrantfilen muistiossa ja muutin sen sisällön kopioimlla siihen Tero Karvisen valmiiksi luoman Vagrantfilen joka löytyy [täältä](https://terokarvinen.com/2023/salt-vagrant/). 

<img width="404" alt="Näyttökuva 2023-11-06 105939" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/941b7a56-cd33-4d01-a8bc-c5e553f533d7">

Vagrantfile kertoo mitä koneita halutaan luoda ja kuinka monta. Karvisen valmiissa Vagrantfilessä oli siis käytössä Debian ja orjakoneita kaksi. Tämän jälkeen kone käyttöön komennolla ``$ vagrant up``. Tässä meni melko kauan, noin 5 minuuttia. Varmistin vielä, että VirtualBoxiin oli ilmestynyt yksi herra ja kaksi orjaa.

<img width="855" alt="Näyttökuva 2023-11-06 114300" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2ed9785d-7163-4e5f-99b4-4c5e4098b9f0">

<img width="323" alt="Näyttökuva 2023-11-06 115211" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/dd3556b5-1c23-4e91-9f83-fc2610811828">

Tämän jälkeen loin SSH-yhteyden herraan komennolla ``$ vagrant ssh tmaster``.

<img width="496" alt="Näyttökuva 2023-11-06 115153" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b879ba66-bd7a-484f-81b0-0860f9fe1fdc">

SSH-yhteyden luomisen jälkeen hyväksyin orjien avaimet komennolla ``$ sudo salt-key -A`` ja testasin yhteyden toimivuuden ohjeiden mukaisesti komennolla ``$ sudo salt '*' test.ping``. Molemmat orjat vastaavat pingiin true, joten yhteys toimii onnistuneesti. 

<img width="418" alt="Näyttökuva 2023-11-06 115332" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9f31af0f-a018-4e60-97ee-16d9bcc4d802">

## e) Aja useita idempotentteja (state.single) komentoja verkon yli.
Lähdin testaamaan idempotenttia asentamalla Apache2:n komennolla ``$ sudo salt '*' state.single pkg.installed apache2``. Alla olevan kuvan yläreunasta näkyy yhteenveto t001 koneelle ja alhaalla t002 koneen asennustapahtuman alku. Kuvasta näkyy, että asennus onnistui (Changed=1), koska Apache2-palvelua ei löytynyt koneilta.

<img width="429" alt="Näyttökuva 2023-11-06 121525" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/3ddfb8ee-2ae1-44d6-b1b4-b75fe4efcd5b">

Ajoin komennon uudestaan luodakseni idempotentin. Tuloste kertoo "All specified packages are already installed", eli muutoksia ei tapahdu. Lopputulos oli sama molemmilla koneilla.

<img width="406" alt="Näyttökuva 2023-11-06 122017" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/6572ac60-44b7-472c-8ac8-748eaf95a715">

Asennuksen jälkeen lähdin testaamaan toimiiko Apache2 odotetusti komennolla ``$ sudo salt '*' state.single service.running apache2``. Tuloste kertoo, että Apache2 on päällä.

<img width="402" alt="Näyttökuva 2023-11-06 122435" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/65833342-e3ea-474f-9b02-76d9a3cc9a43">

Apache2 toimi toivotulla tavalla, joten seuraavaksi ajoin komennon ``$ sudo salt '*' state.single service.dead apache2`` sammuttaakseni palvelun. Alla olevasta kuvasta näkee, että Apache2 oli onnistuneesti "tapettu". (changed=1)

<img width="441" alt="Näyttökuva 2023-11-06 122724" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/30d553b0-2096-47ff-9970-54691124642f">

Ajoin taas komennon uudestaan luodakseni idempotentin, ja tuloste kertoo että Apache2 on jo "kuollut". Komento on siis ajettu onnistuneesti, mutta muutoksia ei tapahdu.

<img width="400" alt="Näyttökuva 2023-11-06 122820" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0efbfed0-ac61-4f0e-ab13-2187265c9820">

## f) Kerää teknistä tietoa orjista verkon yli (grains.item)
Lähdin keräämään tietoa koneista komennolla ``$ sudo salt '*' grains.items``, joka onkin jo tuttu H1 Viisikko-tehtävästä. Komento antaa taas pitkän listan tietoa molemmista koneista. Halusin selvittää koneiden IP-osoitteet, joten ajoin komennon ``$ sudo salt '*' grains.item osfinger ipv4`` ja saan tulokseksi tarkat tiedot molempien koneiden IPv4 osoitteista. Tulosteesta voi huomata, että t002-orjan IP on 192.168.12.102 ja t001-orjan IP on 192.168.12.100.

<img width="307" alt="Näyttökuva 2023-11-06 124021" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/38d55406-e3fb-4d04-9dba-5c9ec1dc4882">

## g) Aja shell-komento orjalla verkon yli.
Käytin komentoa ``$ sudo salt '*' cmd.run 'ls -la'`` joka antaa tutun ls -la-listauksen. Kokeilin myös komentoa ``$ sudo salt '*' cmd.run 'ls -la /'``, joka listaa koneiden root-hakemiston sisällön.

<img width="299" alt="Näyttökuva 2023-11-06 125118" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/db52ee4f-c5df-4a55-b33b-48507865dfa8">

<img width="349" alt="Näyttökuva 2023-11-06 125248" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/e308bf76-752d-4e0d-bff6-606720c2e142">




## Lähteet:
- https://developer.hashicorp.com/vagrant/downloads
- https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654
- https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/ubuntu.html
- https://terokarvinen.com/2023/salt-vagrant/
- https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/
- https://terokarvinen.com/2023/configuration-management-2023-autumn/
- https://developer.hashicorp.com/vagrant/docs/vagrantfile



