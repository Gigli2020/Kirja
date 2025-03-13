## Trait-oliot polymorfismin toteuttamiseksi

Rust ei käytä periytymistä polymorfismin toteuttamiseen, kuten monet perinteiset oliopohjaiset kielet. Sen sijaan Rust käyttää **trait-olioita**, jotka mahdollistavat dynaamisen polymorfismin ilman periytymistä.

### Trait-olion luominen

Trait-olio saadaan aikaan käyttämällä `dyn`-avainsanaa, joka ilmaisee dynaamista lähettämistä. Alla on esimerkki `Draw`-traitista ja `Screen`-rakenteesta, joka voi sisältää minkä tahansa `Draw`-traitin toteuttavan olion:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-03/src/lib.rs}}
```

Tässä `dyn Draw` tarkoittaa, että `components`-vektori voi sisältää mitä tahansa `Draw`-traitin toteuttavaa tyyppiä. Tämä vastaa perinteisen OOP:n periytymismekanismeja, joissa voi olla yläluokan viittauksia aliluokkiin.

### Trait-olion käyttö polymorfismissa

Polymorfismi mahdollistaa saman koodin käyttämisen eri tyyppisten olioiden kanssa. Seuraavassa esimerkissä lisätään `Button`-rakenne, joka toteuttaa `Draw`-traitin:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-04/src/lib.rs}}
```

Nyt `Screen`-olioon voidaan lisätä sekä `Button`-tyyppisiä että muita `Draw`-traitin toteuttavia olioita. Tämä tarjoaa joustavan tavan laajentaa ohjelmaa.

### Trait-olioiden suorituskyky ja rajoitukset

Trait-olion käyttäminen johtaa **dynaamiseen lähettämiseen**, mikä tarkoittaa, että kääntäjä ei tiedä tarkkaa tyyppiä käännösaikana, vaan ohjelma ratkaisee sen ajonaikaisesti. Tämä tuo joustavuutta, mutta voi myös aiheuttaa pienen suorituskykykustannuksen verrattuna staattiseen polymorfismiin, jossa kääntäjä voi optimoida tyypit etukäteen.

Koska trait-oliot käyttävät dynaamista lähetystä, ne eivät voi sisältää metodeja, joissa on geneerisiä parametreja tai assosioituja tyyppejä. Jos tarvitset geneerisiä metodeja, sinun on käytettävä **trait-sidontoja** staattisen polymorfismin saavuttamiseksi.

---

Tässä luvussa olemme nähneet, kuinka trait-oliot mahdollistavat polymorfismin Rustissa ilman perinteistä periytymistä. Seuraavaksi vertailemme, milloin kannattaa käyttää trait-olioita ja milloin geneerisiä trait-sidontoja.
