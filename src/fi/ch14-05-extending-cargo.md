## Cargon laajentaminen mukautetuilla komennoilla

Cargo on suunniteltu niin, että voit laajentaa sitä uusilla alikomennoilla ilman, että sinun tarvitsee muokata itse Cargoa. Jos jokin binääritiedosto sijaitsee `$PATH`-hakemistossasi ja sen nimi on `cargo-jotain`, voit suorittaa sen kuten minkä tahansa Cargon alikomennon käyttämällä komentoa `cargo jotain`. Tällaiset mukautetut komennot näkyvät myös, kun suoritat `cargo --list`. 

Mahdollisuus asentaa laajennuksia `cargo install` -komennolla ja käyttää niitä samalla tavalla kuin sisäänrakennettuja Cargo-työkaluja on erittäin kätevä osa Cargon suunnittelua!

## Yhteenveto

Koodin jakaminen Cargon ja [crates.io](https://crates.io/)<!-- ignore -->-palvelun kautta on yksi Rust-ekosysteemin vahvuuksista. Rustin standardikirjasto on pieni ja vakaa, mutta erillisten pakettien jakaminen, käyttöönotto ja parantaminen voidaan tehdä riippumatta itse kielen kehityksestä. 

Älä epäröi jakaa koodia, joka on sinulle hyödyllistä, [crates.io](https://crates.io/)<!-- ignore -->-palvelussa – todennäköisesti se on hyödyllistä myös muille!
