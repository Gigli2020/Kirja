## Laajennettava rinnakkaisuus `Sync`- ja `Send`-traitien avulla

On mielenkiintoista, että Rust-kielellä on _hyvin_ vähän sisäänrakennettuja rinnakkaisuusominaisuuksia. Lähes kaikki tämän luvun aikana käsitellyt rinnakkaisuusominaisuudet ovat olleet standardikirjaston osia, eivät itse kielen ominaisuuksia. Rinnakkaisuuden hallinnan vaihtoehdot eivät rajoitu vain kieleen tai standardikirjastoon; voit kirjoittaa omia rinnakkaisuusratkaisujasi tai käyttää muiden tekemiä ratkaisuja.

Kaksi tärkeää rinnakkaisuuteen liittyvää käsitettä on kuitenkin sisäänrakennettu itse kieleen: `std::marker`-traitit `Sync` ja `Send`.

### Omistajuuden siirtäminen säikeiden välillä `Send`-traitin avulla

`Send`-merkkaustraitti osoittaa, että kyseisen tyypin arvot voidaan siirtää säikeiden välillä. Lähes kaikki Rustin tyypit ovat `Send`, mutta on olemassa poikkeuksia, kuten `Rc<T>`. `Rc<T>` ei voi olla `Send`, koska jos kloonaisit `Rc<T>`-arvon ja yrittäisit siirtää sen toiseen säikeeseen, molemmat säikeet voisivat yrittää päivittää viittauslaskuria samanaikaisesti. Tämän vuoksi `Rc<T>` on tarkoitettu käytettäväksi vain yksisäikeisissä tilanteissa, joissa et halua maksaa säikeistämisen turvallisuuteen liittyvää suorituskykyrasitetta.

Rustin tyyppijärjestelmä ja trait-rajoitukset varmistavat, että et voi vahingossa siirtää `Rc<T>`-arvoa säikeiden välillä vaarallisella tavalla. Kun yritimme tehdä tämän esimerkissä 16-14, saimme virheilmoituksen `the trait Send is not implemented for Rc<Mutex<i32>>`. Kun vaihdoimme `Arc<T>`-tyyppiin, joka tukee `Send`-traitia, koodi kääntyi.

Kaikki `Send`-tyypeistä koostuvat tyypit merkitään automaattisesti `Send`-traitilla. Lähes kaikki primitiivityypit ovat `Send`, lukuun ottamatta raakaviittauksia, joita käsittelemme luvussa 20.

### Pääsy samaan tietoon useista säikeistä `Sync`-traitin avulla

`Sync`-merkkaustraitti tarkoittaa, että kyseistä tyyppiä on turvallista viitata useista säikeistä. Toisin sanoen tyyppi `T` on `Sync`, jos `&T` (eli `T`:n muuttumaton viittaus) on `Send`, mikä tarkoittaa, että viittauksen voi lähettää turvallisesti toiseen säikeeseen. Samoin kuin `Send`, primitiivityypit ovat `Sync`, ja kaikki `Sync`-tyypeistä koostuvat tyypit ovat myös `Sync`.

Älykäs osoitin `Rc<T>` ei ole `Sync` samoista syistä kuin se ei ole `Send`. Myös `RefCell<T>` ja siihen liittyvät `Cell<T>`-tyypit eivät ole `Sync`, koska niiden lainanvalvonta tapahtuu ajonaikaisesti eikä ole säieturvallinen. Sen sijaan älykäs osoitin `Mutex<T>` on `Sync` ja sitä voidaan käyttää jakamaan pääsy useille säikeille, kuten nähtiin luvussa [“`Mutex<T>`:n jakaminen useiden säikeiden kesken”](ch16-03-shared-state.html#sharing-a-mutext-between-multiple-threads).

### `Send`- ja `Sync`-traitien manuaalinen toteuttaminen on vaarallista

Koska `Send`- ja `Sync`-tyypeistä koostuvat tyypit merkitään automaattisesti näillä traiteilla, niitä ei tarvitse toteuttaa manuaalisesti. Koska ne ovat merkkaustraiteja, niillä ei ole mitään toteutettavia metodeja, vaan ne ovat hyödyllisiä vain rinnakkaisuuteen liittyvien sääntöjen valvonnassa.

Näiden traitien manuaalinen toteuttaminen vaatii `unsafe`-koodia, jota käsittelemme luvussa 20. On tärkeää ymmärtää, että uusien rinnakkaisuusmekanismien luominen ilman `Send`- ja `Sync`-komponentteja vaatii erityistä varovaisuutta, jotta Rustin turvallisuustakeet säilyvät. [“The Rustonomicon”](../nomicon/index.html) sisältää lisätietoa näistä takeista ja niiden ylläpidosta.

## Yhteenveto

Tämä ei ole viimeinen kerta, kun kohtaamme rinnakkaisuuden tässä kirjassa: seuraava luku keskittyy asynkroniseen ohjelmointiin, ja luvun 21 projektissa hyödynnämme tämän luvun käsitteitä realistisemmassa tilanteessa.

Kuten aiemmin mainittiin, koska suurin osa Rustin rinnakkaisuusominaisuuksista ei ole osa itse kieltä, monet rinnakkaisuusratkaisut toteutetaan erillisinä kirjastoina. Näitä kirjastoja kehitetään nopeammin kuin standardikirjastoa, joten kannattaa etsiä verkosta ajankohtaisia kirjastoja monisäikeisiin tilanteisiin.

Rustin standardikirjasto tarjoaa kanavia viestinvälitykseen ja älyosoitinluokkia, kuten `Mutex<T>` ja `Arc<T>`, jotka ovat turvallisia käyttää rinnakkaisissa konteksteissa. Rustin tyyppijärjestelmä ja lainanvalvonta varmistavat, että näitä ratkaisuja käyttävä koodi ei joudu tietokilpailuihin tai virheellisiin viittauksiin. Kun koodisi kääntyy, voit olla varma, että se toimii useilla säikeillä ilman vaikeasti jäljitettäviä virheitä, joita esiintyy monissa muissa kielissä. Rinnakkaisohjelmointi ei ole enää asia, jota p...
