# h4 Demonit
## x) Lue ja tiivistä

## a) Hello SLS!
Tätä tehtävää aloin tekemään käyttäen aiemmin <i>H2 karjaa</i>-tehtävässä luotua herra-orja-arkkitehtuuria. Aloitin tehtävän avaamalla oman Windows-koneeni komentokehoitteen, jossa siirryin oikeaan hakemistoon komennolla <br> ``$ cd essi_vagrant``. Koneet käyntiin tutulla ``$ vagrant up`` komennolla, jonka jälkeen muodostin SSH-yhteyden komennolla ``$ vagrant ssh``. Siirryin hakemistoon /srv/salt/hello, johon olin jo aiemmassa tehtävässä luonut <i>init.sls</i>- tiedoston. Komennolla ``$ sudo nano init.sls`` siirryin muokkaamaan tiedoston sisältöä nano-tekstieditorissa. Poistin ensin vanhan tehtävän sisällöt tiedostosta, jonka jälkeen lisäsin tiedostoon seuraavan sisällön:
<br>
```
tasks:
  cmd.run:
    - name: echo 'Hello World!'
```

<img width="218" alt="Näyttökuva 2023-11-19 115604" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/dc259370-c231-4107-9efc-14a3f5d583f9">

Muokattuani tiedoston sisältöä, oli vuorossa testata tilan toimiminen. Ajoin komennon ``$ sudo salt '*' state.apply hello``, ja tuloste kertoi cmd.run ajon onnistuneen molemmilla orjilla. Molempien orjien tuloste kertoi succeeded-kohdassa, että muutoksia on tehty. Lisäksi stdout-kohta kertoi komennon tulostaneen <i>"Hello World!"</i>.

<img width="434" alt="Näyttökuva 2023-11-19 115642" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/76fd3c19-a988-4c03-9a3f-2cbabaedcd10">

## b) Top.
Tässä tehtävässä oli tarkoituksena luoda <i>top.sls</i>-tiedosto, jonka avulla tilat voidaan ajaa automaattisesti. Lähdin tekemään tehtävää luomalla ensin tiedoston komennolla ``$ sudo nano top.sls`` hakemistoon /srv/salt. Lisäsin tiedostoon Teron [top.sls - What Slave Runs What States](https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)- ohjeen mukaisen pätkän koodia:

```
base:
  '*':
    - hello
```

<img width="215" alt="Näyttökuva 2023-11-19 124439" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/cc04afe3-feb5-4fda-9c71-fdd6bd97cc27">

Tämän jälkeen testaamaan, toimiiko tila ajamalla komento ``$ sudo salt '*' state.apply``. Koska tein muutokset <i>top.sls</i>-tiedostoon, komenolle ei tarvitse asettaa erikseen kohde-parametrejä. Tilan ajo sujui taas onnistuneesti molemmilla orjilla.  

<img width="357" alt="Näyttökuva 2023-11-19 124424" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2c3a9046-b1d4-4248-9005-8c56cb996eda">

## c) Apache. 
Aloitin tehtävän asentamalla Apachen sekä korvaamalla sen testisivun uudella tiedostolla käsin. Poistuin masterilta ja siirryin tekemään tehtävää orjakoneelle komennolla ``$ vagrant ssh t001``. Päästyäni sisään t001-orjalle asensin Apachen komennolla ``$ sudo apt-get install apache2`` ja tarkistin vielä asennuksen onnistuneen komennolla ``$ sudo systemctl status apache2``. Tuloste kertoi, että Apache oli onnistuneesti asennettu ja palvelu oli aktiivinen (running).

<img width="565" alt="Näyttökuva 2023-11-19 131543" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b5c079fc-9ccb-4ca6-9f0d-92252998654a">

Seuraavaksi vuorossa oli testisivun korvaaminen uudella tiedostolla. Testisivu <i>index.html</i> sijaitsi /var/www/html hakemistossa, ja sinne siirryttyäni poistin testisivun tiedoston komennolla ``$ sudo rm index.html``. Poistettuani testisivun loin hakemistoon korvaavan tiedoston komennolla ``$ sudo nano indexkorvaus.txt``.

