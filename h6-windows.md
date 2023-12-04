# h6 Windows
Tässä raportissa kerron vastaukseni Palvelinten Hallinta- kurssin tehtävään 'h6 Windows'. Tehtävät on suoritettu itsenäisesti 3.12. ja 4.12. aikana.

## x) Lue ja tiivistä. 
- <b>Halonen, Rajala ja Ollikainen 2023: [Installing Windows 10 on a virtual machine](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md)</b>
- <b>LSB Workgroup, The Linux Foundation 2015: [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)</b>

## a) Asenna Windows virtuaalikoneeseen
Asensin virtuaalikoneen 28.11. luennolla [Halosen, Rajalan ja Ollikaisen](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md) ohjeen mukaisesti. Asennuksesta ei ole kuvia, mutta suoritin sen täysin ohjeen mukaisesti. Alla vielä kuva asennetusta Windowsista VirtualBoxissa. 

<img width="319" alt="Näyttökuva 2023-12-03 212733" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d81867ee-15bd-4dd0-ba5a-95f6e7984f9d">

## b) Asenna Salt Windowsille.
Aloitin tehtävän suorittamisen avaamalla selaimen virtuaalikoneessa, ja Googlaamalla <i>salt windows download</i>. Valitsin ensimmäisen hakutuloksen, joka oli Salt Projectin oma [Windows downloads](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html#windows-downloads) sivu. Ladattuani tiedoston avasin sen ja ryhdyin asentamaan Saltia.

<img width="246" alt="Näyttökuva 2023-12-03 213022" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0e6d60a0-15f8-4920-b173-1632a9b36642">

Suoritin asennuksen oletusasetuksilla. Jätin myös master ja minion hostnamet oletusasetuksille, sillä tulen käyttämään tässä tehtävässä Saltia vain paikallisesti. Asennuksen alussa sain ilmoituksen, että tarvittavaa versiota vcredistä ei ole asennettuna, joten asensin sen myös samalla.<br>
Asennuksen jälkeen siirryin tarkistamaan, että asennus oli onnistunut. Avasin Powershellin <i>Run as Administrator</i>, jotta voin ajaa komentoja adminina tehtävän muissa osioissa. Powershellissä ajoin komennon ``$ salt-call --local --version``, ja komento kertoi asennuksen onnistuneen.

<img width="290" alt="Näyttökuva 2023-12-03 213354" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d24c4133-6ef1-4117-ad02-28f7cd7f68d9">

## c) Kerää Windows-koneesta tietoa grains.items -toiminnolla


## d) Kokeile Saltin file -toimintoa Windowsilla.
## e) Kokeile jotain itsellesi uutta toimintoa Saltista Windowsilla.

## Lähteet:
https://terokarvinen.com/2023/configuration-management-2023-autumn/#h6-windows
https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md
https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html
https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html
