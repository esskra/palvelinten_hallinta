# h3 Versio
Tässä raportissa kerron vastaukseni Palvelinten Hallinta-kurssin tehtävään <i>h3 Versio</i>. Tehtävät ovat suoritettu omalla Windows-koneellani, ja sitä varten latasin Git Bash ohjelmiston [täältä](https://gitforwindows.org/). Asensin ohjelmiston Windowsille sen omilla oletusasetuksilla. 

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

