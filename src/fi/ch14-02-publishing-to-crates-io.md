## Paketin julkaiseminen Crates.io:ssa

Olemme käyttäneet [crates.io](https://crates.io/)<!-- ignore -->-palvelun paketteja projektimme riippuvuuksina, mutta voit myös jakaa omaa koodiasi muille julkaisemalla omia pakettejasi. Crates.io:n pakettirekisteri jakaa pakettiesi lähdekoodin, joten se isännöi pääasiassa avoimen lähdekoodin projekteja.

Rust ja Cargo tarjoavat ominaisuuksia, jotka helpottavat julkaistun pakettisi löytämistä ja käyttöä. Käymme seuraavaksi läpi näitä ominaisuuksia ja selitämme, miten paketti julkaistaan.

### Hyödyllisten dokumentaatiokommenttien lisääminen

Pakettisi tarkka dokumentointi auttaa muita käyttäjiä ymmärtämään, miten ja milloin sitä tulisi käyttää, joten dokumentointiin kannattaa panostaa. Kuten luvussa 3 käsiteltiin, Rust-koodia voi kommentoida kahdella vinoviivalla `//`. Rustissa on myös erityinen _dokumentaatiokommentti_, joka tuottaa HTML-dokumentaatiota. Tämä dokumentaatio näyttää julkisen API:n dokumentaatiokommenttien sisällön ohjelmoijille, jotka haluavat _käyttää_ pakettiasi sen sijaan, että he keskittyisivät sen toteutuksen yksityiskohtiin.

Dokumentaatiokommentit käyttävät kolmea vinoviivaa `///` kahden sijaan ja tukevat Markdown-muotoilua. Ne sijoitetaan dokumentoitavan kohteen yläpuolelle. Alla olevassa esimerkissä näkyy dokumentaatiokommentti `add_one`-funktiolle paketissa nimeltä `my_crate`.

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-01/src/lib.rs}}
```

Tässä annetaan kuvaus siitä, mitä `add_one`-funktio tekee, lisätään otsikko `# Examples` ja annetaan esimerkki siitä, miten funktiota käytetään. Voimme luoda HTML-dokumentaation tästä kommentista suorittamalla komennon `cargo doc`. Tämä komento ajaa Rustin mukana tulevan `rustdoc`-työkalun ja sijoittaa luodun HTML-dokumentaation _target/doc_-hakemistoon.

Jos haluat avata luodun dokumentaation selaimessa, voit käyttää komentoa `cargo doc --open`. Tämä luo dokumentaation sekä nykyisestä paketistasi että sen kaikista riippuvuuksista ja avaa tuloksen selaimessa. Siirtymällä `add_one`-funktioon näet, miten dokumentaatiokommentit on renderöity.

#### Yleisesti käytetyt osiot

Markdown-otsikon `# Examples` avulla luomme HTML-dokumentaatioon osion nimeltä “Examples”. Tässä on joitakin muita osioita, joita pakettien tekijät käyttävät dokumentaatiossaan:

- **Panics**: Tilanteet, joissa dokumentoitu funktio voi kaatua. Funktiota kutsuvien ohjelmoijien kannattaa varmistaa, että he eivät kutsu funktiota näissä tilanteissa.
- **Errors**: Jos funktio palauttaa `Result`, voi olla hyödyllistä kuvata, millaisia virheitä saattaa ilmetä ja millaisissa tilanteissa ne syntyvät, jotta kutsujat voivat käsitellä ne oikein.
- **Safety**: Jos funktio on `unsafe`, tässä osiossa tulisi selittää, miksi se on turvaton ja mitkä ehdot kutsujan on täytettävä.

Useimmat dokumentaatiokommentit eivät tarvitse kaikkia näitä osioita, mutta tämä lista toimii hyvänä muistilistana.

#### Dokumentaatiokommentit testeinä

Dokumentaatiokommenteissa olevat koodiesimerkit auttavat havainnollistamaan, miten kirjastoasi käytetään, mutta ne tarjoavat myös lisäedun: `cargo test` suorittaa dokumentaation koodiesimerkit testeinä! Tämä varmistaa, että esimerkit pysyvät ajan tasalla ja toimivat oikein.

```text
   Doc-tests my_crate

running 1 test
test src/lib.rs - add_one (line 5) ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.27s
```

Jos muutat funktiota tai esimerkkiä niin, että `assert_eq!` epäonnistuu, `cargo test` ilmoittaa siitä.

### Crates.io-tilin luominen

Ennen kuin voit julkaista paketteja, sinun täytyy luoda tili [crates.io](https://crates.io/)<!-- ignore -->-palveluun ja hankkia API-avain. Mene Crates.io:n etusivulle ja kirjaudu sisään GitHub-tililläsi. Kun olet kirjautunut, käy tiliasetuksissasi osoitteessa [https://crates.io/me/](https://crates.io/me/)<!-- ignore --> ja kopioi API-avaimesi. Syötä se seuraavaan komentoon:

```console
$ cargo login
abcdefghijklmnopqrstuvwxyz012345
```

Tämä komento tallentaa API-avaimesi paikallisesti _~/.cargo/credentials_-tiedostoon. Älä jaa avaintasi muille.

### Metatiedon lisääminen uuteen pakettiin

Ennen julkaisua sinun täytyy lisätä metatietoja paketin _Cargo.toml_-tiedostoon.

Paketillasi täytyy olla yksilöllinen nimi, joka ei ole jo käytössä Crates.io:ssa. Kun olet valinnut nimen ja varmistanut, että se on vapaa, lisää se _Cargo.toml_-tiedoston `[package]`-osioon:

```toml
[package]
name = "guessing_game"
```

Lisäksi paketti tarvitsee kuvauksen ja lisenssin:

```toml
[package]
name = "guessing_game"
license = "MIT"
```

Jos haluat käyttää useita lisenssejä, voit kirjoittaa esimerkiksi `MIT OR Apache-2.0`.

### Paketin julkaiseminen Crates.io:hon

Kun kaikki on valmista, voit julkaista paketin komennolla:

```console
$ cargo publish
```

Julkaiseminen on pysyvää: julkaistua versiota ei voi poistaa tai korvata. Tämä varmistaa, että kaikki projektit, jotka käyttävät pakettiasi, voivat edelleen kääntyä ilman keskeytyksiä.

### Uuden version julkaiseminen

Jos päivität pakettiasi, muuta `version`-arvo _Cargo.toml_-tiedostossa ja suorita `cargo publish` uudelleen.

### Version poistaminen käytöstä `cargo yank` -komennolla

Vaikka julkaistua versiota ei voi poistaa, voit estää uusia projekteja käyttämästä sitä `cargo yank` -komennolla:

```console
$ cargo yank --vers 1.0.1
```

Jos haluat peruuttaa yankingin, käytä `--undo`-valitsinta:

```console
$ cargo yank --vers 1.0.1 --undo
```

Näin voit estää ongelmallisten versioiden käytön uusissa projekteissa ilman, että vanhat projektit rikkoutuvat.

