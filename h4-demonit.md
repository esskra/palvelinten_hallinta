# h4 Demonit
## x) Lue ja tiivistä

## a) Hello SLS!
><i><b>Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.</i></b>
<br>
Tätä tehtävää aloin tekemään käyttäen aiemmin <i>H2 karjaa</i>-tehtävässä luotua herra-orja-arkkitehtuuria. Aloitin tehtävän avaamalla oman Windows-koneeni komentokehoitteen, jossa siirryin oikeaan hakemistoon komennolla ```$ cd essi_vagrant```. Koneet pyörimään tutulla <br>``$ vagrant up`` komennolla, jonka jälkeen muodostin SSH-yhteyden komennolla ``$ vagrant ssh``. Siirryin hakemistoon /srv/salt/hello, johon olin jo aiemmassa tehtävässä luonut init.sls tiedoston. Komennolla ``$ sudo nano init.sls`` siirryin muokkaamaan tiedoston sisältöä Nano-tekstieditorissa. Poistin ensin vanhan tehtävän sisällöt tiedostosta, jonka jälkeen lisäsin tiedostoon seuraavan sisällön:
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
> Tee top.sls niin, että tilat ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply".

