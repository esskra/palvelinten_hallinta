# h5 CSI Kerava
Tässä raportissa kerron vastaukseni Palvelinten Hallinta- kurssin tehtävään 'h5 CSI Kerava'. Tehtävät on suoritettu itsenäisesti 26.11.2023.
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
Tässä tehtävässä tarkoituksena oli luoda Salt-tila, joka asentaa järjestelmään uuden komennon. Tehtävää varten siirryin takaisin Vagrantin puolelle. Käytin mallina viime luennolla talteen ottamaani kuvakaappausta Teron tekemästä demosta. 
Aloitin tehtävän luomalla ensin käsin shell scriptin komenolla ``$ nano tervehdys``. Halusin tehdä yksinkertaisen skriptin,  joka tervehtii käyttäjää ja kertoo senhetkisen päivän. Lisäsin tiedostoon seuraavat komennot: 

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
Tämän tehtävän tarkoituksena oli asentaa Salt-tila, joka asentaa Apachen näyttämään kotihakemistoja. Tässä tehtävässä käytin apuna Teron <i>[Apache User Homepages Automatically – Salt Package-File-Service Example](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/)</i> ohjetta. 
Sirryin /srv/salt hakemistoon, ja loin sinne uuden <i>apache2</i> hakemiston komennolla ``$ sudo mkdir apache2``. Siirryin luomaani <i>apache2</i>- hakemistoon, ja loin sinne uuden html-tiedoston komennolla ``$ sudo nano apache2.html``. Alla tiedostoon lisäämäni sisältö.

<img width="287" alt="Näyttökuva 2023-11-27 095839" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0df56709-093a-4d76-9177-4173c99a7939">

Html-tiedoston luomisen jälkeen loin vielä <i>init.sls</i>- tiedoston komennolla ``$ sudo nano init.sls``. Lisäsin sinne ohjeiden mukaiset konfiguraatiot, jossa vaihdoin vain tiedoston nimet oikeiksi:

```
apache2:
 pkg.installed
/var/www/html/index.html:
 file.managed:
   - source: salt://apache2/apache2.html
/etc/apache2/mods-enabled/userdir.conf:
 file.symlink:
   - target: ../mods-available/userdir.conf
/etc/apache2/mods-enabled/userdir.load:
 file.symlink:
   - target: ../mods-available/userdir.load
apache2service:
 service.running:
   - name: apache2
   - watch:
     - file: /etc/apache2/mods-enabled/userdir.conf
     - file: /etc/apache2/mods-enabled/userdir.load
```

<img width="294" alt="Näyttökuva 2023-11-27 095123" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f6fff607-79d8-4b3c-a6a1-9dda6703f297">

Lisäämässäni komentopätkässä <i>pkg.installed</i> kertoo, onko Apache2 asennettuna ja <i>file.managed</i> kertoo, että index.html tiedoston lähteenä onkin äsken luomani <i>apache2.html</i>-tiedosto. Muista konfiguraatioista en rehellisesti ainakaan tässä vaiheessa ymmärrä paljoakaan.
Joka tapauksessa seuraavaksi siirryin tilan ajoon t001-orjalle komennolla ``$ sudo salt 't001' state.apply apache2``. Tilan ajo onnistui, ja tuloste näytti seuraavaa:

<img width="492" alt="Näyttökuva 2023-11-27 095018" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a59c7d09-45a5-42d9-8bc2-1992ab1ea970">

<img width="497" alt="Näyttökuva 2023-11-27 095036" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/bd029745-f45a-4d14-a729-a1f9538c972c">

Succeeded: 5 (changed=4) kertoo, että kaikki tilat ovat ajettu onnistuneesti, mutta yhteen ei ole tehty muutoksia. Tässä tapauksessa muutoksia ei tapahtunut <i>pkg.installed</i> komennolla, sillä t001-orjalla oli jo Apache2 asennettuna. 
Lähdin vielä testaamaan, oliko <i>index.html</i> tiedoston sisältö tilan ajamisen jälkeen oikein. Nythän siis tiedostossa tulisi olla aikaisemmin luomani <i>apache2.html</i> tiedoston sisältö. Ajoin komennon ``$ sudo salt 't001' cmd.run 'cat /var/www/html/index.html`'``, ja komento tulosti t001 orjan <i>index.html</i> tiedoston sisällön. Tuloste näyttää juuri siltä miltä pitääkin!

<img width="472" alt="Näyttökuva 2023-11-27 095317" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d2aed081-1ea0-43c1-838d-815b84bded61">

## e) Ämpärillinen. 
Tässä tehtävässä loin Salt-tilan, joka asentaa järjestelmään kansiollisen komentoja. 
Aloitin siirymällä /srv/salt-hakemistoon, ja loin sinne uuden hakemiston komennolla ``$ sudo mkdir ämpäri``. Siirryin <i>ämpäri</i>- hakemistoon, ja lisäsin sinne muutaman tiedoston ``$ sudo touch`` komennolla. Ajattelin ensin jättäväni tiedostot tyhjiksi, mutta päädyin lisäämään jokaiseen hyvin yksinkertaisen scriptin. 

<img width="245" alt="Näyttökuva 2023-11-27 103820" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/cad373b3-1aa3-439d-8e81-2a4438dd7066">

Luotuani tyhjät tiedostot hakemistoon, siirryin luomaan sinne <i>init.sls</i> tiedostoa komennolla ``$ sudo nano init.sls``.
Lisäsin tiedostoon seuraavan sisällön: 
```
/usr/local/bin/Aaaa:
  file.managed:
    - source: salt://ämpäri/Aaaa
    - mode: "0755"

/usr/local/bin/Beee:
  file.managed:
    - source: salt://ämpäri/Beee
    - mode: "0755"

/usr/local/bin/Ceee:
  file.managed:
    - source: salt://ämpäri/Ceee
    - mode: "0755"
```
<img width="275" alt="Näyttökuva 2023-11-27 103351" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/4b27d7b7-f16e-4463-ab2c-2a4ba96018f3">

Sitten kokeilemaan, toimiiko tilan ajo. Ajoin tilan taas t001-orjalle komennolla ``$ sudo salt 't001' state.apply ämpäri``. Tuloste näyttää vihreää, eli tilan ajo onnistui!

<img width="280" alt="Näyttökuva 2023-11-27 104153" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2cf78f2a-35e7-4680-832b-bd39dacf7c38">

Lopuksi testasin vielä tilojen ajoa orjalla komennolla ``$ sudo salt 't001' cmd.run 'Aaaa'``. Jokaisen skriptin ajaminen orjalla sujui onnistuneesti, jee!

<img width="434" alt="Näyttökuva 2023-11-27 104254" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9f76509c-ee8c-47ea-8cc5-b381771b3571">


# Lähteet:
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 26.11.2023.
- Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu 26.11.2023.
  


