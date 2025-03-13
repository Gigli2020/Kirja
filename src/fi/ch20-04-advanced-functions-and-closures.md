## Kehittyneet funktiot ja sulkeiset

Tässä osiossa tarkastelemme kehittyneitä ominaisuuksia, jotka liittyvät funktioihin ja sulkeisiin,
mukaan lukien funktiomerkintätyypit (`fn`-tyyppi) ja sulkeisten palauttaminen funktioista.

### Funktiomerkintätyypit (`fn`-tyyppi)

Aiemmin käsittelimme, kuinka sulkeisia voi välittää funktioille. Voit myös välittää tavallisia funktioita funktioille! Tämä on hyödyllistä, kun haluat käyttää jo määriteltyä funktiota sen sijaan, että loisit uuden sulkeisen. Funktiot muuttuvat automaattisesti tyypiksi `fn` (pienellä alkukirjaimella), jota ei pidä sekoittaa `Fn`-traitiin. `fn`-tyyppiä kutsutaan _funktiomerkintätyypiksi_.

Seuraavassa esimerkissä määrittelemme `add_one`-funktion, joka lisää annetun arvon yhdellä. `do_twice`-funktio ottaa kaksi parametria: funktiomerkintätyypin, joka ottaa yhden `i32`-tyyppisen arvon ja palauttaa `i32`, sekä yhden `i32`-arvon. `do_twice` kutsuu parametrina saatua funktiota kahdesti ja laskee summan:

```rust
{{#rustdoc_include ../listings/ch20-advanced-features/listing-20-28/src/main.rs}}
```

Tämä tulostaa `The answer is: 12`. `do_twice`-funktion parametri `f` on määritelty `fn`-tyyppiseksi, mikä tarkoittaa, että sitä voidaan kutsua `do_twice`-funktion sisällä.

Toisin kuin sulkeiset, `fn` on tyyppi eikä trait, joten voimme käyttää sitä suoraan parametrin tyyppinä ilman geneeristä trait-sidontaa.

Funktiomerkintätyypit toteuttavat kaikki kolme sulkeistraitia (`Fn`, `FnMut` ja `FnOnce`), joten funktiomerkintöjä voi käyttää aina siellä, missä sulkeisia odotetaan. Yleensä on kuitenkin hyvä tapa käyttää geneeristä tyyppiä ja `Fn`-traitia, jotta funktiot voivat vastaanottaa sekä tavallisia funktioita että sulkeisia.

Joskus on hyödyllistä hyväksyä vain `fn`-tyyppejä, mutta ei sulkeisia. Tämä voi olla tarpeen esimerkiksi, kun käytät ulkoisia kieliä, kuten C:tä, jossa funktiot voidaan välittää parametreina, mutta sulkeisia ei ole.

Seuraavassa esimerkissä näytetään, kuinka `map`-metodia voidaan käyttää joko sulkeisen tai nimettyä funktiota käyttäen:

```rust
{{#rustdoc_include ../listings/ch20-advanced-features/no-listing-15-map-closure/src/main.rs:here}}
```

Voimme myös käyttää `map`-funktiota nimetyllä funktiolla:

```rust
{{#rustdoc_include ../listings/ch20-advanced-features/no-listing-16-map-function/src/main.rs:here}}
```

Koska `to_string` on saatavilla useista eri konteksteista, käytämme täysin määriteltyä syntaksia sen kutsumiseen. Tässä käytämme `ToString`-traitin `to_string`-metodia, joka on toteutettu kaikille `Display`-traitin toteuttaville tyypeille.

Lisäksi `enum`-varianttien nimet toimivat myös alustusfunktioina, joita voidaan käyttää funktiomerkintöinä:

```rust
{{#rustdoc_include ../listings/ch20-advanced-features/no-listing-17-map-initializer/src/main.rs:here}}
```

Tässä luomme `Status::Value`-instanssit `map`-funktion sisällä käyttämällä `Status::Value`-varianttia funktiomerkintänä.

### Sulkeisten palauttaminen funktioista

Sulkeiset toteutetaan Rustissa traitien avulla, joten niitä ei voi palauttaa suoraan funktioista. Useimmissa tapauksissa, joissa haluat palauttaa traitin, voit sen sijaan käyttää konkreettista tyyppiä, joka toteuttaa kyseisen traitin. Sulkeisten tapauksessa tämä on kuitenkin harvoin mahdollista, koska niillä ei yleensä ole konkreettista tyyppiä, joka voidaan palauttaa.

Rust ei salli funktiomerkinnän `fn` käyttöä paluuarvona, jos sulkeinen kaappaa arvoja ympäristöstään.

Sen sijaan voimme käyttää `impl Trait` -syntaksia:

```rust
{{#rustdoc_include ../listings/ch20-advanced-features/no-listing-18-returns-closure/src/lib.rs}}
```

Jos haluat työskennellä useiden eri sulkeisten kanssa, jotka jakavat saman funktion allekirjoituksen mutta eri toteutukset, voit käyttää trait-olioita (`dyn Trait`):

```rust,noplayground
{{#rustdoc_include ../listings/ch20-advanced-features/no-listing-19-returns-closure-trait-object/src/main.rs}}
```

Tämä koodi toimii, mutta se ei toimisi, jos yrittäisimme käyttää `impl Fn(i32) -> i32` -paluuarvoa, koska jokaisella sulkeisella on oma erillinen tyyppinsä.

Seuraavaksi tarkastelemme makroja!

