# h3 Versio
Tässä raportissa kerron vastaukseni Palvelinten Hallinta-kurssin tehtävään h3 Versio. Tehtävät ovat suoritettu osiota <br><i>"g) Se toinen järjestelmä"</i> lukuunottamatta omalla Windows-koneellani, ja sitä varten latasin Git Bash ohjelmiston [täältä](https://gitforwindows.org/). Asensin ohjelmiston Windowsille sen omilla oletusasetuksilla. Viimeiseen osioon on käytetty aiemmin kurssilla asennettua Debian 12-virtuaalikonetta. 
<br>Tehtävät on suoritettu osittain yhdessä [Thomas Helmisen](https://github.com/ThomasHelminen) sekä [Joonas Hautaviidan](https://github.com/hautadata) kanssa.

## a) Online
Tässä osiossa tarkoituksena oli luoda uusi repository, jota tarvitaan <i>h3 Versio</i>-tehtävän muissa osioissa. Uuden varaston luominen oli jo tuttua hommaa edellisistä tehtävistä. Loin Githubiin verkossa repositoryn <i>wintergit</i> tehtävänannon mukaisesti. Lisäsin repositorylle README.md-tiedoston sekä GNU General Public License v3.0-lisenssin. 

<img width="411" alt="Näyttökuva 2023-11-09 174901" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d6cca8d2-cfb0-4c96-abf9-a87897f27238">

<img width="682" alt="Näyttökuva 2023-11-09 175022" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7fc8e2e9-334d-4826-9660-aa48d13e7faa">

Varaston luomisen jälkeen varmistin, että lisenssi ja README.md-tiedosto oli lisätty onnistuneesti. 

## b) Dolly
Tässä osiossa tarkoituksena oli kloonata edellisessä harjoituksessa tehty varasto, tehdä sille muutoksia terminaalin kautta ja pushata muutokset Githubiin. Harjoituksen tekeminen oli melko hidasta ja tietoa joutui etsimään runsaasti verkosta, sillä minulla ei ollut lainkaan kokemusta Githubista tai Gitistä ennen kurssin alkua.
Esimmäisenä tarkistin Windowsin komentokehotteesta komennolla ``$ git -v``, että Git Bash-ohjelmisto oli asentunut oikein, jotta voisin aloittaa harjoituksen suorittamisen. Asennus oli onnistunut ja asennettu versio oli 2.42.0.windows.2. 

<img width="198" alt="Näyttökuva 2023-11-09 183332" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ae1ed558-bdf7-40b2-9e87-4f8c324262fb">

Tarkastettuani, että Git Bash oli asentunut oikein, siirryin Githubiin kopioimaan <i>wintergit</i>-varastoni SSH-osoitetta. Linkkiä kopioidessani muistin kuitenkin, että en ollut vielä luonut tarvittavia avaimia harjoitusta varten, sillä Github kertoi, ettei profiilissani ollut vielä yhtäkään julkista SSH-avainta. 

<img width="297" alt="Näyttökuva 2023-11-08 185828" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a1d911d0-39a9-4740-8851-bdaadad92dd6">

ssh-keygen -o -t rsa -C "windows-ssh@github" , jossa -o tarkoittaa OpenSSH-formaattia, -t RSA/SHA256-avaintyypin valintaa ja -C on kommentti

Siirryin Git Bashiin luomaan avainta. Käytin komentoa ``$ ssh keygen -o -t rsa -C`` avaimen luomiseen. Komennon tarkoitus olisi luoda sekä julkisen että yksityinen avain. Julkinen avain tulisi lisätä Githubin kautta profiiliini, jotta SSH-yhteyden luominen onnistuisi. Komennossa -o tarkoittaa formaatin olevan OpenSSH, -t avaintyypin valintaa ja -C kommenttia. En ollut itse tässä vaiheessa täysin varma, tarvitseeko komento erikseen parametrejä, vai voiko avaimen luoda pelkällä ``$ ssh keygen``- komennolla. 

<img width="512" alt="Näyttökuva 2023-11-09 141847" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d4048109-6f18-40d4-b366-767d5ca5081a">

Komennon ajettuani tarkistin, että avaimet sisältävät tiedostot oli luotu .ssh-hakemistooni. Tiedostot oli luotu onnistuneesti, joten ajoin terminaalissa komennon ``$ cat id_rsa.pub`` tulostaakseni tiedoston sisällön. Kopioin tiedoston sisältämän julkisen avaimen ja siirryin selaimessa Githubiin, josta lisäsin avaimen profiiliini <b>Settings</b> > <b>SSH and GPG keys</b> > <b>New SSH key</b> kautta. 

<img width="493" alt="Näyttökuva 2023-11-09 183817" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f474bd35-7906-43ac-9341-5fea83dcf539">

<img width="515" alt="Näyttökuva 2023-11-09 142735" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/acb8c0e2-01af-486c-bb70-ea0fd2ab1bf8">

Lisättyäni avaimen profiiliini pääsin luomaan SSH-yhteyttä Git Bashissa. SSH-yhteyttä varten käytin komentoa <br> ``$ ssh -T git@github``. ``$ man ssh``-komento kertoo, että komennon T- tarkoittaa "Disable pseudo-terminal allocation.". Se siis estää terminaalia yrittämästä avata SSH-yhteyden kanssa terminaalia, koska Githubilla ei sitä ole. SSH-yhteyden luomisen jälkeen pysyn siis oman koneeni terminaalin sisällä. Komento toimi toivotulla tavalla, ja tuloste kertoi että olin päässyt sisään käyttäjälleni <i>esskra</i>

<img width="489" alt="Näyttökuva 2023-11-09 175158" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/7007ef72-afdd-444d-8821-e55ee84990e6">

Nyt pystyin siirtymään varastoni kloonaamiseen. Käytin komentoa ``$ git clone git@github.com:esskra/wintergit.git``. Komennossa linkkinä oli varastoni SSH-osoite. Komento kloonasi varaston, ja tarkistin vielä komennon toimineeni siirtymällä terminaalissa wintergit-hakemistoon ja tulostamalla hakemiston sisällön ``$ ls``-komennolla. Hakemiston sisältä löytyi tässä vaiheessa varaston tekovaiheessa luotu lisenssitiedosto sekä README.md-tiedosto. 

<img width="421" alt="Näyttökuva 2023-11-09 175225" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/eca3e95b-9934-4a83-a180-41a4baa3fd1b">

<img width="329" alt="Näyttökuva 2023-11-09 175350" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b4b5d4be-33f4-4ca6-aea2-53f772c13685">

Kloonattuani varaston siirryin luomaan uutta tiedostoa hakemiston sisällä nano-tekstieditorilla. Uuden tiedoston nimeksi annoin <i>wintertestfile.md</i> ja loin sen komennolla ``$ nano wintertestfile.md``. 

<img width="310" alt="Näyttökuva 2023-11-09 175619" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/953bf149-bc6e-4c1a-b18f-f1a780872aa4">

Nyt kun tiedosto oli luotu, pääsin testaamaan sen pushaamista Githubiin. Ajoin ensin ``$ git init``-komennon, joka alustaa lisätyt tiedostot. Tämän jälkeen komento ``$ git add .`` joka lisää kaikki uudet muutokset tulevaa tiedostojen pushaamista varten. Jatkoin komennolla ``$ git commit``, joka tallentaa tehdyt muutokset pysyvästi. Lisäsin kommentin samalla komennolla käytämällä -m parametriä. Koska käytin komentoa ensimmäistä kertaa, jouduin määrittämään itselleni käyttäjänimen ja sähköpostin. Näiden määrittelyyn käytin komentoja ``$ git config --global user.email`` & ``$ git config --global user.name``.
Käyttäjänimen ja salasanan määrittelyn jälkeen ajoin komennon ``$ git push``, joka pushaa tiedostot Githubiin, eli tämä komento on se, joka tekee muutokset näkyväksi verkkoselaimella. 

<img width="482" alt="Näyttökuva 2023-11-09 175808" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/61f03770-7cce-46b5-b293-1cd377c98cba">

Kun olin ajanut ``$ git push``-komennon, siirryin selaimella Githubiin tarkistamaan, oliko tekemäni muutokset näkyvissä myös verkon kautta. Olin ajanut komennon onnistuneesti, sillä <i>wintergit</i>-hakemistossani näkyi aiemmin terminaalissa luomani tiedosto <i>wintertestfile.md</i> ja sille ``$ git commit`` vaiheessa antamani kommentti.

<img width="656" alt="Näyttökuva 2023-11-09 175829" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/200e55c1-95a0-46a7-82e6-ab1d1433d4f0">

Lopuksi kokeilin vielä muutosten tallentamista jo olemassaolevaan tiedostoon. Avasin terminaalissa README.md-tiedoston, ja lisäsin sille Nano-tekstieditorissa lisää sisältöä. Lisättyäni tiedostoon uutta sisältöä, ajoin jälleen samat <br>``$ git init``, ``$ git add .``, ``$ git commit`` ja ``$ git push``-komennot. Koska olin aiemmin jo määritellyt itselleni käyttäjänimen ja salasanan, ei sitä enää tässä vaiheessa tarvinnut tehdä uudestaan. Siirryin taas selaimen puolelle tarkastamaan muutokset, ja tekemäni muutokset näkyivät selaimessa antamani kommentin kera. Toisella kerralla muutosten pushaaminen tuntui jo melko helpolta!

<img width="477" alt="Näyttökuva 2023-11-09 180633" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/e235abe9-d64d-486d-9a2e-004adf436a76">

<img width="672" alt="Näyttökuva 2023-11-09 221312" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0c983282-c3eb-455f-b67b-28dbc77cfcad">

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
<br>``$ SSH -T git@github.com``. Muodostettuani yhteyden kloonasin <i>wintergit</i>-varaston virtuaalikoneelleni komennolla 
<br>``$ git clone git@github.com:esskra/wintergit.git``. Varaston osoite pysyi luonnollisesti samana, kuin tehtävän b-osiossa. Kloonattuani varaston siirryin <i>wintergit</i>-hakemistoon, ja tarkistin vielä, että aikaisemmin luodut tiedostot näkyivät hakemistossa. 

<img width="482" alt="Näyttökuva 2023-11-09 215356" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/c7c81d08-dedf-4348-973e-e74cabbd7ff0">

<img width="479" alt="Näyttökuva 2023-11-09 215432" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/46c836da-4795-4bf6-b42a-81074b64a84e">

<img width="372" alt="Näyttökuva 2023-11-09 215459" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a725b525-8c84-41cb-940c-e7bf962fb4a6">

Varmistettuani, että kloonaus oli sujunut odotetusti, aloitin uuden tiedoston luomisen. Uusi tiedosto olisi tarkoitus  pushata Githubiin terminaalin kautta. Loin uuden tiedoston komennolla ``$ nano debianwintertest.md``. Lisäsin nanossa tiedostolle sisällön ja tallensin sen.

<img width="431" alt="Näyttökuva 2023-11-09 215755" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/03d07a7e-6fcf-41be-80ed-20630b1486ab">

Tiedoston luomisen jälkeen lähdin testaamaan, saanko pushattua tiedoston Githubiin. Käytin komentoja ``$ git init`` <br> ``$ git add .`` ja ``$ git commit -m``. Komento configuroi nimen ja sähköpostin automaattisesti, koska käytin samaa konetta kuin aikaisemmin tehdessäni harjoitusta Windowsilla. Lopuksi pushasin tiedoston Githubiin komennolla <br>
``$ git push``.

<img width="486" alt="Näyttökuva 2023-11-09 215837" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/5442998b-31d2-44d3-9747-69ceabc48185">

<img width="471" alt="Näyttökuva 2023-11-09 215914" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ec127272-7182-4495-8560-e265a1b8531e">


Siirryin Githubiin tarkistamaan, oliko komento toiminut odotetulla tavalla. <i>Wintergit</i>-hakemistoon oli ilmestynyt aikaisemmin luomani debianwintertest.md, eli tiedoston luominen ja pushaaminen oli onnistunut. Halusin vielä lopuksi tarkistaa myös toimeni lokin kautta, ja ajoin taas komennon ``$ git log``. Tulosteesta näki, että viimeisimmän toimen olin tehnyt virtuaalikoneellani, sillä lokissa tekijänä näkyi aikaisemmin automaattisesti luotu käyttäjänimeni.

<img width="658" alt="Näyttökuva 2023-11-09 215936" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9ea1e5c6-a8ec-4d3a-ab9b-428e1755784f">

<img width="488" alt="Näyttökuva 2023-11-09 220030" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/60741d71-5047-4d3f-bd8c-31772a057415">

# Lähteet
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 10.11.2023.
- Github Docs. s.a. Cloning a repository. Luettavissa: https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository Luettu 10.11.2023
- Git for windows. s.a. Luettavissa: https://gitforwindows.org/ Luettu 10.11.2023
- McKenzie C. 11.1.2022. How to SSH into GitHub on Windows example. Luettavissa: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/GitHub-SSH-Windows-Example Luettu 10.11.2023
- Stack Overflow. 28.7.2013. What is Pseudo TTY-Allocation? (SSH and Github) Luettavissa: https://stackoverflow.com/questions/17900760/what-is-pseudo-tty-allocation-ssh-and-github Luettu 10.11.2023
- Atlassian. s.a. Add, edit, and commit to source files. Luettavissa: https://support.atlassian.com/bitbucket-cloud/docs/add-edit-and-commit-to-source-files/ Luettu 10.11.2023





