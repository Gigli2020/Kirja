## Koodin suorittaminen siivouksen yhteydessä `Drop`-traitin avulla

Toinen älykkäiden osoittimien kannalta tärkeä trait on `Drop`, jonka avulla voit määrittää, mitä tapahtuu, kun arvo poistuu laajuudesta. `Drop`-traitin toteuttaminen mahdollistaa esimerkiksi resurssien, kuten tiedostojen tai verkkoyhteyksien, vapauttamisen automaattisesti.

Esittelemme `Drop`-traitin älykkäiden osoittimien yhteydessä, koska sitä käytetään lähes aina älykkäitä osoittimia toteutettaessa. Esimerkiksi `Box<T>` vapauttaa keolla varaamansa tilan, kun se poistuu laajuudesta.

Joissakin ohjelmointikielissä ohjelmoijan on itse kutsuttava tiettyä koodia vapauttaakseen muistia tai muita resursseja, kuten tiedostokahvoja tai lukituksia. Jos tämä unohtuu, järjestelmä voi ylikuormittua ja kaatua. Rust mahdollistaa tietyn koodin suorittamisen automaattisesti aina, kun arvo poistuu laajuudesta, mikä estää resurssivuodot.

`Drop`-traitin toteuttaminen edellyttää `drop`-metodin määrittämistä, joka ottaa vastaan muuttuvan viittauksen `self`:iin. Alla olevassa esimerkissä (`Listing 15-14`) `CustomSmartPointer`-rakenne toteuttaa `Drop`-traitin ja tulostaa viestin, kun instanssi poistuu laajuudesta:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-14/src/main.rs}}
```

Koska `Drop` sisältyy Rustin preludiin, sitä ei tarvitse tuoda erikseen laajuuteen. Kun ohjelma päättyy, Rust kutsuu `drop`-metodia automaattisesti.

### Arvon vapauttaminen ennen sen poistumista laajuudesta `std::mem::drop`-funktiolla

Rust ei salli `drop`-metodin kutsumista suoraan. Jos yritämme muokata `main`-funktiota kutsumaan `drop`-metodia käsin (Listing 15-15), saamme käännösvirheen:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-15/src/main.rs:here}}
```

Käännös tuottaa seuraavan virheilmoituksen:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-15/output.txt}}
```

Tämä johtuu siitä, että Rust kutsuisi `drop`-metodia automaattisesti uudelleen funktion lopussa, mikä johtaisi _double free_ -virheeseen. Koska `Drop`-traitia ei voi kutsua suoraan, Rust tarjoaa `std::mem::drop`-funktion, jolla arvon voi vapauttaa ennen sen poistumista laajuudesta.

Seuraavassa esimerkissä (Listing 15-16) käytetään `std::mem::drop`-funktiota arvon vapauttamiseen ennen `main`-funktion päättymistä:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-16/src/main.rs:here}}
```

Tämän ohjelman tulostus näyttää seuraavalta:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-16/output.txt}}
```

Tuloste osoittaa, että `drop`-metodin koodi suoritetaan heti, kun `drop(c)` kutsutaan.

`Drop`-traitin avulla voidaan luoda erilaisia siivousmekanismeja, esimerkiksi oma muistinhallinta. Rustin omistajuusjärjestelmä huolehtii automaattisesti siivouksesta, joten ohjelmoijan ei tarvitse muistaa vapauttaa resursseja erikseen. Lisäksi Rust varmistaa, että `drop`-metodia ei kutsuta useammin kuin kerran arvon elinkaaren aikana.

Nyt kun olemme käsitelleet `Box<T>`-tyypin ja älykkäiden osoittimien perusominaisuuksia, tutustumme seuraavaksi muihin Rustin standardikirjaston älykkäisiin osoittimiin.
