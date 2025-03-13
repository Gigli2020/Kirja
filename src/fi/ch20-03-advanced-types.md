# Kehittyneet tyyppiominaisuudet

Rust tarjoaa kehittyneitä tapoja hallita tyyppejä ja niiden käyttäytymistä. Tässä luvussa käsittelemme:

- **Uudelleennimettyjä tyyppejä (`type alias`)**
- **Ulkopuolisten tyyppien määrittämistä (`newtype pattern`)**
- **Dynamisia koon määrittelyjä (`dynamically sized types`)**

## Tyyppialiasit (`type alias`)

Rustissa voidaan määrittää alias tyypeille `type`-avainsanalla:

```rust
type Kilometrit = i32;

let matka: Kilometrit = 5;
println!("Matka: {}", matka);
```

Tämä ei luo uutta tyyppiä, mutta tekee koodista selkeämpää.

## `Newtype`-malli

Jos haluat luoda täysin erillisen tyypin, käytä `struct`-wrapperia:

```rust
struct Username(String);

fn tulosta_nimi(nimi: Username) {
    println!("Käyttäjänimi: {}", nimi.0);
}
```

Tämä estää sekaannukset samanlaisten tyyppien välillä ja mahdollistaa traitien toteuttamisen erikseen.

## Dynaamisesti kokoiltavat tyypit (`DST`)

Normaalisti Rustin tyypit ovat staattisesti määriteltyjä, mutta `str` ja `dyn Trait` ovat poikkeuksia:

```rust
fn tulosta(teksti: &str) {
    println!("{}", teksti);
}
```

`&str` on viittaus dynaamiseen `str`-tyyppiin, jonka tarkka koko ei ole tiedossa käännösaikana.

---

Tässä luvussa käsittelimme kehittyneitä tyyppiominaisuuksia. Seuraavaksi tutkimme kehittyneitä funktio-ominaisuuksia ja sulkeisia (`closures`).
