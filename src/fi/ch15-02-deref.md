## Älykkäiden osoittimien käsittely kuten tavallisia viitteitä `Deref`-traitin avulla

Toteuttamalla `Deref`-traitin voit mukauttaa _dereferenssioperaattorin_ `*` käyttäytymistä (ei pidä sekoittaa kertolasku- tai globaalioperaattoriin). Kun toteutat `Deref`-traitin niin, että älykästä osoitinta voidaan käsitellä tavallisen viitteen tapaan, voit kirjoittaa koodia, joka toimii sekä viitteiden että älykkäiden osoittimien kanssa.

Katsotaan ensin, miten dereferenssioperaattori toimii tavallisten viitteiden kanssa. Sitten määritämme oman tyypin, joka käyttäytyy kuten `Box<T>`, ja näemme, miksi dereferenssioperaattori ei toimi sen kanssa ilman `Deref`-traitin toteutusta. Lopuksi käsittelemme Rustin _deref-muunnosta_ (`deref coercion`), joka mahdollistaa sen, että viitteet ja älykkäät osoittimet toimivat saumattomasti yhdessä.

> **Huom:** Seuraavassa toteuttamamme `MyBox<T>`-tyyppi eroaa oikeasta `Box<T>`-tyypistä siinä, että se ei tallenna dataa keolle. Keskitymme esimerkissä `Deref`-traitin käyttöön, joten datan tallennuspaikka ei ole oleellista.

### Osoittimen seuraaminen arvon luo

Tavallinen viite on osoitin, joka viittaa toiseen muistipaikkaan tallennettuun arvoon. Alla oleva esimerkki (Listing 15-6) näyttää, kuinka viitettä voidaan käyttää seuraamalla sitä dereferenssioperaattorilla:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-06/src/main.rs}}
```

Tässä muuttuja `x` sisältää `i32`-arvon `5`. Määritämme `y`:n viittaamaan `x`:ään. Kun haluamme vertailla `y`:n sisältämää arvoa, meidän on käytettävä `*y` seurataksemme viitettä ja päästäksemme käsiksi varsinaiseen arvoon. Ilman `*`-operaattoria vertailu ei toimisi, koska `y` on viite, ei suoraan `i32`-arvo.

### `Box<T>` käyttäminen kuten viitettä

Voimme muokata edellistä koodia käyttämään `Box<T>`-tyyppiä tavallisen viitteen sijaan. Seuraavassa esimerkissä (Listing 15-7) `Box<T>`:n dereferenssioperaattori toimii samalla tavalla kuin viitteen kanssa:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-07/src/main.rs}}
```

Suurin ero on, että `y` on nyt `Box<T>`, joka osoittaa kopioituun `x`-arvoon viittauksen sijaan. Voimme käyttää `*y`-operaattoria samalla tavalla kuin viitteen kanssa.

### Oman älykkään osoittimen määrittely

Rakennetaan nyt oma älykäs osoitin, `MyBox<T>`, joka käyttäytyy `Box<T>`:n tavoin. Toteutamme sen ensin ilman `Deref`-traitia:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-08/src/main.rs:here}}
```

`MyBox<T>` on tuple-struct, jossa on yksi `T`-tyyppinen kenttä. Määrittelemme myös `new`-funktion vastaamaan `Box<T>::new`-metodia.

Jos yritämme käyttää `MyBox<T>`-tyyppiä kuten `Box<T>`-tyyppiä, koodi ei kuitenkaan käänny, koska Rust ei tiedä, miten `MyBox<T>`-tyyppiä pitäisi dereferoida:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-09/src/main.rs:here}}
```

Saamme seuraavan virheilmoituksen:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-09/output.txt}}
```

Rust ei osaa käyttää `*`-operaattoria `MyBox<T>`-tyypille, koska emme ole toteuttaneet `Deref`-traitia.

### `Deref`-traitin toteuttaminen

Toteutetaan `Deref`-trait `MyBox<T>`:lle:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-10/src/main.rs:here}}
```

Tämä määrittää `deref`-metodin, joka palauttaa viitteen sisäiseen arvoon. Nyt `*`-operaattori toimii `MyBox<T>`-tyypin kanssa.

Rust korvaa `*y`-operaation seuraavalla koodilla:

```rust,ignore
*(y.deref())
```

Tämä tapahtuu automaattisesti, joten meidän ei tarvitse kutsua `deref`-metodia suoraan.

### `Deref`-muunnos (Deref coercion)

_Deref-muunnos_ on Rustin ominaisuus, joka muuntaa viittauksen `&T` toiseen tyyppiin `&U`, jos `T` toteuttaa `Deref<Target=U>`. Tämä mahdollistaa esimerkiksi `&String`-tyypin automaattisen muuntamisen `&str`-tyypiksi.

Alla olevassa esimerkissä (Listing 15-12) `hello`-funktio ottaa vastaan `&str`-tyyppisen parametrin. Voimme kutsua sitä `MyBox<String>`-tyypillä, koska `Deref`-muunnos muuttaa `&MyBox<String>` -> `&String` -> `&str`:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-12/src/main.rs:here}}
```

Jos Rustissa ei olisi `Deref`-muunnosta, meidän pitäisi kirjoittaa monimutkaisempi koodi (Listing 15-13):

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-13/src/main.rs:here}}
```

Tämä tekisi koodista vaikealukuisemman. `Deref`-muunnos mahdollistaa yksinkertaisemman syntaksin.

### `Deref`-muunnoksen vaikutus mutabiliteettiin

Rust suorittaa `Deref`-muunnoksen kolmessa tapauksessa:

1. `&T` -> `&U`, kun `T: Deref<Target=U>`
2. `&mut T` -> `&mut U`, kun `T: DerefMut<Target=U>`
3. `&mut T` -> `&U`, kun `T: Deref<Target=U>`

Huomaa, että `&mut T` voidaan muuntaa `&U`:ksi, mutta ei toisinpäin. Tämä johtuu Rustin lainaussäännöistä, jotka estävät muuttamattoman viitteen muuttamisen muuttuvaksi.

`Deref`-traitin ja `Deref`-muunnoksen ansiosta Rust mahdollistaa joustavan tavan käsitellä viitteitä ja älykkäitä osoittimia ilman ylimääräisiä viittausten ja dereferenssien lisäämisiä. Tämä parantaa koodin luettavuutta ja tekee siitä turvallisempaa.
