# h7 Miniprojekti
Kokemukseni Linuxista on kurssin loppuessa vielä varsin vähäistä, joten tämä miniprojekti on myös siitä johtuen melko yksinkertainen. Halusin kuitenkin päästä hyödyntämään mahdollisimman montaa kurssilla oppimaani asiaa, ja pohdinnan jälkeen päädyin siihen, että tämän miniprojektin tarkoituksena on luoda Vagrantilla herra/orja-arkkitehtuuri ja asentaa sille halutut konfiguraatiot. Käytössä on yksi herra-kone ja kaksi orja-konetta. Kaikki kohdat on tehty ensin käsin aiemmin luodulla herra/orja- arkkitehtuurilla, mutta tässä raportissa käyn läpi vain uudessa ympäristössä toteutetun automatisaation.

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

## Tilan ajaminen
Halusin ajaa tilat mahdollisimman tehokkaasti, ja sitä varten loin ``sudo nano top.sls``-komennolla <i>top.sls</i> tiedoston, jotta saisin kaikki komennot ajettua kerralla. Lisäsin tiedostoon seuraavan sisällön:

<img width="304" alt="Näyttökuva 2023-12-11 000034" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/56f573dc-9b65-4ecd-99ea-cc2cf7fa73dd">

Tiedoston luotuani pääsin siirtymään tilan ajoon. Komennolla ``$ sudo salt '*' state.apply`` ajoin tilan molemmille orjille. Alla olevat kuvat otettu toisesta ajosta, jotta saisin tulosteen mahtumaan kuvaan:

<img width="435" alt="Näyttökuva 2023-12-10 231551" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/045a39f0-f4c5-4671-bc04-65629e63a746">
<br>
<img width="368" alt="Näyttökuva 2023-12-10 231604" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/18dbf631-a729-4b52-81ca-80927b53d6a5">

<img width="192" alt="Näyttökuva 2023-12-10 231615" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/063ccabb-57fd-401a-aeea-e067d0d3d608">

Tilan ajaminen onnistui ilman virheitä molemmille orjille! Nyt molemmilla orjilla oli siis asennettuna Apache, jonka oletuskotisivu oli muutettu aiemmin luomaani <i>apache.html</i> sivuun. Tämän lisäksi orjilla oli nyt käytössä aiemmin luomani komento, sekä asennettuna ohjelmat <i>git, curl, tree & cowsay</i>. 

## Tarkistus
Tilan onnistuneen ajon jälkeen siirryin vielä orjalle tarkistamaan, että kaikki toimi niin kuin pitikin. Alla olevassa kuvassa hieman testailua:

<img width="341" alt="Näyttökuva 2023-12-11 000805" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/adf27cb2-7ec9-4990-b87c-4c8305737f9f">

``$ curl`` ja ``$ tree`` komennot toimivat, minkä lisäksi oma komentoni ``$ komento`` toimii myös halutulla tavalla. Git on myös asentunut, ja asennettu versio on 2.30.2.
Hyvältä näyttää! Lopuksi ajoin vielä aiemmin asennetun ``$ cowsay`` komennon, joka toimi myös.

<img width="308" alt="Näyttökuva 2023-12-11 000903" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0ae2462d-db31-4db1-941c-07ecd2f2b6d5">

# Lähteet
- Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu 10.12.2023
- Karvinen, T. 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/ Luettu 10.12.2023
- Krakau, E. 2023. H5 CSI Kerava. Luettavissa: https://github.com/esskra/palvelinten_hallinta/blob/main/h5-CSI-kerava.md Luettu 10.12.2023
- Krakau, E. 2023. H2 Karjaa. Luettavissa: https://github.com/esskra/palvelinten_hallinta/blob/main/h2-karjaa.md Luettu: 10.12.2023



