## h1 Viisikko
x) Verkkosivun luominen GitHubilla. 
- Verkkosivua varten tarvitaan repository. Repositorya luodessa sinne täytyy lisätä myös README-tiedosto.
- Repositoryyn luodaan tiedosto, mistä tulee verkkosivu. Verkkosivun tiedoston tulee olla .md- päätteinen.

Aja salt-komento paikallisesti 
- Saltia käytetään normaalisti verkon slave-koneiden hallintaan 
- Saltin avulla voi ajaa komennot paikallisesti ja nähdä myös niiden tulokset heti
- Saltin tärkeimmät tilafunktiot ovat pkg, file, service, user ja cmd.

## a) Asenna Salt (Salt-minion) koneellesi.
Saltin asennuksen jälkeen tarkistin, että asennus oli onnistunut. Käytin sen tarkistamiseen sudo salt-call --version komentoa, joka antoi tulokseksi asennetun version: salt-call 3006.4 (Sulfur)

## b) Viisi tärkeintä
## 1. pkg
<b>pkg. installed</b>

<img width="464" alt="image" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/d76da5f0-9c7d-4747-8151-c067be8a72ee">

Komento asentaa koneelle tree-palvelun. Tulosteen changes-kohdasta näkyy asennettu versio. Succeeded-kohta kertoo funktion onnistuneen. 

<b>pkg.removed</b>

Komento poistaa koneelle asennetun tree-palvelun. Tuloste toimii samalla tavalla kuin pkg.installed, eli changes-kohta kertoo poistetun version ja succeeded=1 kertoo funktion ajon onnistuneen.

## 2. file
<b>file.managed</b>

<img width="464" alt="Näyttökuva 2023-10-30 100622" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f8524ee3-98eb-477e-97f9-7d6b9598945a">

Tuloste kertoo, että tiedosto "hellotero" on luotu onnistuneesti hakemistoon /tmp. 

<b>file.managed contents</b>

Funktio luo uuden tiedoston "moitero" jonka sisältönä on teksti "foo". Contents määrittää tiedoston sisällön. Käytin komentoa sudo salt-call --local -l info state.single file.managed /tmp/moitero contents="foo". Alla olevassa kuvassa tarkistettu, että tiedoston sisältö on oikeasti luotu. 

<img width="239" alt="Näyttökuva 2023-10-30 100819" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/70850ec9-43bd-46c7-afc7-e3f82180a465">

<b>file.absent</b>

Funktio poistaa valitun tiedoston. Käytin komentoa sudo salt-call --local -l info state.single file.absent /tmp/hellotero, ja se poisti /tmp-hakemistosta "hellotero" tiedoston. 

## 3. service

<b>service.running</b>

<img width="464" alt="Näyttökuva 2023-10-30 101306" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/20f5a6eb-957a-4582-85b9-39e885eeab3c">

Funktiolla pystyy käynnistämään palveluita. Käytin komentoa sudo salt-call --local -l info state.single service.running apache2 enable=True. Sain ensin tulosteen, jossa kerrottiin, ettei käynnistys onnistunut koska apache2 ei ollut saatavilla. Asensin apache2 komennolla sudo apt-get install apache2, jonka jälkeen sain onnistuneesti ajettua funktion. 

<b>service.dead</b>

<img width="464" alt="Näyttökuva 2023-10-30 101500" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/2c938599-92f3-4f4d-929e-47004417ccb5">

Muuttamalla edellisen funktion muotoon service.dead enable=False, voidaan "tappaa", eli ajaa valittu palvelu alas. Tuloste kertoo taas funktion onnistuneen. 

## 4. user
<b>user.present</b>

<img width="464" alt="Näyttökuva 2023-10-30 101748" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/9eebc9f0-c4d1-4865-9150-ba41e0e8d609">


user.present funktio luo uuden käyttäjän. Kuvasta näkee, kuinka käyttäjä "terote08" on luotu onnistuneesti.

<b>user.absent</b>
Funktio poistaa käyttäjän. Tuloste kertoo, että aikaisemmin luotu käyttäjä ja group on poistettu onnistuneesti.

## 5. cmd
<b>cmd.run</b>

Käytin sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo" komentoa luodakseni /tmp/ hakemistoon tiedoston "foo". 

## c) Idempotentti. Anna esimerkki idempotenssista. 
Testasin idempotentin ilmenemistä yrittämällä ajaa saman komennon, kuin edellisen tehtävän kohdassa 5, jossa luotiin /tmp/ hakemistoon tiedosto "foo". Tuloste kertoo, että komento on ajettu, mutta tiedostoa ei luoda, koska se on jo olemassa. 

<img width="464" alt="Näyttökuva 2023-10-30 102433" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/a042519c-e64f-443c-b47f-4bb5f4c1f867">

## d) Tietoa koneesta. Kerää tietoja koneesta Saltin grains.items -tekniikalla. 
Käytin komentoa sudo salt-call --local grains.items. Komennon tuloste antoi kattavat tiedot koneesta. Mielenkiintoisena pidin erityisesti kohtaa, missä kerrottiin käyttöjärjestelmä sekä sen "kutsumanimi" Bookworm. Tämä oli mielestäni mielenkiintoista siksi, että omistamani RaspberryPi käyttää samalla pohjalla olevaa järjestelmää.

<img width="464" alt="image" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/0f5e3b11-75c0-4597-9f10-cb495ad3c2cc">


