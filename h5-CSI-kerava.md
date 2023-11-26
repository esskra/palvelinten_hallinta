# h5 CSI Kerava
## x) Lue ja tiivistä.
<b>Karvinen 2018: [Apache User Homepages Automatically – Salt Package-File-Service Example](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/)</b>
  - Ensin käsin, vasta sitten automaattisesti. Automaatio on mahdollista tehdä vain, jos tiedät, miten asia tehdään käsin.
  - Tilakomennot oltava idempotenttisia.

## a) CSI Kerava. 
Tässä tehtävässä tarkoituksena oli tutustua tarkemmin <i>find</i>-komentoon, jonka avulla voi selvittää viimeisimimmäksi muokattuja tiedostoja. Lähdin suorittamaan tehtävää omalla koneellani Windowsin komentokehotteessa. Käynnistin Vagrantin komentokehotteessa hakemistossa <i>essi_vagrant</i> komennolla ``$ vagrant up``, jonka jälkeen muodostin yhteyden herrakoneeseen komennolla ``$ vagrant ssh``. <br>
Aloitin selvittämällä ensin /etc/-hakemiston viimeiseksi muokatut tiedostot käyttämällä tehtävänannossa vinkattua komentoa  ``$ find /etc/ -printf "%T+ %p\n"|sort``. 

<img width="389" alt="Näyttökuva 2023-11-26 181820" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/823a2c56-ea5d-49d5-b985-223501e67ae5">

Komento purettuna osiin: 
- <i>find</i> komentoa voidaan käyttää tiedostojen ja hakemistojen etsimiseen.
- <i>printf</i> tulostaa muotoillun datan.
- <i>%T+</i> kertoo tiedoston muokkausajan.
- <i>%p</i> kertoo tiedostonimen ja polun.
- <i>\n</i> asettelee tulostuksen omille riveilleen.
- <i>|sort</i> järjestää tulostettavat rivit siten, että viimeisin muutos näkyy alimmaisena.

Tämän jälkeen oli vuorossa selvittää kotihakemiston viimeisimmät muutokset tiedostoihin. Koska olin jo valmiiksi kotihakemistossa, riitti tällä kertaa komento ``$ find -printf "%T+ %p\n"|sort``, eli tällä kertaa ei tarvinnut erikseen kertoa minkä hakemiston sisältöä haetaan. Alla komennon antama tulostus.

<img width="384" alt="Näyttökuva 2023-11-26 181942" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ec7aa14b-8272-4afd-bf62-c5dd7f521249">

## b) Gui2fs

