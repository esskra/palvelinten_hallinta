# h7 Miniprojekti
Kokemukseni Linuxista on kurssin loppuessa vielä varsin vähäistä, joten tämä miniprojekti on myös siitä johtuen melko yksinkertainen. Halusin kuitenkin päästä hyödyntämään mahdollisimman montaa kurssilla oppimaani asiaa, ja pohdinnan jälkeen päädyin siihen, että tämän miniprojektin tarkoituksena on luoda Vagrantilla herra/orja-arkkitehtuuri ja asentaa niille halutut konfiguraatiot. Käytössä on yksi herra-kone ja kaksi orja-konetta. Kaikki kohdat on tehty ensin käsin aiemmin luodulla herra/orja- arkkitehtuurilla, mutta tässä raportissa käyn läpi vain uudessa ympäristössä toteutetun automatisaation.

<b>Projektin tavoitteena on:</b>
- Luoda onnistunut herra/orja-arkkitehtuuri Vagrantilla.
- Asentaa Apache ja luoda automaattisesti käyttäjälle oma kotisivu.
- Asentaa muutama ohjelma: <i>git, curl, tree & cowsay</i>
- Luoda oma komento toimimaan orjilla.

## Herra/orja arkkitehtuuri
Aloitin projektin luomalla tarvittavan ympäristön sen suorittamiseen. Siirryin isäntäkoneella Windowsin komentokehotteeseen, ja loin <i>projekti</i>-hakemistoon uuden Vagrant-koneen komennolla ``vagrant init``. Vagrantin olin asentanut Windows-koneelleni jo aiemmin tehtävässä [h2 Karjaa](https://github.com/esskra/palvelinten_hallinta/blob/main/h2-karjaa.md). Koneen luotuani avasin Vagrantfilen ja liitin siihen seuraavat konfiguraatiot herra/orja- arkkitehtuuria varten:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2014-2023 Tero Karvinen http://TeroKarvinen.com

$minion = <<MINION
sudo apt-get update
sudo apt-get -qy install salt-minion
echo "master: 192.168.12.3">/etc/salt/minion
sudo service salt-minion restart
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MINION

$master = <<MASTER
sudo apt-get update
sudo apt-get -qy install salt-master
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MASTER

Vagrant.configure("2") do |config|
	config.vm.box = "debian/bullseye64"

	config.vm.define "t001" do |t001|
		t001.vm.provision :shell, inline: $minion
		t001.vm.network "private_network", ip: "192.168.12.100"
		t001.vm.hostname = "ulla"
	end

	config.vm.define "t002" do |t002|
		t002.vm.provision :shell, inline: $minion
		t002.vm.network "private_network", ip: "192.168.12.102"
		t002.vm.hostname = "seppo"
	end

	config.vm.define "tmaster", primary: true do |tmaster|
		tmaster.vm.provision :shell, inline: $master
		tmaster.vm.network "private_network", ip: "192.168.12.3"
		tmaster.vm.hostname = "herra"
	end
end
```
Vagrantfilen muokkaamisen jälkeen ajoin ``vagrant up``- komennon komentokehotteessa käynnistääkseni koneet, jonka jälkeen muodostin yhteyden herra-koneeseen komennolla ``vagrant ssh tmaster``. 
Nyt oli jäljellä enää orjien avainten hyväksyminen, ja pääsisin varsinaisen projektin pariin. Ajoin komennon ``$ sudo salt-key -A`` ja hyväksyin molempien orja-koneiden avaimet. Testasin lopuksi yhteyden komennolla ``$ sudo salt '*' test.ping`` ja molemmat orjat vastasivat pingiin True, joten yhteys koneiden välillä toimi. 

<img width="323" alt="Näyttökuva 2023-12-10 221316" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d206e868-d745-4683-83c0-208092a4522b">

Yhteyden testaamisen jälkeen varmistin vielä, että koneiden paketit olivat ajan tasalla. Ajoin orjille komennot ``$ sudo salt '*' cmd.run 'sudo apt-get update'`` ja ``$ sudo salt '*' cmd.run 'sudo apt-get upgrade -y'``.

## Moduulit
Moduuleita varten loin uuden hakemiston komennolla ``$ sudo mkdir /srv/salt``. <i>/salt</i>-hakemistoon oli tarkoituksena luoda jokaiselle moduulille oma hakemisto.

### Apache
Aloitin luomalla <i>/salt</i>-hakemistoon uuden hakemiston Apachea varten komennolla ``$ sudo mkdir apache``. <i>apache</i> -hakemistossa loin ensin html-tiedoston, joka tulisi korvaamaan Apachen <i>index.html</i>-tiedoston. Alla olevassa kuvassa tiedoston sisältö: 

<img width="366" alt="Näyttökuva 2023-12-10 235734" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d3a8802e-970c-4743-a59e-8bd8eb0c87ee">

Luotuani <i>apache.html</i> tiedoston komennolla ``$ sudo nano apache.html`` siirryin nanoon luomaan <i>init.sls</i>-tiedostoa. Lisäsin sinne hieman muokattuna aiemmin [h5 CSI-Kerava](https://github.com/esskra/palvelinten_hallinta/blob/main/h5-CSI-kerava.md) tehtävässä käyttämäni konfiguraation:

```
apache2:
 pkg.installed
/var/www/html/index.html:
 file.managed:
   - source: salt://apache/apache.html
/etc/apache2/mods-enabled/userdir.conf:
 file.symlink:
   - target: ../mods-available/userdir.conf
/etc/apache2/mods-enabled/userdir.load:
 file.symlink:
   - target: ../mods-available/userdir.load
apache2service:
 service.running:
   - name: apache2
   - watch:
     - file: /etc/apache2/mods-enabled/userdir.conf
     - file: /etc/apache2/mods-enabled/userdir.load
```
Apachen osalta alkoi näyttää valmiilta, joten pääsin siirtymään seuraavaan osioon.

### Ohjelmat
Uusien pakettien asennusta varten loin taas uuden hakemiston /srv/salt-hakemistoon, tällä kertaa nimellä <i>ohjelmat</i>. Tällä kertaa riitti vain yhden tiedoston luominen, ja sen tein tuttuun tapaan komennolla ``$ sudo nano init.sls``. Alla olevassa kuvassa tiedoston sisältö, joka kertoo paketit, jotka haluan orjille asentaa:

<img width="333" alt="Näyttökuva 2023-12-10 224447" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/410f41ad-ea3c-49fa-8dbe-547c80351fa2">


### Oma komento
Viimeiseksi loin vielä <i>komento</i>- hakemiston, jonne loin komennolla ``$ sudo nano komento`` lyhyen shell scriptin, joka tulisi toimimaan omana komentonaan. Scriptin tarkoituksena oli vain yksinkertaisesti echota haluttu viesti. 
Luotuani shell scriptin loin vielä <i>komento</i>-hakemistoon oman <i>init.sls</i>-tiedoston. Lisäsin tiedostoon seuraavat konfiguraatiot: 

<img width="315" alt="Näyttökuva 2023-12-11 000004" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/61d07fc4-8e37-44e6-8dd0-44276224a7bb">

Tässä vaiheessa kaikki konfiguraatiot oli valmiina, ja pääsin siirtymään tilojen ajamiseen!

### Tilan ajaminen
Halusin ajaa tilat mahdollisimman tehokkaasti, ja sitä varten loin ``sudo nano top.sls``-komennolla <i>top.sls</i> tiedoston, jotta saisin kaikki komennot ajettua kerralla. Lisäsin tiedostoon seuraavan sisällön:

<img width="304" alt="Näyttökuva 2023-12-11 000034" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/56f573dc-9b65-4ecd-99ea-cc2cf7fa73dd">

Tiedoston luotuani pääsin siirtymään tilojen ajoon. Komennolla ``$ sudo salt '*' state.apply`` ajoin tilat molemmille orjille. Tulosteena onnistunut ajo, ja 