<img width="332" alt="Näyttökuva 2023-11-19 131320" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9bac305b-c5d6-44d2-b8d6-8f85cd12e8d2">

Aiemmin tulikin jo testattua, että palvelu on päällä, joten poistin Apachen t001-orjalta komennolla ``$ sudo apt-get remove apache2`` ja poistuin koneelta.
<br>
Siirryin takaisin herra-koneelle komennolla ``$ vagrant ssh`` jonka jälkeen vuorossa oli tilan luominen tiedostoon. Aloitin luomalla tehtävää varten uuden hakemiston komennolla ``$ mkdir apache`` hakemistoon /var/salt. Tämän jälkeen loin /var/salt/apache hakemistoon uuden <i>init.sls</i>-tiedoston.

<img width="280" alt="Näyttökuva 2023-11-19 133732" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a3a3c4f6-7cee-47ed-94a2-7467acd96aa5">

Tarkoituksena oli siis tehdä sama kuin edellisessä vaiheessa, mutta nyt automatisoituna. Tähän vaiheeseen käytin apuna [tätä](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules) Salt Projectin ohjetta. <br>
Aloitin kirjoittamaan tilaa komennolla ``$ sudo nano init.sls``, jonka avulla siirryin nano-tekstieditoriin. Ensimmäiseksi lisäsin tiedostoon pkg.installend funktion, joka asentaa halutun palvelun. Tämän jälkeen kaksi file-tilafunktiota, joista toinen poistaa halutun tiedoston ja toinen lisää halutun tiedoston haluttuun hakemiston. Viimeisenä service-funktiolla tarkastus, että demoni on käynnissä. Tiedoston sisältö näytti seuraavalta:

```
apache2_asennus:
  pkg.installed:
    - name: apache2

index_poisto:
  file.absent:
    - name: /var/www/html/index.html

korvaaja:
  file.managed:
    - name: /var/www/html/indexkorvaus.html

demoni:
  service.running:
    - name: apache2
    - enable: True
```

<img width="298" alt="Näyttökuva 2023-11-19 133119" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2113721b-0052-49fc-bd02-4c0f65ae9109">

Tässä kohtaa vähän jännitti, olinko onnistunut tehtävässä, joten ajoin komennon ``$ sudo salt '*' state.apply apache`` ottaakseni siitä selvää. Vähän yllätyksekseni tuloste kertoi tilan ajon onnistuneen! Alla olevissa kuvissa orjakoneen t002 tapahtuneet muutokset: Apache2 on jo asennettu, joten siinä ei muutoksia. <i>index.html</i> tiedosto on poistettu ja tyhjä tiedosto <i>indexkorvaus.html</i> lisätty /var/www/html hakemistoon. Viimeisenä service.running kertoo, että Apache2 on jo käytössä, tulos on True, muutoksia ei tapahdu. Lopussa tuloste kertoo, että kaikki neljä tilaa on asennettu onnistuneesti, mutta vain kahteen on tehty muutoksia. Tässä taas hyvä esimerkki idempotentista!

<img width="373" alt="Näyttökuva 2023-11-19 133626" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b195cb1f-35ff-4cab-86a7-0463f7887d00">

<img width="296" alt="Näyttökuva 2023-11-19 133643" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/6457b339-d16d-466b-9d96-3cebb391f816">

Koska ajoin state.apply komennon molemmille koneelle, sain myös tulosteen molempien koneiden tilan ajosta. Koska poistin Apachen t001-koneelta, init.sls- tiedoston pkg.installed funktio asensi Apachen sille onnistuneesti. Muuten tuloste oli sama kuin t002-koneella.

<img width="387" alt="Näyttökuva 2023-11-19 133720" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/4f9b6eeb-1026-47c5-9b04-a80cb6c974ee">


