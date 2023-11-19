# h4 Demonit
## x) Lue ja tiivistä

## a) Hello SLS!
<i><b>Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.</i></b>
<br>
Tässä tehtävässä käytin aiemmin H2 karjaa-tehtävässä luotua herra-orja-arkkitehtuuria. Aloitin tehtävän avaamalla oman Windows-koneeni komentokehoitteen, jossa siirryin oikeaan hakemistoon komennolla ``$ cd essi_vagrant``. Koneet pyörimään tutulla ``$ vagrant up`` komennolla, jonka jälkeen muodostin SSH-yhteyden komennolla ``$ vagrant ssh``. 
Siirryin hakemistoon /srv/salt/hello, ja aloin muokkaamaan sieltä löytyvää, jo aiemmin luotua init.sls-tiedostoa. Poistettuani tiedoston aiemman sisällön, lisäsin sille seuraavan sisällön: 
```
tasks:
  cmd.run:
    - name: echo 'Hello World!'
```
