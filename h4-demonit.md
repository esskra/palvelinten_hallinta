# h4 Demonit
## x) Lue ja tiivistä

## a) Hello SLS!
Tätä tehtävää aloin tekemään käyttäen aiemmin <i>H2 karjaa</i>-tehtävässä luotua herra-orja-arkkitehtuuria. Aloitin tehtävän avaamalla oman Windows-koneeni komentokehoitteen, jossa siirryin oikeaan hakemistoon komennolla <br> ``$ cd essi_vagrant``. Koneet pyörimään tutulla ``$ vagrant up`` komennolla, jonka jälkeen muodostin SSH-yhteyden komennolla ``$ vagrant ssh``. Siirryin hakemistoon /srv/salt/hello, johon olin jo aiemmassa tehtävässä luonut <i>init.sls</i>- tiedoston. Komennolla ``$ sudo nano init.sls`` siirryin muokkaamaan tiedoston sisältöä nano-tekstieditorissa. Poistin ensin vanhan tehtävän sisällöt tiedostosta, jonka jälkeen lisäsin tiedostoon seuraavan sisällön:
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
Siirryin takaisin herra-koneelle komennolla ``$ vagrant ssh`` jonka jälkeen vuorossa oli tilan luominen tiedostoon. Aloitin luomalla tehtävää varten uuden hakemiston komennolla ``$ mkdir apache`` hakemistoon /var/salt. Tämän jälkeen loin /var/salt/apache hakemistoon uuden <i>init.sls</i>-tiedoston, jonka jälkeen siirryin kirjoittamaan tilaa komennolla ``$ sudo nano init.sls``. Tarkoituksena oli siis tehdä sama kuin edellisessä vaiheessa, mutta nyt automatisoituna. Tähän vaiheeseen käytin apuna [tätä](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules) Salt Projectin ohjetta. Ensimmäiseksi lisäsin tiedostoon pkg.installend funktion, joka asentaa halutun palvelun. Tämän jälkeen älkeen kaksi file-tilafunktiota, joista toinen poistaa halutun tiedoston ja toineen lisää halutun tiedoston haluttuun hakemiston. Viimeisenä service-funktiolla tarkastus, että demoni on käynnissä. Tiedoston sisältö näytti seuraavalta:

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







# Lähteet:
