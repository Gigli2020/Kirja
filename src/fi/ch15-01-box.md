## `Box<T>`-tyypin käyttäminen keolle sijoitettavan datan osoittamiseen

Yksi yksinkertaisimmista älykkäistä osoittimista on _box_, jonka tyyppi on `Box<T>`. Box mahdollistaa datan tallentamisen keolle sen sijaan, että se sijaitsee pinossa. Pinoon jää vain osoitin keolla olevaan dataan. Luvussa 4 käsiteltiin pinon ja keon eroa, joten voit palata siihen tarvittaessa.

Box ei aiheuta suorituskykyhaittaa, lukuun ottamatta sitä, että sen data tallennetaan keolle pinoon sijaan. Se ei myöskään tarjoa erityisiä lisäominaisuuksia. Useimmiten käytät boxia seuraavissa tilanteissa:

- Kun sinulla on tyyppi, jonka kokoa ei voida määrittää käännösaikana, mutta haluat käyttää sitä kontekstissa, jossa tarvitaan tarkka koko.  
- Kun sinulla on suuri määrä dataa ja haluat siirtää omistajuuden ilman, että dataa kopioidaan.  
- Kun haluat omistaa arvon ja tarvitset vain sen toteuttavan tietyn traitin, etkä välitä sen tarkasta tyypistä.  

Ensimmäistä tapausta käsitellään osiossa [“Rekursiivisten tyyppien mahdollistaminen Boxilla”](#rekursiivisten-tyyppien-mahdollistaminen-boxilla). Toisessa tapauksessa suuren tietomäärän omistajuuden siirtäminen voi olla hidasta, koska data kopioidaan pinoon. Voit parantaa suorituskykyä sijoittamalla tiedon keolle boxiin, jolloin vain osoitin kopioidaan, mutta varsinainen data pysyy paikallaan. Kolmas tapaus tunnetaan _trait-oliona_, jota käsitellään tarkemmin luvussa 18.

### `Box<T>`-tyypin käyttäminen keolle tallennettavaan dataan

Ennen kuin käsittelemme `Box<T>`:n käyttöä keon hyödyntämisessä, käydään läpi sen syntaksi ja kuinka sen sisältämää dataa käytetään.

Seuraavassa esimerkissä (Listing 15-1) tallennetaan `i32`-tyyppinen arvo keolle käyttäen boxia:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-01/src/main.rs}}
```

Tässä määritetään muuttuja `b`, joka on `Box`, joka osoittaa arvoon `5`, joka on sijoitettu keolle. Ohjelma tulostaa `b = 5`. Boxin sisältöä voidaan käyttää samalla tavalla kuin pinossa olevaa arvoa. Kun `b` poistuu laajuudesta, sekä box (pinoon tallennettu osoitin) että keolla oleva data vapautetaan.

Yksittäisen arvon sijoittaminen keolle ei yleensä ole hyödyllistä, joten tätä tapaa ei käytetä usein. Useimmissa tapauksissa on parempi pitää yksittäiset arvot pinossa. Katsotaan seuraavaksi tapaus, jossa box mahdollistaa tyypin määrittelyn, jota ei muuten voisi tehdä.

### Rekursiivisten tyyppien mahdollistaminen Boxilla

_Rekursiivinen tyyppi_ sisältää arvon, joka on samaa tyyppiä kuin itse tyyppi. Rekursiiviset tyypit aiheuttavat ongelman, koska Rustin on tiedettävä tyypin koko käännösaikana. Koska rekursiivinen tyyppi voi periaatteessa jatkua äärettömästi, Rust ei voi määrittää sen vaatimaa muistimäärää. Koska boxilla on aina tunnettu koko, voimme ratkaista tämän sijoittamalla rekursiivisen tyypin sisään boxin.

Esimerkkinä tarkastellaan _cons-listaa_, joka on yleinen tietorakenne funktionaalisissa ohjelmointikielissä. 

#### Cons-listan taustaa

_Cons-lista_ tulee Lisp-ohjelmointikielestä ja sen muunnelmista. Se koostuu sisäkkäisistä pareista ja toimii linkitetyn listan tapaan. Sen nimi tulee Lispin `cons`-funktiosta, joka luo uuden parin kahdesta argumentistaan. Esimerkiksi seuraava pseudokoodi kuvaa cons-listaa, joka sisältää luvut 1, 2 ja 3:

```text
(1, (2, (3, Nil)))
```

Jokainen alkio sisältää kaksi elementtiä: nykyisen arvon ja seuraavan alkion. Viimeinen alkio sisältää vain arvon `Nil`, joka merkitsee listan loppua.

### `Box<T>`:n käyttäminen rekursiivisten tyyppien kanssa

Rust ei osaa määrittää, kuinka paljon muistia rekursiivinen tyyppi tarvitsee. Seuraava käännösvirhe kertoo tästä:

```text
help: insert some indirection (e.g., a `Box`, `Rc`, or `&`) to break the cycle
  |
2 |     Cons(i32, Box<List>),
  |               ++++    +
```

Tästä saamme vinkin käyttää `Box<T>`:tä. Koska `Box<T>` on osoitin, sen koko on aina tiedossa. Voimme siis sijoittaa `Box<T>`:n `Cons`-variantin sisään estääksemme rekursion aiheuttaman ongelman.

Seuraavassa on korjattu `List`-enum, joka käyttää `Box<T>`:tä:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-05/src/main.rs}}
```

Tässä `Cons`-variantti sisältää `i32`-arvon sekä osoittimen seuraavaan listan alkioon, joka sijaitsee keolla. Tällä tavoin vältämme äärettömän rekursion ja Rust pystyy määrittämään muistin koon.

Box tarjoaa vain osoittimen ja keolle allokoinnin, mutta ei muita erityisominaisuuksia. Se ei tuo suorituskykyhaittaa, ja sen avulla voidaan käsitellä tietorakenteita, joissa tarvitaan viittausta itsenäisesti sijaitsevaan dataan. `Box<T>` on älykäs osoitin, koska se toteuttaa `Deref`-traitin, jonka avulla `Box<T>`:tä voidaan käsitellä kuten viitteitä. Kun `Box<T>` poistuu laajuudesta, sen osoittama data keolla vapautetaan `Drop`-traitin ansiosta.

Näitä kahta traitia (`Deref` ja `Drop`) käytetään myös muissa älykkäissä osoittimissa, joita käsittelemme myöhemmin tässä luvussa. Jatketaan seuraavaksi niiden tarkastelua!
