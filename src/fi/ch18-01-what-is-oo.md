## Oliopohjaisten kielten ominaisuudet

Ohjelmointiyhteisössä ei ole yksimielisyyttä siitä, mitä ominaisuuksia kielen täytyy sisältää ollakseen oliopohjainen. Rustissa on vaikutteita monista ohjelmointiparadigmoista, mukaan lukien OOP. Esimerkiksi luvussa 13 tarkastelimme Rustin funktionaalisia ominaisuuksia. Monet OOP-kielet jakavat kuitenkin tiettyjä yhteisiä piirteitä, kuten oliot, kapseloinnin ja periytymisen. Tarkastellaan näitä ominaisuuksia yksityiskohtaisemmin ja sitä, miten Rust tukee niitä.

### Oliot sisältävät tietoa ja käyttäytymistä

Erich Gammalta, Richard Helmilta, Ralph Johnsonilta ja John Vlissidesilta peräisin oleva kirja _Design Patterns: Elements of Reusable Object-Oriented Software_ (Addison-Wesley Professional, 1994), joka tunnetaan myös nimellä _The Gang of Four_ -kirja, määrittelee oliopohjaisen ohjelmoinnin seuraavasti:

> Oliopohjaiset ohjelmat koostuvat olioista. **Olio** yhdistää sekä tiedon että sitä käsittelevät proseduurit. Näitä proseduurien kutsutaan tyypillisesti **metodeiksi** tai **operaatioiksi**.

Tämän määritelmän perusteella Rust on oliopohjainen: rakenne (`struct`) ja luettelotyypit (`enum`) voivat sisältää tietoa, ja `impl`-lohkot mahdollistavat metodien määrittelyn niille. Vaikka Rust ei nimeltä kutsu niitä olioiksi, ne tarjoavat samaa toiminnallisuutta kuin Gang of Four -määritelmän mukaiset oliot.

### Kapselointi piilottaa toteutuksen yksityiskohdat

Toinen OOP:hen yleisesti liittyvä käsite on _kapselointi_, joka tarkoittaa, että olion toteutuksen yksityiskohdat eivät ole suoraan käytettävissä ulkopuolisesta koodista. Olion kanssa voidaan olla vuorovaikutuksessa vain sen julkisen rajapinnan kautta, mikä estää suoraa pääsyä sisäiseen tietoon ja varmistaa tietojen eheyden.

Rustissa kapselointi hallitaan `pub`-avainsanalla, kuten käsittelimme luvussa 7. Oletusarvoisesti kaikki on yksityistä, ja voimme päättää, mitkä moduulit, tyypit, funktiot ja metodit ovat julkisia. Seuraavassa esimerkissä määritellään `AveragedCollection`, joka sisältää `i32`-lukujen vektorin sekä lasketun keskiarvon:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-01/src/lib.rs}}
```

Struktuuri on `pub`, jotta muut voivat käyttää sitä, mutta sen kentät pysyvät yksityisinä. Tämä varmistaa, että kun arvo lisätään tai poistetaan listasta, keskiarvo päivittyy automaattisesti seuraavalla tavalla:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-02/src/lib.rs:here}}
```

Koska `list` ja `average` ovat yksityisiä, niitä ei voi muokata suoraan ulkopuolisesta koodista. Tämä mahdollistaa turvallisen kapseloinnin.

### Periytyminen tyyppijärjestelmänä ja koodin uudelleenkäyttönä

_Periytyminen_ on mekanismi, jossa olio voi periä toisen olion määrittelyn ja siten saada käyttöönsä sen tiedot ja käyttäytymisen ilman erillistä määrittelyä.

Jos periytymisen täytyy olla osa ohjelmointikieltä, jotta se olisi oliopohjainen, Rust ei täytä tätä vaatimusta. Rust ei tarjoa mahdollisuutta periyttää rakenteen kenttiä ja metodeja ilman makrojen käyttöä.

Periytymisen tarkoituksena on kaksi pääkäyttötapaa: koodin uudelleenkäyttö ja polymorfismi.

Rust mahdollistaa koodin uudelleenkäytön _traitien oletusmetodeilla_. Kun esimerkiksi toteutimme `Summary`-traitin metodin `summarize` oletustoteutuksen (kuten luvussa 10-14), kaikki `Summary`-traitin toteuttavat tyypit saivat tämän metodin käyttöönsä ilman lisäkoodia. Tämä vastaa periytymistä siinä mielessä, että alaluokka perisi yläluokan metodin toteutuksen, mutta Rustissa se tehdään traitien avulla.

Polymorfismi mahdollistaa sen, että sama koodi voi toimia useiden eri tyyppisten tietojen kanssa. Rust ei käytä periytymistä polymorfismin saavuttamiseksi, vaan hyödyntää geneerisyyttä ja trait-sidontoja, mikä tunnetaan _rajoitetuksi parametriseksi polymorfismiksi_.

Periytyminen on menettänyt suosiotaan ohjelmointikielten suunnittelussa, koska se voi johtaa tarpeettoman laajaan koodin jakamiseen ja vähentää joustavuutta. Monissa kielissä esiintyy _yksinkertainen periytyminen_ (subclass voi periä vain yhdestä luokasta), mikä rajoittaa ohjelman suunnittelun joustavuutta.

Tästä syystä Rust käyttää periytymisen sijaan trait-olioita polymorfismin toteuttamiseen.

---

Seuraavaksi tarkastelemme, kuinka trait-oliot mahdollistavat polymorfismin Rustissa.
