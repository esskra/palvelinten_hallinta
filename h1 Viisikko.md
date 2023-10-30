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
Komento asentaa koneelle tree-palvelun. Tulosteen changes-kohdasta näkyy asennettu versio. Succeeded-kohta kertoo komennon onnistuneen. 

pkg.removed

