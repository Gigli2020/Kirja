## Binäärien asentaminen `cargo install` -komennolla

`cargo install` -komento mahdollistaa binäärikääntäjien asentamisen ja käyttämisen paikallisesti. Tätä ei ole tarkoitettu korvaamaan järjestelmän pakettienhallintaa, vaan se on kätevä tapa Rust-kehittäjille asentaa työkaluja, joita muut ovat jakaneet [crates.io](https://crates.io/)<!-- ignore -->-palvelussa. Huomaa, että voit asentaa vain paketteja, joilla on binääritavoite. 

_Binääritavoite_ tarkoittaa ajettavaa ohjelmaa, joka syntyy, jos paketilla on _src/main.rs_-tiedosto tai jokin muu binääriksi määritelty tiedosto. Kirjastotavoitteet eivät ole suoraan ajettavissa, vaan ne on tarkoitettu käytettäväksi osana muita ohjelmia. Usein paketin _README_-tiedostossa on tietoa siitä, onko kyseessä kirjasto, binääri vai molemmat.

Kaikki `cargo install` -komennolla asennetut binäärit tallennetaan asennuspolun _bin_-hakemistoon. Jos asensit Rustin _rustup.rs_:n kautta etkä ole muuttanut asetuksia, tämä hakemisto on *$HOME/.cargo/bin*. Varmista, että tämä hakemisto on mukana järjestelmäsi `$PATH`-muuttujassa, jotta voit suorittaa asentamasi ohjelmat.

Esimerkiksi luvussa 12 mainittiin Rustilla toteutettu `grep`-työkalu nimeltä `ripgrep`, jota voidaan käyttää tiedostojen hakemiseen. `ripgrep` voidaan asentaa seuraavasti:

```console
$ cargo install ripgrep
    Updating crates.io index
  Downloaded ripgrep v14.1.1
  Downloaded 1 crate (213.6 KB) in 0.40s
  Installing ripgrep v14.1.1
--snip--
   Compiling grep v0.3.2
    Finished `release` profile [optimized + debuginfo] target(s) in 6.73s
  Installing ~/.cargo/bin/rg
   Installed package `ripgrep v14.1.1` (executable `rg`)
```

Lähes viimeinen rivi näyttää asennetun binäärin sijainnin ja nimen, joka `ripgrep`-tapauksessa on `rg`. Jos asennushakemisto on `$PATH`-muuttujassa, kuten aiemmin mainittiin, voit suorittaa komennon `rg --help` ja alkaa käyttää tätä nopeaa Rust-pohjaista hakutyökalua!
