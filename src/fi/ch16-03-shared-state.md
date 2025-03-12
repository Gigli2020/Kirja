## Jaetun tilan rinnakkaisuus

Viestinvälitys on hyvä tapa hallita rinnakkaisuutta, mutta se ei ole ainoa tapa. Toinen menetelmä on, että useat säikeet pääsevät käsiksi samaan jaettuun tietoon. Go-ohjelmointikielen dokumentaatiossa sanotaan: "Älä kommunikoi jakamalla muistia, vaan jaa muisti kommunikoimalla."

Miltä muistia jakamalla kommunikointi näyttäisi? Lisäksi, miksi viestinvälityksen kannattajat varoittavat muistinjaon käytöstä?

Kanavat missä tahansa ohjelmointikielessä ovat eräänlainen yksinomistajuuden muoto: kun siirrät arvon kanavan kautta, sinun ei tulisi enää käyttää sitä. Jaetun muistin rinnakkaisuus on kuin moniomistajuus: useat säikeet voivat käyttää samaa muistialuetta samanaikaisesti. Kuten luvussa 15 nähtiin, älykkäät osoittimet mahdollistavat moniomistajuuden, mutta se lisää monimutkaisuutta, koska eri omistajien hallinta vaatii tarkkuutta. Rustin tyyppijärjestelmä ja omistussäännöt auttavat merkittävästi tämän hallinnassa.

### Muteksien käyttäminen rajoittamaan tietojen käyttö yhteen säikeeseen kerrallaan

Muteksien nimi tulee termistä _mutual exclusion_ (keskinäinen poissulkeminen), mikä tarkoittaa, että muteksi sallii vain yhden säikeen käyttää tietoa kerrallaan. Jotta säie pääsee käsiksi muteksin suojaamaan tietoon, sen on ensin hankittava lukitus (_lock_). Lukitus on osa muteksia ja seuraa, kuka tällä hetkellä omistaa tiedon yksinoikeudella. Tästä syystä muteksia kuvataan suojaavan tietoa lukitusjärjestelmän avulla.

Muteksit voivat olla hankalia käyttää, koska niihin liittyy kaksi tärkeää sääntöä:

1. Lukitus on hankittava ennen kuin tietoa voi käyttää.
2. Kun tietoa ei enää tarvita, lukitus on vapautettava, jotta muut säikeet voivat käyttää tietoa.

Rustin tyyppijärjestelmän ja omistajuussääntöjen ansiosta et voi tehdä virheitä lukituksen hallinnassa.

#### `Mutex<T>`-tyypin käyttöliittymä

Tarkastellaan esimerkkiä muteksin käytöstä yksisäikeisessä kontekstissa:

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-12/src/main.rs}}
```

#### `Mutex<T>`:n jakaminen useiden säikeiden kesken

Nyt yritämme jakaa muuttujan useiden säikeiden kesken `Mutex<T>`:n avulla:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-13/src/main.rs}}
```

Rust antaa virheen, koska `counter`-muuttuja siirtyy ensimmäisen säikeen omistukseen eikä sitä voi käyttää seuraavissa säikeissä. Korjaamme tämän hyödyntämällä moniomistajuutta.

#### Atominen referenssilaskenta `Arc<T>`:n avulla

`Arc<T>` on versio `Rc<T>`:stä, joka on turvallinen käyttää rinnakkaisessa ympäristössä.

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-15/src/main.rs}}
```

Kun suoritamme tämän ohjelman, se tulostaa:

```text
Tulos: 10
```

Tämä osoittaa, että olemme onnistuneesti jakaneet laskurin turvallisesti useiden säikeiden kesken. Käyttämällä `Mutex<T>`- ja `Arc<T>`-yhdistelmää voimme hallita jaettua tilaa turvallisesti useiden säikeiden kesken ilman tietojen vioittumista tai kilpailevia tiloja.
