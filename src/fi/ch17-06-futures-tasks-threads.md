## Yhdistetään kaikki: Futuureja, tehtäviä ja säikeitä

Kuten näimme [luvussa 16](ch16-00-concurrency.html), säikeet tarjoavat yhden lähestymistavan rinnakkaisuuteen. Tässä luvussa olemme tutkineet toista lähestymistapaa: asynkronisuutta, joka perustuu futuureihin ja streameihin. Kysymys kuuluu: milloin pitäisi käyttää säikeitä ja milloin asynkronisuutta? Vastaus on: _se riippuu_. Monissa tapauksissa ei ole kyse valinnasta säikeiden _tai_ asynkronisuuden välillä, vaan niiden yhdistelmästä.

Monet käyttöjärjestelmät ovat tukeneet säikeisiin perustuvaa rinnakkaisuu...

### Säikeiden ja asynkronisten tehtävien vertailu

Vaikka säikeet ja asynkroniset tehtävät näyttävät pinnallisesti samanlaisilta, niiden toiminta on hyvin erilaista. Yksinkertaisessa esimerkissä voi olla vaikea havaita eroa, mutta jos loisimme miljoonia asynkronisia tehtäviä modernilla tietokoneella, se onnistuisi ilman ongelmia. Jos taas yrittäisimme luoda miljoona säiettä, tietokoneen muisti loppuisi kesken!

Tämä ei tarkoita, että asynkroniset tehtävät ovat aina parempia kuin säikeet (tai päinvastoin). Sä...

### Yhteenveto

Tämä ei ole viimeinen kerta, kun käsittelemme rinnakkaisuutta tässä kirjassa. [Luvussa 21](ch21-00-final-project-a-web-server.html) sovellamme näitä käsitteitä realistisemmassa tilanteessa ja vertailemme suoraan, miten ongelmat voidaan ratkaista säikeillä verrattuna asynkronisiin tehtäviin.

Riippumatta siitä, minkä näistä lähestymistavoista valitset, Rust tarjoaa työkalut, joilla voit kirjoittaa turvallista, nopeaa ja rinnakkaista koodia—oli kyseessä sitten korkean suorituskyvyn verkkopalvelin tai sulautettu käyttöjärjestelmä.