## d) SSHouto
Tätä tehtävää siirryin tekemään tehtävänannon vinkin mukaisesti tavalliselle virtuaalikoneelle.
Tässä tehtävässä siis käytössä taas aiemmin asennettu Debian 12-virtuaalikone ja käytin apuna [tätä](https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/) Tero Karvisen ohjetta. Kirjauduttuani koneelle ja avattuani terminaalin ajoin ``$ sudo apt-get update`` ja ``$ sudo apt-get upgrade`` komennot varmistaakseni, että paketit olivat ajan tasalla. 
<br>
Pakettien päivittämisen jälkeen sirryin /etc/ssh hakemistoon. Listasin hakemiston sisällön, ja hetken ihmettelin, ettei sshd-konfiguraatiotiedostoa löydy hakemistosta. Hetken ihmettelyn jälkeen onneksi tajusin, etten ollut vielä asentanut palvelua Debian-virtuaalikoneelleni :D Tästä viisastuneena siis asentamaan palvelu komennolla ``$ sudo apt-get install openssh-server``, jonka jälkeen hakemiston listaukseen olikin ilmestynyt <i>sshd_config</i>-tiedosto. Siirryin muokkaamaan tiedostoa komennolla ``$ sudo nano sshd_config``. <br>
Etsin tiedostosta ohjeiden mukaisesti portista kertovan kohdan. Poistin <i>"Port"</i>-kohdasta kommenttia tarkoittavan #-merkin ja muutin portin numeroksi '8888'.

<img width="480" alt="Näyttökuva 2023-11-19 135116" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/8991c88c-075d-4fa4-86e4-ab77f61acd61">

Ohjeessa neuvottiin seuraavaksi käynnistämään demoni uudestaan, mutta muuten siitä ei liiemmin ollut tähän vaiheeseen apua. Kokeilin komentoa ``$ sudo systemctl restart ssh.service`` ja tarkistin palvelun statuksen komennolla ``$ sudo systemctl status ssh.service``. Palvelu näytti olevan aktiivinen, joten siirryin seuraavaan vaiheeseen.

<img width="428" alt="Näyttökuva 2023-11-19 140800" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0162097f-087c-4208-bef7-8106b278ff91">

Testasin ohjeessa olevaa ``$ nc -vz 8888`` komentoa eri kohdeosoitteilla, mutta komento tulosti toistuvasti ettei <i>nc</i>-komentoa löydy. Aikani yritettyä jouduin toteamaan, ettei komento toimi. Ajattelin tämän johtuneen siitä, ettei demonin uudelleenkäynnistys ollut toiminut. Testasin tällä kertaa komentoa ``$ sudo systemctl stop ssh.service``, varmistin että palvelu on varmasti pois päältä, jonka jälkeen vasta käynnistin sen uudestaan komennolla ``$ sudo systemctl start ssh.service``. Tarkistin vielä lopuksi, että demoni oli päällä, eli nyt uudelleenkäynnistys oli varmasti onnistunut. <br>
Kokeilin uudestaan ``$ nc -vz 8888`` ja törmäsin jälleen samaan ongelmaan. Lähdin siis tutkimaan Teron ohjetta tarkemmin ja huomasin vasta tässä vaiheessa, että yhteyden voi myös muodostaa SSHn avulla. <br>
Nyt ajoin vuorostaan komennon ``$ $ ssh -p8888 essi@essi-virtualbox`` ja komento toimi! Salasanani annettua olin päässy sisään koneelleni aiemmin antamani 8888-portin kautta. Ennen koneelta poistumista tarkistin vielä, että olin varmasti etäyhteydellä koneen sisällä komennolla ``$ who`` ja sisällä oltiin! Varmistuksen jälkeen poistuin koneelta komennolla ``$ exit``.

<img width="425" alt="Näyttökuva 2023-11-19 140910" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/e6a2a529-c32d-464a-8f28-8e8d6b0276bc">

<img width="302" alt="Näyttökuva 2023-11-19 140928" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7423f4c3-52ee-4aba-850b-6acd86511506">



# Lähteet:
- https://docs.saltproject.io/en/getstarted/fundamentals/top.html
- 

