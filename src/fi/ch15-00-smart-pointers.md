# Älykkäät osoittimet

_Osoitin_ on yleinen käsite muuttujalle, joka sisältää muistiosoitteen. Tämä osoite viittaa johonkin toiseen dataan. Rustin yleisin osoitin on viite, johon tutustuit luvussa 4. Viitteet merkitään `&`-symbolilla, ja ne lainaavat arvoa, johon ne viittaavat. Viitteillä ei ole erityisiä ominaisuuksia viittauksen lisäksi, eikä niistä aiheudu lisäkustannuksia.

_Älykkäät osoittimet_ puolestaan ovat tietorakenteita, jotka toimivat kuin osoittimet, mutta sisältävät myös lisätietoa ja -ominaisuuksia. Älykkäät osoittimet eivät ole Rustille ainutlaatuisia: ne ovat peräisin C++:sta ja niitä löytyy myös muista ohjelmointikielistä. Rustin standardikirjasto tarjoaa useita älykkäitä osoittimia, jotka tarjoavat enemmän toiminnallisuuksia kuin tavalliset viitteet. Yleisestä konseptista esimerkkinä tarkastelemme muutamaa erilaista älykästä osoitinta, mukaan lukien _viittauslaskennan...

Rustin omistajuus- ja lainausmallin vuoksi sillä on erityinen ero viitteiden ja älykkäiden osoittimien välillä: kun viitteet vain lainaavat dataa, älykkäät osoittimet _omistavat_ viittaamansa datan monissa tapauksissa.

Vaikka emme ole aiemmin kutsuneet niitä tällä nimellä, olemme jo tavanneet muutamia älykkäitä osoittimia tässä kirjassa, kuten `String`- ja `Vec<T>`-tyypit luvussa 8. Molemmat kuuluvat älykkäisiin osoittimiin, koska ne omistavat muistia ja mahdollistavat sen käsittelyn. Ne sisältävät myös metatietoa ja tarjoavat ylimääräisiä ominaisuuksia tai takuita. Esimerkiksi `String` tallentaa kapasiteettinsa metatietona ja varmistaa, että sen data on aina kelvollista UTF-8-muotoista tekstiä.

Älykkäät osoittimet toteutetaan yleensä structien avulla. Toisin kuin tavalliset structit, älykkäät osoittimet toteuttavat `Deref`- ja `Drop`-traitit. `Deref`-trait mahdollistaa sen, että älykkään osoittimen instanssia voidaan käyttää kuten viitettä, jolloin koodi voi käsitellä sekä viitteitä että älykkäitä osoittimia samalla tavalla. `Drop`-trait puolestaan mahdollistaa mukautetun koodin suorittamisen, kun älykkään osoittimen instanssi poistuu laajuudesta. Tässä luvussa käsittelemme molempia näitä traittej...

Älykkäät osoittimet ovat Rustissa laajalti käytetty suunnittelumalli, mutta emme käsittele kaikkia mahdollisia toteutuksia. Monet kirjastot sisältävät omia älykkäitä osoittimia, ja voit jopa kirjoittaa omasi. Käymme kuitenkin läpi yleisimmät standardikirjaston tarjoamat älykkäät osoittimet:

- `Box<T>` arvojen allokointiin keolle  
- `Rc<T>`, viittauslaskentatyyppi, joka mahdollistaa usean omistajan  
- `Ref<T>` ja `RefMut<T>`, joita käytetään `RefCell<T>`-tyypin kautta, ja jotka pakottavat lainaussäännöt noudatettavaksi ajon aikana käännösvaiheen sijaan  

Lisäksi käsittelemme _sisäisen mutabiliteetin_ mallia, jossa muuttumaton tyyppi tarjoaa API:n sisäisen arvon muuttamiseen. Käymme myös läpi _viittauskehäongelman_, joka voi aiheuttaa muistivuotoja, ja opimme keinoja niiden estämiseen.

Aloitetaan!
