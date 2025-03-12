## Viitesyklit voivat aiheuttaa muistivuotoja

Rustin muistiturvatakuut tekevät vaikeaksi, mutta eivät mahdottomaksi, luoda muistia, jota ei koskaan vapauteta (_memory leak_ eli muistivuoto). Rust ei takaa muistivuotojen estämistä kokonaan, mikä tarkoittaa, että muistivuodot ovat muistiturvallisia Rustissa. Tämä voidaan todistaa käyttämällä `Rc<T>`- ja `RefCell<T>`-tyyppejä: on mahdollista luoda viittauksia, joissa kohteet viittaavat toisiinsa muodostaen syklin. Tämä aiheuttaa muistivuotoja, koska jokaisen syklissä olevan kohteen viittauslaskuri ei koskaan saavuta arvoa 0, eikä arvoja koskaan vapauteta.

### Viitesyklin luominen

Tarkastellaan, kuinka viitesykli voi syntyä ja miten sen voi estää, alkaen `List`-enum-tyypin ja `tail`-metodin määrittelystä kohdassa Lista 15-25:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-25/src/main.rs}}
```

Käytämme jälleen `List`-määrittelyn muunnelmaa, joka pohjautuu kohtaan Lista 15-5. `Cons`-variantin toinen elementti on nyt `RefCell<Rc<List>>`, mikä tarkoittaa, että emme enää halua muokata `i32`-arvoa, kuten kohdassa Lista 15-24, vaan haluamme muuttaa `Cons`-variantin osoittamaa `List`-arvoa. Olemme myös lisänneet `tail`-metodin, jotta voimme kätevästi käyttää toista elementtiä, jos kyseessä on `Cons`-variantti.

Kohdassa Lista 15-26 lisäämme `main`-funktion, joka käyttää määrittelyjä kohdasta Lista 15-25. Tämä koodi luo listan `a` ja toisen listan `b`, joka osoittaa `a`:han. Sitten se muuttaa `a`:ta osoittamaan `b`:hen, mikä luo viitesyklin. Koodissa on mukana `println!`-komentoja, jotka näyttävät viittauslaskurien arvot eri vaiheissa.

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-26/src/main.rs:here}}
```

...

(Koko tiedosto käännettynä)

