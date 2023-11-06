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

<img width="529" alt="Näyttökuva 2023-11-06 013612" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/6774f2f1-b297-4069-aa12-abe4a25b541d">

Käynnistyksen jälkeen SSH-yhteyden luominen komennolla ``$ vagrant ssh``. SSH-yhteys onnistui ja olin sisällä koneessa. 
Tarkistin myös, että virtuaalikone oli automaattisesti luotu VirtualBoxiin. Alla olevassa kuvassa SSH-yhteys muodostettu, ja vasemmalla voi nähdä Vagrantin luoman koneen VirtualBoxin valikossa.

<img width="960" alt="Näyttökuva 2023-11-06 014007" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b9578dae-193d-4926-9227-3fad00422998">

<img width="465" alt="Näyttökuva 2023-11-06 021101" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/fa9b372c-f089-495a-905d-f70b94f582ca">


Testasin vielä nettiyhteyden toimivuuden komennolla ``$ ping 8.8.8.8`` ja kuten kuvasta näkyy, nettiyhteys toimii. Lopuksi poistuin koneesta ``exit``-komennolla, ja poistin koneen komennolla ``$ vagrant destroy``. 

## c) Oma orjansa.
Loin uuden koneen samalla tavalla kuin edellisessä tehtävässä ja SSH-yhteyden luomisen jälkeen asensin herran komennolla ``$ sudo apt-get install salt-master`` ja orjan komennolla  ``$ sudo apt-get install salt-minion``. 
Asennuksen jälkeen tarkistin että asennetut palvelut ovat toivotusti päällä komennolla ``$ sudo systemctl status salt-master`` ja ``$ sudo systemctl status salt-minion``. En ollut täysin varma komennon toimivuudesta, mutta muistan käyttäneeni niitä tarkistaakseni, onko Apache2 päällä, joten laitoin komennon myös tässä tapauksessa testiin. Komento toimi ja kertoi molempien palveluiden olevan päällä.

<img width="456" alt="Näyttökuva 2023-11-06 022737" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/33100d8a-7a5a-4c84-bdd1-e6463d1a41b9">

<br>
<img width="459" alt="Näyttökuva 2023-11-06 022806" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/dc04a527-cfc5-4603-a35b-d612ec4cbfe5">



## Lähteet:
https://developer.hashicorp.com/vagrant/downloads
https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/ubuntu.html


