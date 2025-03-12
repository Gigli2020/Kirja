## Rakennusten mukauttaminen julkaisuprofiilien avulla

Rustissa _julkaisuprofiilit_ ovat ennalta määriteltyjä ja muokattavissa olevia profiileja, joilla on erilaisia asetuksia. Näiden avulla ohjelmoija voi hallita paremmin koodin kääntämiseen liittyviä valintoja. Jokainen profiili määritellään itsenäisesti muista.

Cargo sisältää kaksi pääprofiilia: `dev`-profiilin, jota Cargo käyttää suoritettaessa `cargo build`, ja `release`-profiilin, jota Cargo käyttää suoritettaessa `cargo build --release`. `dev`-profiili on määritelty kehitykseen sopivilla oletusasetuksilla, kun taas `release`-profiililla on hyvät oletusasetukset julkaisua varten.

Nämä profiilinimet voivat olla tuttuja käännöksen tulosteesta:

```console
$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
$ cargo build --release
    Finished `release` profile [optimized] target(s) in 0.32s
```

`dev` ja `release` ovat siis eri profiileja, joita kääntäjä käyttää.

Cargo määrittää oletusasetukset jokaiselle profiilille, jos projektin _Cargo.toml_-tiedostoon ei ole lisätty erillisiä `[profile.*]`-osioita. Lisäämällä `[profile.*]`-osiot haluamillesi profiileille voit muuttaa niiden asetuksia. Esimerkiksi tässä ovat oletusarvot `opt-level`-asetukselle `dev`- ja `release`-profiileissa:

<span class="filename">Tiedostonimi: Cargo.toml</span>

```toml
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

`opt-level`-asetus määrittää, kuinka paljon Rust optimoi koodia, asteikolla 0–3. Optimointien lisääminen pidentää käännösaikaa, joten kehitysvaiheessa, kun käännöksiä suoritetaan usein, on parempi käyttää vähemmän optimointeja, jotta kääntäminen on nopeampaa, vaikka suorituskyky ei olisi paras mahdollinen. Tästä syystä `dev`-profiilin oletusarvo `opt-level`-asetukselle on `0`. Kun koodi on valmis julkaistavaksi, kannattaa panostaa pidempään käännösaikaan, koska käännetty ohjelma suoritetaan useita kertoja. Siksi `release`-profiilin oletusarvo `opt-level`-asetukselle on `3`.

Voit ohittaa oletusasetuksen lisäämällä uuden arvon _Cargo.toml_-tiedostoon. Esimerkiksi jos haluat käyttää optimointitasoa 1 kehitysprofiilissa, voit lisätä seuraavat rivit:

<span class="filename">Tiedostonimi: Cargo.toml</span>

```toml
[profile.dev]
opt-level = 1
```

Tämä korvaa oletusarvon `0`. Nyt, kun suoritat `cargo build`, Cargo käyttää `dev`-profiilin oletusasetuksia, mutta soveltaa myös omaa muutostasi `opt-level`-asetukseen. Koska `opt-level` on asetettu arvoon `1`, Cargo käyttää enemmän optimointeja kuin oletusarvo, mutta ei yhtä paljon kuin julkaisurakenteessa.

Täydellinen luettelo kunkin profiilin asetusvaihtoehdoista ja oletusarvoista löytyy [Cargon dokumentaatiosta](https://doc.rust-lang.org/cargo/reference/profiles.html).
