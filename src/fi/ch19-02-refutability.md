## Mallien refutaatio ja ehdollisuus

Rustissa mallit voivat olla **refutoitavia** tai **ei-refutoitavia**. Tämä tarkoittaa, voiko malli epäonnistua sovituksessa vai ei. Tällä on vaikutusta siihen, missä voimme käyttää tiettyjä malleja.

### Ei-refutoitavat ja refutoitavat mallit

- **Ei-refutoitavat mallit** (irrefutable) sovittuvat aina annettuun arvoon. Esimerkiksi `let`-lausunnoissa käytettävät mallit ovat aina ei-refutoitavia:
  ```rust
  let x = 5; // Tämä ei voi epäonnistua.
  ```

- **Refutoitavat mall...

### Ei-refutoitavat mallit `let`-lausunnoissa

Koska `let`-lausunnot eivät voi epäonnistua, niissä on käytettävä ei-refutoitavia malleja:

```rust,ignore
let Some(x) = Some(5); // Tämä antaa virheen, koska `Some(x)` on refutoitava malli.
```

Rust sallii kuitenkin mallin käytön `if let`-rakenteessa, koska siinä epäonnistuminen voidaan käsitellä:

```rust
if let Some(x) = Some(5) {
    println!("Löytyi: {}", x);
}
```

### Refutoitavat mallit `match`-lauseissa

`match`-lauseiden tulee kattaa kaikki mahdolliset tapaukset, mutta ne voivat käyttää refutoitavia malleja, koska Rust tarkistaa, että kaikki mahdolliset arvot käsitellään:

```rust
match Some(5) {
    Some(x) => println!("Löytyi: {}", x),
    None => println!("Ei arvoa"),
}
```

---

Tässä luvussa opimme, kuinka Rust käsittelee refutoitavia ja ei-refutoitavia malleja. Tämä auttaa ymmärtämään, miksi jotkin mallit toimivat vain tietyissä rakenteissa, kuten `let` tai `if let`.

