## Kaikki paikat, joissa malleja voidaan käyttää

Rustin mallinmukaisuus on tehokas työkalu, ja sitä voidaan käyttää monissa eri paikoissa. Mallit eivät rajoitu vain `match`-lauseisiin, vaan niitä voidaan hyödyntää laajasti ohjelman eri osissa. Tässä luvussa tarkastelemme kaikkia paikkoja, joissa voit käyttää malleja.

### `match`-lauseet

Rustin `match`-lause on yksi monipuolisimmista tavoista käyttää malleja:

```rust
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-01/src/main.rs}}
```

### `if let`- ja `while let`-laus...

Malleja voidaan käyttää myös `if let`- ja `while let`-rakenteissa:

```rust
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-02/src/main.rs}}
```

### Funktioiden parametrit

Voimme käyttää malleja myös funktioiden parametreissa, jolloin voimme purkaa tietorakenteita suoraan:

```rust
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-03/src/main.rs}}
```

### `let`-lausunnot

Malleja voidaan hyödyntää `let`-lausunnoissa:

```rust
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-04/src/main.rs}}
```

### `for`-silmukat

Rust mahdollistaa myös mallien käytön `for`-silmukoissa:

```rust
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-05/src/main.rs}}
```

---

Tässä luvussa olemme nähneet, kuinka malleja voidaan käyttää monissa eri paikoissa Rustissa. Tämä tekee koodista selkeämpää ja tehokkaampaa, kun vältämme tarpeettoman toistuvan koodin.

