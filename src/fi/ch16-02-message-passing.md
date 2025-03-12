## Käyttämällä säikeitä koodin ajamiseen samanaikaisesti

Useimmissa nykyisissä käyttöjärjestelmissä suoritettava ohjelma ajetaan _prosessissa_, ja käyttöjärjestelmä hallitsee useita prosesseja samanaikaisesti. Ohjelmassa voi myös olla itsenäisiä osia, jotka suoritetaan rinnakkain. Näitä itsenäisiä osia suorittavia toiminnallisuuksia kutsutaan _säikeiksi_. Esimerkiksi verkkopalvelin voi käyttää useita säikeitä, jotta se voi vastata useisiin pyyntöihin yhtä aikaa.

Jakamalla ohjelmasi laskentaa useisiin säikeisiin voit suorittaa useita tehtäviä samanaikaisesti, mikä voi parantaa suorituskykyä. Tämä tuo kuitenkin mukanaan monimutkaisuutta. Koska säikeet voivat toimia samanaikaisesti, ei ole mitään sisäistä takuuta siitä, missä järjestyksessä eri säikeillä oleva koodi suoritetaan. Tämä voi johtaa ongelmiin, kuten:

- Kilpaileviin tiloihin (_race condition_), joissa säikeet käyttävät tietoja tai resursseja epäjohdonmukaisessa järjestyksessä  
- Lukoittumisiin (_deadlock_), joissa kaksi säiettä odottaa toisiaan, estäen molempia jatkamasta  
- Virheisiin, jotka ilmenevät vain tietyissä tilanteissa ja joita on vaikea toistaa ja korjata luotettavasti  

Rust pyrkii vähentämään säikeiden käytön haittoja, mutta monisäikeisessä ohjelmoinnissa on silti oltava huolellinen ja käytettävä erilaista ohjelmointirakennetta kuin yksittäisessä säikeessä toimivissa ohjelmissa.

Ohjelmointikielet toteuttavat säikeet eri tavoin, ja monet käyttöjärjestelmät tarjoavat ohjelmointirajapinnan (API) uusien säikeiden luomiseksi. Rustin standardikirjasto käyttää _1:1_-säiemallia, jossa jokaiselle ohjelman säikeelle on yksi käyttöjärjestelmän säie. On olemassa kirjastoja, jotka toteuttavat muita säiemalleja ja tekevät erilaisia kompromisseja verrattuna 1:1-malliin. (Rustin asynkroninen järjestelmä, jota käsittelemme seuraavassa luvussa, tarjoaa toisen lähestymistavan rinnakkaisuuteen.)

### Uuden säikeen luominen `spawn`-funktiolla

Uuden säikeen luomiseksi kutsumme `thread::spawn`-funktiota ja annamme sille _sulkeisfunktioksi_ kutsutun rakenteen (sulkeisista puhuttiin luvussa 13), joka sisältää uuden säikeen ajettavan koodin. Esimerkissä 16-1 pääsäie tulostaa tekstiä ja samanaikaisesti luotu säie tulostaa omaa tekstiään:

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-01/src/main.rs}}
```

Huomaa, että kun Rust-ohjelman pääsäie päättyy, kaikki luodut säikeet lopetetaan, riippumatta siitä, ovatko ne suorittaneet kaiken työnsä loppuun. Tämän ohjelman tuloste voi olla hieman erilainen joka kerralla, mutta se näyttää suunnilleen tältä:

```text
hi number 1 from the main thread!
hi number 1 from the spawned thread!
hi number 2 from the main thread!
hi number 2 from the spawned thread!
...
hi number 5 from the spawned thread!
```

Kutsut `thread::sleep`-funktioon pakottavat säikeen pysähtymään hetkeksi, antaen toiselle säikeelle mahdollisuuden suorittaa tehtäviään. Säikeet todennäköisesti vuorottelevat, mutta tätä ei voida taata: se riippuu käyttöjärjestelmän säikeiden ajoituksesta. Tässä esimerkissä pääsäie tulosti ensin, vaikka koodissa `spawn`-säie on kirjoitettu ensin. Lisäksi vaikka käskimme `spawn`-säikeen tulostaa, kunnes `i` saavuttaa arvon `9`, se pääsi vain `5`:een ennen kuin pääsäie lopetti ohjelman suorittamisen.

Jos ajat tämän koodin etkä näe mitään tulostetta `spawn`-säikeestä tai et näe säikeiden tulostusten limittyvän, yritä kasvattaa toistokertojen määrää, jotta käyttöjärjestelmällä on enemmän mahdollisuuksia vaihtaa säikeiden välillä.

### Odottaminen kaikkien säikeiden valmistumiseen `join`-käsittelijöiden avulla

Esimerkissä 16-1 luotu säie päättyy usein ennenaikaisesti pääsäikeen päättyessä. Lisäksi, koska säikeiden suoritusjärjestystä ei voida taata, `spawn`-säikeen suorittamista ei voida varmistaa.

Voimme korjata tämän ongelman tallentamalla `thread::spawn`-funktion palauttaman arvon muuttujaan. `thread::spawn` palauttaa `JoinHandle`-tyyppisen arvon, joka edustaa luotua säiettä. Kun kutsumme `join`-metodia tälle arvolle, ohjelma odottaa, että kyseinen säie päättyy ennen kuin jatkaa suorittamista.

