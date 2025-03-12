# Peloton rinnakkaisuus

Rinnakkaisen ohjelmoinnin turvallinen ja tehokas käsittely on yksi Rustin tärkeimmistä tavoitteista. _Rinnakkainen ohjelmointi_, jossa ohjelman eri osat suoritetaan itsenäisesti, ja _rinnakkaisohjelmointi_, jossa ohjelman eri osat suoritetaan samanaikaisesti, ovat yhä tärkeämpiä, kun useammat tietokoneet hyödyntävät moniydinsuorittimiaan. Historiallisesti ohjelmointi näissä yhteyksissä on ollut vaikeaa ja virhealtista. Rust pyrkii muuttamaan tämän.

Aluksi Rustin kehitystiimi ajatteli, että muistin turvallisuuden varmistaminen ja rinnakkaisuusongelmien estäminen olivat kaksi erillistä haastetta, jotka pitäisi ratkaista eri menetelmillä. Ajan myötä tiimi havaitsi, että omistajuus- ja tyyppijärjestelmät ovat tehokkaita työkaluja sekä muistin turvallisuuden että rinnakkaisuusongelmien hallintaan! Hyödyntämällä omistajuutta ja tyyppitarkistuksia monet rinnakkaisuusvirheet Rustissa ovat kääntöaikaisia virheitä eivätkä ajonaikaisia virheitä.  

Tämän ansiosta sinun ei tarvitse käyttää paljon aikaa ajonaikaisen rinnakkaisuusvirheen toistamiseen – virheellinen koodi ei yksinkertaisesti käänny ja antaa virheilmoituksen, joka selittää ongelman. Näin voit korjata koodisi jo kehitysvaiheessa, sen sijaan että joutuisit selvittämään ongelmia vasta tuotannossa. Olemme nimenneet tämän Rustin ominaisuuden _pelottomaksi rinnakkaisuudeksi_.  

Peloton rinnakkaisuus mahdollistaa koodin kirjoittamisen ilman hienovaraisia virheitä ja tekee refaktoroinnista helpompaa ilman uusien virheiden syntymistä.

> **Huom:** Yksinkertaisuuden vuoksi käytämme tässä luvussa termiä _rinnakkainen_ tarkoittamaan sekä rinnakkaista että samanaikaista suorittamista. Mikäli tämä kirja käsittelisi erityisesti rinnakkaisuutta ja/tai samanaikaisuutta, käyttäisimme tarkempaa terminologiaa. Tässä luvussa voit mielessäsi korvata sanan _rinnakkainen_ termillä _rinnakkainen ja/tai samanaikainen_.

Monet ohjelmointikielet tarjoavat vain tiettyjä ratkaisuja rinnakkaisuusongelmiin. Esimerkiksi Erlang tukee viestinvälitykseen perustuvaa rinnakkaisuutta, mutta tilan jakaminen säikeiden välillä on siinä monimutkaista. Korkean tason ohjelmointikielille on järkevää tarjota vain rajoitettu joukko ratkaisuja, koska ne hyödyntävät abstraktioita ja luopuvat osasta kontrollia helpottaakseen kehittämistä.  

Matalamman tason ohjelmointikieliltä odotetaan kuitenkin parempaa suorituskykyä eri tilanteissa ja vähemmän abstraktioita laitteiston päällä. Siksi Rust tarjoaa joukon erilaisia työkaluja, joiden avulla voit valita parhaan ratkaisun omiin tarpeisiisi ja ohjelmointikontekstiisi.

Tässä luvussa käsittelemme seuraavia aiheita:

- Kuinka luoda säikeitä suorittamaan useita koodin osia samanaikaisesti
- _Viestinvälitykseen perustuva rinnakkaisuus_, jossa säikeet lähettävät viestejä toisilleen kanavien kautta
- _Jaetun tilan rinnakkaisuus_, jossa useat säikeet voivat käyttää samaa tietoa
- `Sync`- ja `Send`-traitit, jotka laajentavat Rustin rinnakkaisuustakuut koskemaan sekä standardikirjaston että käyttäjän itse määrittelemiä tyyppejä
