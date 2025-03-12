# Asynkronisen ohjelmoinnin perusteet: Async, Await, Futures ja Streams

Monet tietokoneelle antamamme tehtävät voivat kestää kauan. Olisi hyödyllistä, jos voisimme tehdä jotain muuta samalla, kun odotamme näiden pitkien prosessien valmistumista. Modernit tietokoneet tarjoavat kaksi tekniikkaa useiden tehtävien suorittamiseen samanaikaisesti: _rinnakkaisuus_ ja _samanaikaisuus_. Kun alamme kirjoittaa ohjelmia, jotka sisältävät rinnakkaisia tai samanaikaisia toimintoja, kohtaamme nopeasti uusia haasteita, jotka liittyvät _asynkroniseen ohjelmointiin_. Tämä luku rakentuu ...

Rustin _async_ (lyhenne sanasta _asynchronous_, eli asynkroninen) -abstraktio mahdollistaa juuri tämän. Tässä luvussa opit käyttämään async-ohjelmointia seuraavien aiheiden kautta:

- Kuinka käyttää Rustin `async`- ja `await`-syntaksia
- Kuinka ratkaista joitain samoja haasteita, joita tarkastelimme luvussa 16, asynkronisella mallilla
- Kuinka monisäikeisyys ja async tarjoavat toisiaan täydentäviä ratkaisuja, joita voi yhdistellä monissa tapauksissa

Ennen kuin sukellamme async-ohjelmointiin käytännössä, meidän täytyy ensin erottaa toisistaan _rinnakkaisuus_ ja _samanaikaisuus_.

### Rinnakkaisuus ja samanaikaisuus

Tähän mennessä olemme käsitelleet rinnakkaisuutta ja samanaikaisuutta lähes synonyymeinä. Nyt on tärkeää erottaa ne tarkemmin, sillä niiden eroilla on merkitystä, kun alamme työskennellä async-ohjelmoinnin kanssa.

Ajatellaan esimerkkiä ohjelmistokehitystiimistä. Tiimi voi jakaa työtehtäviä eri tavoin: yksi jäsen voi työskennellä useiden tehtävien parissa vuorotellen tai jokainen jäsen voi keskittyä yhteen tehtävään kerrallaan.

Kun yksittäinen henkilö työskentelee usean eri tehtävän parissa, mutta ei suorita niitä täysin samanaikaisesti, tämä on _samanaikaisuutta_ (concurrency). Ehkä sinulla on kaksi eri projektia avoinna tietokoneellasi, ja kun kyllästyt tai jäät jumiin toisessa projektissa, vaihdat toiseen. Et voi edistää molempia tehtäviä täsmälleen samalla hetkellä, mutta voit työskennellä vuorotellen molempien kanssa.

Kun tiimi jakaa tehtävät siten, että jokainen jäsen keskittyy vain yhteen tehtävään ja suorittaa sitä itsenäisesti, tämä on _rinnakkaisuutta_ (parallelism). Jokainen jäsen voi edetä yhtä aikaa omassa tehtävässään.

Rustin async-ohjelmointi keskittyy ennen kaikkea samanaikaisuuteen. Riippuen käytetystä laitteistosta, käyttöjärjestelmästä ja async-ajoympäristöstä (joista puhumme myöhemmin), tämä samanaikaisuus voi hyödyntää myös rinnakkaisuutta taustalla.

Nyt voimme syventyä siihen, miten async-ohjelmointi toimii Rustissa käytännössä.
