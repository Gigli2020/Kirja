## Cargo-työtilat

Luvussa 12 rakensimme paketin, joka sisälsi sekä binäärikääntäjän että kirjastokääntäjän. Projektin kasvaessa kirjastokääntäjä voi muuttua liian suureksi, jolloin sen jakaminen useisiin kirjastoihin voi olla järkevää. Cargo tarjoaa ominaisuuden nimeltä _työtilat_ (_workspaces_), joka auttaa hallitsemaan useita samanaikaisesti kehitettäviä paketteja.

### Työtilan luominen

_Työtila_ on joukko paketteja, jotka jakavat saman _Cargo.lock_-tiedoston ja _target_-hakemiston. Luodaan projekti, jossa käytämme työtilaa – pidämme koodin yksinkertaisena, jotta voimme keskittyä työtilan rakenteeseen. Työtilan voi järjestää monella tavalla, mutta tässä esitellään yksi yleinen tapa. Työtilassa on yksi binääripaketti ja kaksi kirjastopakettia. Binääri tarjoaa päätoiminnallisuuden ja riippuu kahdesta kirjastosta. Yksi kirjasto sisältää `add_one`-funktion ja toinen `add_two`-funktion. Nä...

```console
$ mkdir add
$ cd add
```

Seuraavaksi luodaan _Cargo.toml_-tiedosto _add_-hakemistoon, joka määrittää työtilan. Tämä tiedosto ei sisällä `[package]`-osiota, vaan `[workspace]`-osion, jossa määritellään työtilan jäsenet. Lisäksi varmistamme, että käytämme Cargon uusinta `resolver`-algoritmia asettamalla sen arvoksi `"2"`.

<span class="filename">Tiedostonimi: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-01-workspace/add/Cargo.toml}}
```

Seuraavaksi luodaan `adder`-binääripaketti suorittamalla `cargo new` _add_-hakemistossa:

```console
$ cargo new adder
    Creating binary (application) `adder` package
      Adding `adder` as member of workspace at `file:///projects/add`
```

Kun suoritat `cargo new` työtilassa, uusi paketti lisätään automaattisesti työtilan _Cargo.toml_-tiedostoon `members`-osioon:

```toml
{{#include ../listings/ch14-more-about-cargo/output-only-01-adder-crate/add/Cargo.toml}}
```

Nyt voimme kääntää työtilan suorittamalla `cargo build`. Hakemistorakenne näyttää tältä:

```text
├── Cargo.lock
├── Cargo.toml
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

Työtilalla on yksi yhteinen _target_-hakemisto, johon kaikki käännetyt tiedostot sijoitetaan.

### Toisen paketin lisääminen työtilaan

Seuraavaksi luodaan uusi työtilan jäsen nimeltä `add_one`. Muokkaa _Cargo.toml_-tiedostoa lisäämällä _add_one_ `members`-listaan:

<span class="filename">Tiedostonimi: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/Cargo.toml}}
```

Luo nyt uusi kirjastopaketti nimeltä `add_one`:

```console
$ cargo new add_one --lib
    Creating library `add_one` package
      Adding `add_one` as member of workspace at `file:///projects/add`
```

Tiedostorakenne näyttää nyt tältä:

```text
├── Cargo.lock
├── Cargo.toml
├── add_one
│   ├── Cargo.toml
│   └── src
│       └── lib.rs
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

Lisätään `add_one`-funktio _add_one/src/lib.rs_-tiedostoon:

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/add_one/src/lib.rs}}
```

Nyt `adder` voi riippua `add_one`-kirjastosta. Lisää riippuvuus _adder/Cargo.toml_-tiedostoon:

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/adder/Cargo.toml:6:7}}
```

Cargo ei oletuksena yhdistä työtilan paketteja riippuvuuksiksi, joten ne on määriteltävä erikseen.

Seuraavaksi muokataan _adder/src/main.rs_-tiedostoa käyttämään `add_one`-funktiota:

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-07/add/adder/src/main.rs}}
```

Rakennetaan työtila suorittamalla `cargo build`:

```console
$ cargo build
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.22s
```

Aja binääri käyttäen `cargo run -p adder`:

```console
$ cargo run -p adder
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/adder`
Hello, world! 10 plus one is 11!
```

#### Ulkoisen paketin käyttäminen työtilassa

Työtilassa on vain yksi _Cargo.lock_ -tiedosto, mikä varmistaa, että kaikki paketit käyttävät samaa versiota riippuvuuksista. Jos lisäät `rand`-paketin sekä _adder/Cargo.toml_- että _add_one/Cargo.toml_-tiedostoon, Cargo varmistaa, että molemmat käyttävät samaa versiota:

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add/add_one/Cargo.toml:6:7}}
```

Rakennettaessa `cargo build` lataa ja kääntää `rand`-kirjaston koko työtilalle.

#### Testin lisääminen työtilaan

Lisätään testi `add_one`-funktiolle _add_one/src/lib.rs_-tiedostoon:

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add/add_one/src/lib.rs}}
```

Suoritetaan testit:

```console
$ cargo test
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running unittests src/lib.rs (target/debug/deps/add_one-93c49ee75dc46543)

running 1 test
test tests::it_works ... ok
```

Voit myös ajaa testit vain yhdelle paketille:

```console
$ cargo test -p add_one
```

### Johtopäätös

Työtilat helpottavat suurten projektien hallintaa jakamalla ne pienempiin osiin. Tämä tekee koodista helpommin ymmärrettävää ja ylläpidettävää. Jos julkaiset työtilan paketteja _crates.io_:ssa, jokainen paketti on julkaistava erikseen.

Harkitse työtilan käyttämistä, jos projektisi kasvaa – se tekee kehityksestä sujuvampaa!
