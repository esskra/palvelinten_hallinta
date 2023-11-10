# h3 Versio
Tässä raportissa kerron vastaukseni Palvelinten Hallinta-kurssin tehtävään h3 Versio. Tehtävät ovat suoritettu osiota <i>g) Se toinen järjestelmä</i> lukuunottamatta omalla Windows-koneellani, ja sitä varten latasin Git Bash ohjelmiston [täältä](https://gitforwindows.org/). Asensin ohjelmiston Windowsille sen omilla oletusasetuksilla. Viimeiseen osioon on käytetty aiemmin kurssilla asennettua Debian 12-virtuaalikonetta. 

## a) Online
Tässä osiossa tarkoituksena oli luoda uusi repository, jota tarvitaan <i>h3 Versio</i>-tehtävän muissa osioissa. Uuden varaston luominen oli jo tuttua hommaa edellisistä tehtävistä. Loin Githubiin verkossa repositoryn <i>wintergit</i> tehtävänannon mukaisesti. Lisäsin repositorylle README.md-tiedoston sekä GNU General Public License v3.0-lisenssin. 

<img width="411" alt="Näyttökuva 2023-11-09 174901" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d6cca8d2-cfb0-4c96-abf9-a87897f27238">


<img width="682" alt="Näyttökuva 2023-11-09 175022" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7fc8e2e9-334d-4826-9660-aa48d13e7faa">

Varaston luomisen jälkeen varmistin, että lisenssi ja README.md-tiedosto oli lisätty onnistuneesti. 

## b) Dolly
Ensimmäisenä tarkistin Windowsin komentokehotteesta komennolla ``$ git -v``, että Git Bash-ohjelmisto oli asentunut oikein.

<img width="198" alt="Näyttökuva 2023-11-09 183332" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ae1ed558-bdf7-40b2-9e87-4f8c324262fb">









## c) Doh! 
Tässä tehtävässä tarkoituksena oli poistaa varastoon tehdyt muutokset ennen ``commit``-komennon ajamista. Aloitin tehtävän muuttamalla README.md tiedoston sisältöä. Alla olevassa kuvassa näkyy tiedostolle tehdyt muutokset nano-tekstieditorilla. 

<img width="534" alt="Näyttökuva 2023-11-09 190001" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/085ae156-2703-41a0-9224-6132cd59bdba">

Ajoin komennon ``$ git reset --hard`` kumotakseni tekemäni muutokset, ilman niiden julkaisemista Githubiin. Päättelin, että komennon tuloste kertoi, mikä oli viimeisin tekemäni toimi, nyt kun edelliset muutokset oli kumottu. Tarkistin vielä komennon toimineen toivotulla tavalla tulostamalla tiedoston sisällön komennolla ``$ cat README.md``. Tuloste kertoi, että aiemmin tehdyt muutokset oli kumottu onnistuneesti.

<img width="530" alt="1" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f63ae9ba-fe93-428a-bc97-9dd512dec876">

## d) Tukki.
Lähdin tarkastelemaan lokia ja sen sisältöä ajamalla komennon ``$ git log``. Komento tulosti lokin viimeisimmistä toimista <i>wintergit</i>-varastossa. 

<img width="489" alt="Näyttökuva 2023-11-09 180857" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a4a06107-b750-407e-acb1-5364097021d2">

Tuloste kertoi, mitä muutoksia varastoon on tehty ja kuka muutokset on tehnyt. Päättelin myös, että varastoon verkossa tehdyt muutokset näkyy käyttäjänimeni <i>esskra</i> tekemiksi. Lähdin testaamaan, olinko päätellyt oikein luomalla verkossa <i>wintergit</i>-varastoon tiedoston githubfile.md. Tiedoston luomisen jälkeen ajoin uudestaan ``$ git log``-komennon, ja pystyin todeta, että vain verkossa luomani tiedostot näkyvät lokissa käyttäjänimeni <i>esskra</i> alla.

<img width="625" alt="Näyttökuva 2023-11-09 192323" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/4223ec37-1310-415c-a599-0e0c60b69237">

## g) Vapaaehtoinen: Se toinen järjestelmä
Tämän osion suoritin aiemmin kurssilla asentamallani Debian 12-virtuaalikoneella. Aloitin avaamalla koneen VirtualBoxin kautta, ja kirjauduttuani koneelle sisään avasin terminaalin. Terminaalissa ajoin ensimmäisenä ``$ sudo apt-get update``-komennon päivittääkseni pakettivarastot, jonka jälkeen päivitin paketit ``$ sudo apt-get upgrade``-komennolla. 
Gitin asensin terminaalissa komennolla ``$ sudo apt-get install git``. Varmistin vielä asennuksen onnistuneen komennolla ``$ git version``. Alla olevasta kuvasta näkyy, että asennettu versio on 2.39.2.

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/a4420f3b-c742-4bdd-a88c-8bb486316851)

