# h3 Versio
Tässä raportissa kerron vastaukseni Palvelinten Hallinta-kurssin tehtävään h3 Versio. Tehtävät ovat suoritettu osiota <br><i>g) Se toinen järjestelmä</i> lukuunottamatta omalla Windows-koneellani, ja sitä varten latasin Git Bash ohjelmiston [täältä](https://gitforwindows.org/). Asensin ohjelmiston Windowsille sen omilla oletusasetuksilla. Viimeiseen osioon on käytetty aiemmin kurssilla asennettua Debian 12-virtuaalikonetta. Tehtävät ovat suoritettu osittain yhdessä [Thomas Helmisen](https://github.com/ThomasHelminen) sekä [Joonas Hautaviidan](https://github.com/hautadata) kanssa.

## a) Online
Tässä osiossa tarkoituksena oli luoda uusi repository, jota tarvitaan <i>h3 Versio</i>-tehtävän muissa osioissa. Uuden varaston luominen oli jo tuttua hommaa edellisistä tehtävistä. Loin Githubiin verkossa repositoryn <i>wintergit</i> tehtävänannon mukaisesti. Lisäsin repositorylle README.md-tiedoston sekä GNU General Public License v3.0-lisenssin. 

<img width="411" alt="Näyttökuva 2023-11-09 174901" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d6cca8d2-cfb0-4c96-abf9-a87897f27238">

<img width="682" alt="Näyttökuva 2023-11-09 175022" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7fc8e2e9-334d-4826-9660-aa48d13e7faa">

Varaston luomisen jälkeen varmistin, että lisenssi ja README.md-tiedosto oli lisätty onnistuneesti. 

## b) Dolly
Tässä osiossa tarkoituksena oli kloonata edellisessä harjoituksessa tehty varasto sekä tehdä sille muutoksia terminaalin kautta ja pushata muutokset Githubiin. 
Esimmäisenä tarkistin Windowsin komentokehotteesta komennolla ``$ git -v``, että Git Bash-ohjelmisto oli asentunut oikein, jotta voisin aloittaa harjoituksen suorittamisen.

<img width="198" alt="Näyttökuva 2023-11-09 183332" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ae1ed558-bdf7-40b2-9e87-4f8c324262fb">

Tarkastettuani, että Git Bash oli asentunut oikein, siirryin Githubiin kopioimaan <i>wintergit</i>-varastoni SSH-osoitetta. Linkkiä kopioidessani muistin kuitenkin, että en ollut vielä luonut tarvittavia avaimia harjoitusta varten, sillä Github kertoi, ettei profiilissani ollut vielä yhtäkään julkista SSH-avainta. 

<img width="297" alt="Näyttökuva 2023-11-08 185828" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a1d911d0-39a9-4740-8851-bdaadad92dd6">


Siirryin Git Bashiin luomaan 







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
Gitin asensin terminaalissa komennolla ``$ sudo apt-get install git``. Varmistin vielä asennuksen onnistuneen komennolla ``$ git -v``. Alla olevasta kuvasta näkyy, että asennettu versio on 2.39.2.

<img width="196" alt="Näyttökuva 2023-11-08 185547" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f3fe9e51-ecbd-454d-a16d-21e21e0e3ab0">

Asennuksen jälkeen lähdin luomaan ``$ SSH-keygen``-komennolla tarvittavia SSH-avaimia. Komento luo sekä julkisen että yksityisen avaimen, joista julkinen lisätään GitHubin asetuksista käyttäjälle. 

<img width="468" alt="Näyttökuva 2023-11-09 215238" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/cb837110-a481-4609-ba59-7e20a99eddf8">

Tulosteesta näkee, että avaimet ovat lisätty .ssh-hakemistoon. Siirryin hakemistoon, ja tulostin julkisen avaimen sisältävän <i>id_rsa.pub</i>-tiedoston sisällön. Kopioin avaimen, siirryin terminaalista desktopin puolelle, avasin nettiselaimen ja lisäsin SSH-avaimen profiilini Githubista valitsemalla <b>Settings</b> > <b>SSH and GPG keys</b> > <b>New SSH key</b>. 

<img width="485" alt="Näyttökuva 2023-11-09 215303" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/8c57b01c-ac09-4828-b32d-3e82de62d30b">

Lisättyäni SSH-avaimen profiiliini, palasin takaisin terminaalin puolelle, jossa muodostin yhteyden komennolla 
``$ SSH -T git@github.com``. Muodostettuani yhteyden kloonasin <i>wintergit</i>-varaston virtuaalikoneelleni komennolla 
``$ git clone git@github.com:esskra/wintergit.git``. Varaston osoite pysyi luonnollisesti samana, kuin tehtävän b-osiossa.


<img width="482" alt="Näyttökuva 2023-11-09 215356" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/c7c81d08-dedf-4348-973e-e74cabbd7ff0">

<img width="479" alt="Näyttökuva 2023-11-09 215432" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/46c836da-4795-4bf6-b42a-81074b64a84e">

Kloonattuani varaston siirryin <i>wintergit</i>-hakemistoon, ja tarkistin vielä, että aikaisemmin luodut tiedostot näkyivät hakemistossa. 

<img width="372" alt="Näyttökuva 2023-11-09 215459" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a725b525-8c84-41cb-940c-e7bf962fb4a6">

Varmistettuani, että kloonaus oli sujunut odotetusti, aloitin uuden tiedoston luomisen. Uusi tiedosto olisi tarkoitus  pushata Githubiin terminaalin kautta. Loin uuden tiedoston komennolla ``$ nano debianwintertest.md``. Alla olevassa kuvassa tiedoston sisältö. 

<img width="431" alt="Näyttökuva 2023-11-09 215755" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/03d07a7e-6fcf-41be-80ed-20630b1486ab">

Tiedoston luomisen jälkeen lähdin testaamaan, saanko pushattua tiedoston Githubiin. Käytin komentoja ``$ git init`` <br> ``$ git add .`` ja ``$ git commit -m "Testi Githubiin Debianilla``. Komento configuroi nimen ja sähköpostin automaattisesti, koska käytin samaa konetta kuin aikaisemmin tehdessäni harjoitusta Windowsilla. Lopuksi pushasin tiedoston Githubiin komennolla ``$ git push``.

<img width="486" alt="Näyttökuva 2023-11-09 215837" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/5442998b-31d2-44d3-9747-69ceabc48185">

<img width="471" alt="Näyttökuva 2023-11-09 215914" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ec127272-7182-4495-8560-e265a1b8531e">


Siirryin Githubiin tarkistamaan, oliko komento toiminut odotetulla tavalla. <i>Wintergit</i>-hakemistoon oli ilmestynyt aikaisemmin luomani debianwintertest.md, eli tiedoston luominen ja pushaaminen oli onnistunut. Halusin vielä lopuksi tarkistaa myös toimeni lokin kautta, ja ajoin taas komennon ``$ git log``. Tulosteesta näki, että viimeisimmän toimen olin tehnyt virtuaalikoneellani, sillä lokissa tekijänä näkyi aikaisemmin automaattisesti luotu käyttäjänimeni.

<img width="658" alt="Näyttökuva 2023-11-09 215936" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9ea1e5c6-a8ec-4d3a-ab9b-428e1755784f">

<img width="488" alt="Näyttökuva 2023-11-09 220030" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/60741d71-5047-4d3f-bd8c-31772a057415">

# Lähteet






