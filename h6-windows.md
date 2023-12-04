# h6 Windows
Tässä raportissa kerron vastaukseni Palvelinten Hallinta- kurssin tehtävään 'h6 Windows'. Tehtävät on suoritettu itsenäisesti 3.12. ja 4.12. aikana.

## x) Lue ja tiivistä. 
- <b>Halonen, Rajala ja Ollikainen 2023: [Installing Windows 10 on a virtual machine](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md)</b>
- <b>LSB Workgroup, The Linux Foundation 2015: [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)</b>

## a) Asenna Windows virtuaalikoneeseen
Asensin virtuaalikoneen 28.11. luennolla Halosen, Rajalan ja Ollikaisen [Installing Windows 10 on a virtual machine](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md) ohjeen mukaisesti. Asennuksesta ei ole kuvia, mutta suoritin sen täysin ohjeen mukaisesti. Alla vielä kuva asennetusta Windowsista VirtualBoxissa. 

<img width="319" alt="Näyttökuva 2023-12-03 212733" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d81867ee-15bd-4dd0-ba5a-95f6e7984f9d">

## b) Asenna Salt Windowsille.
Aloitin tehtävän suorittamisen avaamalla selaimen virtuaalikoneessa, ja Googlaamalla <i>salt windows download</i>. Valitsin ensimmäisen hakutuloksen, joka oli Salt Projectin oma [Windows downloads](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html#windows-downloads) sivu. Ladattuani tiedoston avasin sen ja ryhdyin asentamaan Saltia.

<img width="246" alt="Näyttökuva 2023-12-03 213022" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0e6d60a0-15f8-4920-b173-1632a9b36642">

Suoritin asennuksen oletusasetuksilla. Jätin myös master ja minion hostnamet oletusasetuksille, sillä tulen käyttämään tässä tehtävässä Saltia vain paikallisesti. Asennuksen alussa sain ilmoituksen, että tarvittavaa versiota vcredistä ei ole asennettuna, joten asensin sen myös samalla.<br>
Asennuksen jälkeen siirryin tarkistamaan, että asennus oli onnistunut. Avasin Powershellin <i>Run as Administrator</i>, jotta voin ajaa komentoja adminina tehtävän muissa osioissa. Powershellissä ajoin komennon ``salt-call --local --version``, ja komento kertoi asennuksen onnistuneen.

<img width="290" alt="Näyttökuva 2023-12-03 213354" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d24c4133-6ef1-4117-ad02-28f7cd7f68d9">

## c) Kerää Windows-koneesta tietoa grains.items -toiminnolla
Tämän tehtävän suorittamisen aloitin ajamalla tutun ``salt-call --local grains.items`` komennon. Komennon ajo onnistui, ja se tulosti erilaisia tietoja koneesta. Tässä tehtävässä oli tarkoituksena kuitenkin poimia keskeisiä tietoja koneesta, joten lähdin tarkentamaan hakutuloksia muuttamalla komentoa hieman. Komento tulee olla muodossa <i>grains.item</i>, sillä monikossa komento tulostaa kaikki tiedot. <br>
Halusin selvittää virtuaalikoneen muistin määrän sekä IPv4 ja IPv6-osoitteet. Bongasin kuitenkin aiemmasta tulosteesta kohdan saltpath, ja selvitin myös sen komennolla ``salt-call --local grains.item saltpath``. Oletan saltpathin kertovan Saltin sijainnin virtuaalikoneella. Alla tuloste jokaisesta komennosta.

<img width="329" alt="Näyttökuva 2023-12-04 121026" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/fe32f850-b316-4b15-b541-e8388c225824">

## d) Kokeile Saltin file -toimintoa Windowsilla.
Tässä tehtävässä käyttöön pääsi taas aiemmista tehtävistä tuttu <i>file.managed</i> komento. Halusin luoda työpöydälle uuden <i>testi.txt</i> tiedoston, ja sitä varten ajoin komennon ``salt call --local state.single file.managed 'C:\users\essi\Desktop\testi.txt'``. 

<img width="503" alt="Näyttökuva 2023-12-03 214131" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f87621e3-1e46-4847-bb88-a31982d47eb7">

Tuloste kertoo, että komennon ajo on onnistunut ja uusi tiedosto <i>testi.txt</i> on luotu haluttuun sijaintiin. Halusin vielä tarkistaa, että tiedosto on luotu, ja sitä varten siirryin virtuaalikoneeni työpöydälle. Tiedosto löytyi sieltä mistä pitikin!

<img width="45" alt="Näyttökuva 2023-12-03 214146" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2514e215-3644-4c6e-ba15-965dc708595e">


## e) Kokeile jotain itsellesi uutta toimintoa Saltista Windowsilla.
Tätä tehtävää varten lähdin etsimään jotain itselleni vielä vierasta toimintoa. Hetken googleteltuani löysin Salt Projectin omilta sivuilta [salt.modules.system](https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.system.html) toiminnon, ja päätin lähteä testaamaan sitä. 
Ajoin ensimmäisenä komennon ``salt-call --local system.get_computer_desc``, minkä päättelin tulostavan halutun koneen kuvauksen. Komento printtasi tyhjää, mutta käyttämässäni ohjesivussa oli myös kerrottu komento, millä koneelle pystyy lisäämään kuvauksen. Lähdin ajamaan komentoa ``salt-call --local system.set_computer_desc "VirtuaaliWinukka"``. Komennossa lainausmerkkien sisällä oleva parametri tulee siis olemaan valitun koneen kuvaus.<br>
Ajettuani <i>system.set_computer_desc</i>-komennon, ajoin vielä uudestaan ``salt-call --local system.get_computer_desc`` komennon, joka tulosti nyt äsken antamani kuvauksen koneelle.

<img width="389" alt="Näyttökuva 2023-12-04 113553" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/3d31dd49-dd52-4c7d-9ef2-d6590a0dfff6">

Komennot toimi niin kuin pitikin! 

## Lähteet:
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h6-windows Luettu 3.12.2023
- Halonen, Rajala ja Ollikainen, 2023. Installing Windows 10 on a virtual machine. Luettavissa: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md Luettu 3.12.2023
- Salt Project, s.a.. Salt install guide Windows. Luettavissa: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html Luettu: 3.12.2023
- Salt Project, s.a.. salt.modules.system. Luettavissa: https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.system.html#module-salt.modules.system Luettu 3.12.2023