Asennuksen jälkeen lähdin luomaan ``$ SSH-keygen``-komennolla tarvittavia SSH-avaimia. Komento luo sekä julkisen että yksityisen avaimen, joista julkinen lisätään GitHubin asetuksista käyttäjälle. 

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/27b28cad-6dc1-4759-9358-656d7cff296f)

Tulosteesta näkee, että avaimet ovat lisätty .ssh-hakemistoon. Siirryin hakemistoon, ja tulostin julkisen avaimen sisältävän <i>id_rsa.pub</i>-tiedoston sisällön. Kopioin avaimen ja lisäsin sen profiilini SSH-avaimeksi Githubista valitsemalla <b>Settings</b> > <b>SSH and GPG keys</b> > <b>New SSH key</b>. 

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/f637f112-c12b-46cb-a774-cdbe18f7c030)

Lisättyäni SSH-avaimen profiiliini, palasin takaisin terminaalin puolelle, jossa muodostin yhteyden komennolla 
``$ SSH -T git@github.com``. Muodostettuani yhteyden kloonasin <i>wintergit</i>-varaston virtuaalikoneelleni komennolla 
``$ git clone git@github.com:esskra/wintergit.git``. Varaston osoite pysyi luonnollisesti samana, kuin tehtävän b-osiossa.

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/f53953ce-b42c-4b44-8742-3e8a786e341d)

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/89f2a8ea-f012-4790-8497-b38df9ee7ca9)

Kloonattuani varaston siirryin <i>wintergit</i>-hakemistoon, ja tarkistin vielä, että aikaisemmin luodut tiedostot näkyivät hakemistossa. 

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/f20db2c7-eb3e-4132-ac7b-8dbc9f68aa6f)

Varmistettuani, että kloonaus oli sujunut odotetusti, aloitin uuden tiedoston luomisen. Uusi tiedosto olisi tarkoitus  pushata Githubiin terminaalin kautta. Loin uuden tiedoston komennolla ``$ nano debianwintertest.md``. Alla olevassa kuvassa tiedoston sisältö. 

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/6265886d-af67-48ac-a846-89947049e5af)

Tiedoston luomisen jälkeen lähdin testaamaan, saanko pushattua tiedoston Githubiin. Käytin komentoja ``$ git init`` <br> ``$ git add .`` ja ``$ git commit -m "Testi Githubiin Debianilla``. Komento configuroi nimen ja sähköpostin automaattisesti, koska käytin samaa konetta kuin aikaisemmin tehdessäni harjoitusta Windowsilla. Lopuksi pushasin tiedoston Githubiin komennolla ``$ git push``.

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/845d0088-8319-4652-9c19-a16c3b9e6b39)

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/9388fcbf-f076-434a-9835-bb607378994c)

Siirryin Githubiin tarkistamaan, oliko komento toiminut odotetulla tavalla. <i>Wintergit</i>-hakemistoon oli ilmestynyt aikaisemmin luomani debianwintertest.md, eli tiedoston luominen ja pushaaminen oli onnistunut.

![image](https://github.com/esskra/palvelinten_hallinta/assets/148875302/d56e6d9d-5999-466b-a02d-05a651f6888d)





