## `RefCell<T>` ja sisäinen mutabiliteetti

_Sisäinen mutabiliteetti_ (_interior mutability_) on Rustin suunnittelumalli, joka sallii datan muuttamisen, vaikka siihen olisi vain muuttumattomia viittauksia. Tämä rikkoo normaalisti Rustin lainaussääntöjä, mutta sisäinen mutabiliteetti käyttää `unsafe`-koodia sääntöjen kiertämiseen ohjelmoijan vastuulla. Rustin `RefCell<T>`-tyyppi käyttää tätä mallia.

### Lainaussääntöjen noudattaminen ajonaikana `RefCell<T>`:llä

Toisin kuin `Rc<T>`, `RefCell<T>` mahdollistaa yksittäisen omistajan. Sen ero `Box<T>`:ään on siinä, että se tarkistaa lainaussäännöt ajonaikana käännösajan sijaan.

Rustin lainaussäännöt luvusta 4:

- Voi olla joko yksi muuttuva viite tai useita muuttumattomia viitteitä.
- Viitteiden tulee aina olla kelvollisia.

Normaalit viitteet ja `Box<T>` tarkistavat nämä säännöt käännösaikana. `RefCell<T>` tarkistaa ne ajonaikana: jos rikot sääntöjä, ohjelma kaatuu ajon aikana (`panic!`), mutta kääntäjä ei estä kääntymistä.

Käännösaikainen tarkistus on parempi, koska virheet löytyvät aikaisemmin ilman suorituskykykustannuksia. Ajonaikainen tarkistus taas mahdollistaa tiettyjä ohjelmallisesti turvallisia tilanteita, jotka kääntäjä muuten estäisi.

`RefCell<T>` toimii vain yksisäikeisessä ohjelmassa. Luvussa 16 käsittelemme, miten vastaava toiminnallisuus toimii monisäikeisissä ohjelmissa.

### Milloin valita `Box<T>`, `Rc<T>` tai `RefCell<T>`?

- `Rc<T>` mahdollistaa useita omistajia; `Box<T>` ja `RefCell<T>` eivät.  
- `Box<T>` mahdollistaa muuttuvat viitteet käännösaikana, `Rc<T>` vain muuttumattomat viitteet käännösaikana, `RefCell<T>` sekä muuttuvat että muuttumattomat viitteet ajonaikana.  
- `RefCell<T>` mahdollistaa datan muuttamisen, vaikka `RefCell<T>`-instanssi olisi muuttumaton.  

Tätä kutsutaan sisäiseksi mutabiliteetiksi.

### Sisäinen mutabiliteetti: muuttuva viite muuttumattomaan arvoon

Normaalisti muuttumattomaan arvoon ei voi ottaa muuttuvaa viitettä:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/no-listing-01-cant-borrow-immutable-as-mutable/src/main.rs}}
```

Tämä ei käänny:

```console
{{#include ../listings/ch15-smart-pointers/no-listing-01-cant-borrow-immutable-as-mutable/output.txt}}
```

Joskus on hyödyllistä sallia itseensä muuttuva arvo, mutta estää sen muuttaminen ulkopuolelta. `RefCell<T>` mahdollistaa tämän, mutta lainaussäännöt tarkistetaan ajonaikana.

### Esimerkki sisäisen mutabiliteetin hyödyllisyydestä: mock-oliot

Testauksessa käytetään _mock-olioita_, jotka tallentavat kutsutut metodit. Rustissa tämä voidaan toteuttaa `RefCell<T>`:n avulla.

Alla oleva testikoodi ei toimi ilman sisäistä mutabiliteettia:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-21/src/lib.rs:here}}
```

Tämä epäonnistuu:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-21/output.txt}}
```

Koska `send` ottaa `&self`, `MockMessenger` ei voi lisätä viestejä vektoriin. `Messenger`-traitia ei voida muuttaa `&mut self`:ksi vain testin vuoksi, joten käytämme `RefCell<T>`:tä:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-22/src/lib.rs}}
```

Nyt `send` voi muuttaa `sent_messages`-vektoria `borrow_mut`-kutsulla.

### `RefCell<T>`:n lainausten seuranta ajonaikana

Viitteiden sijaan `RefCell<T>` käyttää `borrow` ja `borrow_mut` -metodeja, jotka palauttavat `Ref<T>` ja `RefMut<T>` -älykkäät osoittimet. `RefCell<T>` seuraa aktiivisten lainausten määrää ja sallii useita muuttumattomia lainauksia tai yhden muuttuvan lainauksen kerrallaan.

Jos sääntöjä rikotaan, `RefCell<T>` kaatuu ajonaikana (Listing 15-23):

```rust,ignore,panics
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-23/src/lib.rs:here}}
```

Tämä antaa `BorrowMutError`-virheen:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-23/output.txt}}
```

Ajonaikainen tarkistus tarkoittaa, että virheet voivat löytyä myöhemmin kehitysprosessissa, mutta `RefCell<T>` mahdollistaa tilanteet, joissa tavalliset viitteet eivät riitä.

### Useita omistajia ja muuttuva data: `Rc<T>` + `RefCell<T>`

Yhdistämällä `Rc<T>` ja `RefCell<T>` saamme datan, jota voidaan sekä jakaa että muuttaa:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-24/src/main.rs}}
```

Tässä `Rc<RefCell<i32>>`-tyyppi mahdollistaa jaetun omistajuuden ja muutettavuuden. `borrow_mut`-metodi palauttaa `RefMut<T>`-osoittimen, jonka avulla voidaan muuttaa dataa.

Tuloste osoittaa, että kaikki listan viittaukset päivittyvät:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-24/output.txt}}
```

Tämä yhdistelmä sallii tietorakenteen näyttävän muuttumattomalta ulospäin, mutta sen sisäinen data voidaan silti muuttaa. Tämä sopii hyvin yksisäikeisiin ohjelmiin, mutta monisäikeisessä ohjelmoinnissa `Mutex<T>` on turvallisempi vaihtoehto. `Mutex<T>` käsitellään luvussa 16.
