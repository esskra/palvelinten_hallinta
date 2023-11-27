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
- <i>/etc/</i>, hakemisto, josta muutoksia etsitään.
- <i>printf</i> tulostaa muotoillun datan.
- <i>%T+</i> kertoo tiedoston muokkausajan.
- <i>%p</i> kertoo tiedostonimen ja polun.
- <i>\n</i> asettelee tulostuksen omille riveilleen.
- <i>|sort</i> järjestää tulostettavat rivit siten, että viimeisin muutos näkyy alimmaisena.

Tämän jälkeen oli vuorossa selvittää kotihakemiston viimeisimmät muutokset tiedostoihin. Koska olin jo valmiiksi kotihakemistossa, riitti tällä kertaa komento ``$ find -printf "%T+ %p\n"|sort``, eli tällä kertaa ei tarvinnut erikseen kertoa minkä hakemiston sisältöä haetaan. Alla komennon antama tulostus.

<img width="384" alt="Näyttökuva 2023-11-26 181942" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ec7aa14b-8272-4afd-bf62-c5dd7f521249">

## b) Gui2fs
Tässä tehtävässä oli tarkoitus tehdä muutoksia graafisen käyttöliittymän kautta ja tarkistaa tehdyt muutokset tiedostojärjestelmästä. Siirryin tätä tehtävää varten aiemmin luodulle Debian 12 virtuaalikoneelle.<br>
Avasin desktopin tiedostoista <i>Documents</i>-kansion, ja loin sinne uuden <i>testitiedosto.txt</i> nimisen tiedoston. 

<img width="347" alt="Näyttökuva 2023-11-27 001059" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/ef5b6408-14b6-4acc-8808-b824ff9944f5">

Luotuani tiedoston graafisen käyttöliittymän kautta, siirryin terminaaliin tarkistamaan tehdyt muutokset. Koska loin tiedoston <i>Documents</i> kansioon, siirryin tietenkin kansioon terminaalissa komennolla ``$ cd Documents``. Tämän jälkeen viime tehtävästä tuttu ``$ find -printf "%T+ %p\n"|sort`` komento. Tulosteesta selviää, että viimeisin muutos koskee graafisen käyttöliittymän kautta luotua <i>testitiedosto.txt</i> tiedostoa. 

<img width="374" alt="Näyttökuva 2023-11-27 001410" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/021d5f8e-a38a-4a9e-b0b7-ad55ef4ecf88">

## c) Komennus
Tässä tehtävässä tarkoituksena oli luoda Salt-tila, joka asentaa järjestelmään uuden komennon. Käytin mallina viime luennolla talteen ottamaani kuvakaappausta Teron tekemästä demosta.
Aloitin tehtävän luomalla ensin käsin shell scriptin komenolla ``$ nano tervehdys``. Halusin tehdä yksinkertaisen skriptin,  joka tervehtii käyttäjää ja kertoo silloisen päivän. Lisäsin tiedostoon seuraavat komennot: 

```
#!/usr/bin/bash
echo Tervehdys "$USER"!
date +"Tänään on: %d.%m.%Y"
```
<br>

<img width="275" alt="Näyttökuva 2023-11-27 012537" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/b5b61428-b5a8-4d22-979c-bf8664eb3e30">

Tiedoston luomisen jälkeen testasin sen toimivuuden, ja annoin sille ajo-oikeudet komennolla ``$ chmod +x tervehdys``. Varmistin vielä, että oikeudet olivat oikein komennolla ``$ ls -l | grep tervehdys``. Tilanne näytti siltä miltä pitikin, eli omistajalla kaikki oikeudet, groupeilla ja muilla vain read ja execute- oikeudet.

<img width="407" alt="Näyttökuva 2023-11-27 012730" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/af8496da-dc06-473d-a40f-6aca1ff608de">

Lopuksi siirsin skriptitiedoston /usr/local/bin- hakemistoon komennolla ``$ sudo cp tervehdys /usr/local/bin``, ja lähdin testaamaan, toimiiko komento ``$ tervehdys``. Alla olevasta kuvasta näkee, että komennon ajo sujui onnistuneesti, vaikka päivämäärä näyttikin ihan mitä sattuu.

<img width="374" alt="Näyttökuva 2023-11-27 012847" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/3014a73b-c5da-4aa5-877f-c96518bff068">

Kun olin saanut homman toimimaan käsin, pääsin siirtymään automatisointiin. Siirryin /srv/salt- hakemistoon, jonne loin uuden hakemiston komennolla ``$ mkdir tervehdys``, ja loin sinne uuden saman nimisen tiedoston komennolla ``$ sudo nano tervehdys``. Lisäsin tiedostoon saman skriptin kuin tämän tehtävän aikaisemmassa vaiheessa.
Luotuani skriptitiedoston siirryin luomaan <i>init.sls</i> tiedostoa, joka sisältää tarvittavat tiedot tilan ajamista varten. Lisäsin tiedostoon seuraavan sisällön:

```
/usr/local/bin/tervehdys:
  file.managed:
    - source: salt://tervehdys/tervehdys
    - mode: "0755"
```

<img width="280" alt="Näyttökuva 2023-11-27 020156" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/5a09b49e-8d56-443a-81bc-749f3a11118f">

Tiedoston luotuani pääsin testaamaan, toimiiko tilan ajaminen. Ajoin komennon `` $ sudo salt 't001' state.apply tervehdys`` luodakseni aiemmin tehdyn skriptitiedoston t001-orjalle. Tuloste kuitenkin kertoi tilan ajamisen epäonnistuneen. Kauan ei tarvinnut pohtia mikä oli pielessä, sillä olin unohtanut lisätä yhden kaksoispisteen. 

<img width="442" alt="Näyttökuva 2023-11-27 015945" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9593dd4b-f14f-4f73-8615-7963eca79946">

Kävin nanossa korjaamassa tilanteen ja ajoin tilan uudestaan. Tällä kertaa homma toimi niin kuin pitikin, ja tuloste kertoi tilan ajon onnistuneen.

<img width="465" alt="Näyttökuva 2023-11-27 015933" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/1f3ee2d0-b148-4109-8f97-1a782ef5343b">

 Lopuksi kävin vielä tarkistamassa, että tiedosto on varmasti luotu t001-orjalle. Ensin listasin orjan /usr/local/bin hakemiston sisällön komennolla ``$ sudo salt 't001' cmd.run 'ls /usr/local/bin'``. <i>tervehdys</i>-tiedosto löytyi sieltä mistä pitikin, joten testasin vielä sen toimivuuden komennolla ``$ sudo salt 't001' cmd.run 'tervehdys'``. Hyvin toimii!

<img width="469" alt="Näyttökuva 2023-11-27 020139" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/255eacf9-795f-4fb8-be8a-d9cc78fc7701">


## d) Apassi

## e) Ämpärillinen. 

# Lähteet:


