# h4 Demonit
## x) Lue ja tiivistä

## a) Hello SLS!
><i><b>Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.</i></b>
<br>
Tässä tehtävässä käytin aiemmin <i>H2 karjaa</i>-tehtävässä luotua herra-orja-arkkitehtuuria. Aloitin tehtävän avaamalla oman Windows-koneeni komentokehoitteen, jossa siirryin oikeaan hakemistoon komennolla ``$ cd essi_vagrant``. Koneet pyörimään tutulla ``$ vagrant up`` komennolla, jonka jälkeen muodostin SSH-yhteyden komennolla ``$ vagrant ssh``. Siirryin hakemistoon /srv/salt/hello, johon olin jo aiemmassa tehtävässä luonut init.sls tiedoston. 

<img width="496" alt="Näyttökuva 2023-11-19 112006" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/c0a230c8-7fbe-4f73-bc04-ac6a98b11dc3">

Komennolla ``$ sudo nano init.sls`` siirryin muokkaamaan tiedoston sisältöä. Poistin ensin vanhan tehtävän sisällöt tiedostosta, jonka jälkeen lisäsin tiedostoon seuraavan sisällön:

```
tasks:
  cmd.run:
    - name: echo 'Hello World!'
```

<img width="218" alt="Näyttökuva 2023-11-19 115604" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/dc259370-c231-4107-9efc-14a3f5d583f9">

