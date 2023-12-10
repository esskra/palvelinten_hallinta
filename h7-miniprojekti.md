# h7 Miniprojekti
Kokemukseni Linuxista on kurssin loppuessa vielä varsin vähäistä, joten tämä miniprojekti on myös siitä johtuen melko yksinkertainen. Halusin kuitenkin päästä hyödyntämään mahdollisimman montaa kurssilla oppimaani asiaa, ja pohdinnan jälkeen päädyin siihen, että tämän miniprojektin tarkoituksena on luoda Vagrantilla herra-orja-arkkitehtuuri ja asentaa niille halutut konfiguraatiot. Käytössä on yksi herra-kone ja kaksi orja-konetta. 

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

## Apache



