## Oliopohjaiset suunnittelumallit Rustissa

Rust ei seuraa perinteistä oliopohjaista ohjelmointimallia, mutta se tarjoaa joukon ominaisuuksia, jotka mahdollistavat monien OOP:n suunnittelumallien toteuttamisen. Tässä luvussa vertailemme Rustin ja perinteisten OOP-mallien lähestymistapoja.

### Tilakoneiden toteuttaminen ilman periytymistä

Monet ohjelmat käyttävät **tilakoneita** hallitakseen eri vaiheita, joissa jokin objekti voi olla. Perinteisissä OOP-kielissä tämä toteutetaan yleensä käyttämällä periytymistä. Rustissa voimme käyttää **enumeja** tämän mallin toteuttamiseen tehokkaasti.

Seuraavassa esimerkissä luomme Rustilla blogijärjestelmän, joka käyttää tilakoneperiaatetta:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-11/src/lib.rs}}
```

Tässä `Post`-rakenne sisältää `state`, joka määrittelee nykyisen tilan käyttäen `Option<Box<dyn State>>`. Eri tilat (`Draft`, `PendingReview`, `Published`) määritellään omiksi struktuureikseen, ja ne toteuttavat `State`-traitin.

### Tilakoneiden toteuttaminen enumien avulla

Rust mahdollistaa myös tilakoneiden toteuttamisen **enumien** avulla, mikä on idiomaattinen tapa hallita tilasiirtymiä:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-17/src/lib.rs}}
```

Tässä `Post`-struktuuri käyttää `State`-enumia tilan määrittämiseen, mikä vähentää dynaamista allokointia ja tekee koodista selkeämpää.

### Milloin käyttää trait-olioita ja milloin enumia?

- **Käytä trait-olioita (`dyn Trait`)**, jos haluat käyttää polymorfiaa ja eri tilat voivat olla eri kokoisia tai niiden toteutukset voivat muuttua dynaamisesti.
- **Käytä enumia**, jos kaikki tilat ovat tiedossa käännösaikana ja niiden käsittely voidaan tehdä suoraan match-lauseella.

Rust tarjoaa monia vaihtoehtoja OOP-mallien toteuttamiseen ilman perinteistä periytymistä, ja oikean mallin valinta riippuu käyttötapauksesta.

---

Tässä luvussa olemme nähneet, kuinka Rust mahdollistaa oliopohjaisten suunnittelumallien toteuttamisen ilman periytymistä. Seuraavaksi jatkamme Rustin muihin ohjelmointiparadigmoihin ja niiden hyödyntämiseen.
