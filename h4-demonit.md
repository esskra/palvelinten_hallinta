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

Tämän jälkeen testaamaan, toimiiko tila ajamalla komento ``$ sudo salt '*' state.apply``. Koska tein muutokset <i>top.sls</i>-tiedostoon, ei 


