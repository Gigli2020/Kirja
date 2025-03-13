## Streamit: Futuurit peräkkäin

Tässä luvussa olemme tähän mennessä keskittyneet yksittäisiin futuureihin. Yksi suuri poikkeus oli asynkroninen kanava, jota käytimme viestinvälitykseen. Muistatko, kuinka käytimme vastaanotinta asynkroniselle kanavalle aikaisemmin tässä luvussa kohdassa ["Viestinvälitys"](ch17-02-concurrency-with-async.html#message-passing)? Asynkroninen `recv`-metodi tuottaa ajan mittaan sarjan kohteita. Tämä on esimerkki paljon yleisemmästä mallista, jota kutsutaan _streamiksi_.

Luvussa 13 tutustuimme `Itera...

### Streamien yhdistäminen

Monet käsitteet voidaan luonnollisesti esittää streameina: tapahtumat jonossa, datan lukeminen tiedostojärjestelmästä pala kerrallaan, kun koko datasetti ei mahdu muistiin, tai ajan mittaan saapuva verkkoliikenne. Koska streamit ovat futuureja, voimme käyttää niitä yhdessä muiden futuurien kanssa ja yhdistellä niitä eri tavoin. Voimme esimerkiksi kerätä tapahtumia vähentääksemme liiallisia verkkopyyntöjä, asettaa aikakatkaisuja pitkään kestäville operaatioille tai rajoittaa käytt...
```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-39/src/main.rs:throttle}}
```

Tässä asetamme aikakatkaisun `messages`-streamille `timeout`-metodilla. Sen jälkeen muokkaamme `while let` -silmukan runkoa, koska stream nyt palauttaa `Result`. `Ok`-variantti tarkoittaa, että viesti saapui ajoissa, kun taas `Err` tarkoittaa, että aikakatkaisu ehti kulua umpeen ennen kuin viesti saapui. Lopuksi käytämme `throttle`-metodia `intervals`-streamiin, jotta se ei ylikuormita `messages`-streamia.

---

Tämän luvun aikana olemme syventäneet ymmärrystämme asynkronisista streameista ja nähneet, kuinka voimme käyttää niitä tehokkaasti Rustissa.

