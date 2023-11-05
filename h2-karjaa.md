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






## Lähteet:
https://developer.hashicorp.com/vagrant/downloads

