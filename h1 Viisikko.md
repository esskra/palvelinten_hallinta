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

<img width="464" alt="Näyttökuva 2023-10-30 100123" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/77414ca9-f0fa-4f04-8c80-6591c3ce6c0b">

Komento asentaa koneelle tree-palvelun. Tulosteen changes-kohdasta näkyy asennettu versio. Succeeded-kohta kertoo funktion onnistuneen. 

<b>pkg.removed</b>
Komento poistaa koneelle asennetun tree-palvelun. Tuloste toimii samalla tavalla kuin pkg.installed, eli changes-kohta kertoo poistetun version ja succeeded=1 kertoo funktion ajon onnistuneen.

## 2. file
<b>file.managed</b>

<img width="464" alt="Näyttökuva 2023-10-30 100622" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/f8524ee3-98eb-477e-97f9-7d6b9598945a">

Tuloste kertoo, että tiedosto "hellotero" on luotu onnistuneesti hakemistoon /tmp. 

<b>file.managed contents</b>
Funktio luo uuden tiedoston "moitero" jonka sisältönä on teksti "foo". Contents määrittää tiedoston sisällön. Alla olevassa kuvassa tarkistettu, että tiedoston sisältö on oikeasti luotu. Käytin komentoa sudo salt-call --local -l info state.single file.managed /tmp/moitero contents="foo". 

<img width="239" alt="Näyttökuva 2023-10-30 100819" src="https://github.com/esskra/palvelinten_hallinta/assets/148875302/70850ec9-43bd-46c7-afc7-e3f82180a465">

<b>file.absent</b>
Funktio poistaa valitun tiedoston. Käytin komentoa sudo salt-call --local -l info state.single file.absent /tmp/hellotero, ja se poisti /tmp-hakemistosta "hellotero" tiedoston. 


