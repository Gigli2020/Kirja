# Yleiset ohjelmointikäsitteet

Nyt kun olemme kirjoittaneet jo muutamia ohjelmia, tarkastellaan joitakin peruskonsepteja, joita kaikki ohjelmat käyttävät. 
Tässä luvussa käsittelemme muuttujia, tietotyyppejä, funktioita, ohjausrakenteita kuten haarautumisia (`if`-lausekkeet) ja silmukoita.

Jos sinulla on aikaisempaa ohjelmointikokemusta, huomaat, että nämä käsitteet ovat samankaltaisia monissa muissa kielissä, 
mutta Rustilla on omat erityispiirteensä, jotka tekevät siitä erilaisen.

Tässä luvussa opimme:

- Miten muuttujat ja muuttumattomuus toimivat Rustissa
- Miten Rustin tietotyypit käyttäytyvät
- Kuinka funktiot määritellään ja käytetään
- Miten ohjausrakenteet kuten `if`-lauseet ja silmukat toimivat

Rust antaa ohjelmoijille tarkkaa hallintaa suorituskyvystä ja turvallisuudesta, ja nämä ohjelmointikäsitteet muodostavat perustan 
Rustilla ohjelmoinnille. Tämä luku auttaa sinua ymmärtämään Rustin ainutlaatuisia ominaisuuksia ja miten voit hyödyntää niitä omissa ohjelmissasi.

Seuraavissa osioissa tutustumme Rustin **muuttujiin ja muuttumattomuuteen**.

> #### Avainsanat
>
> The Rust language has a set of _keywords_ that are reserved for use by the
> language only, much as in other languages. Keep in mind that you cannot use
> these words as names of variables or functions. Most of the keywords have
> special meanings, and you’ll be using them to do various tasks in your Rust
> programs; a few have no current functionality associated with them but have
> been reserved for functionality that might be added to Rust in the future. You
> can find a list of the keywords in [Appendix A][appendix_a]<!-- ignore -->.

[appendix_a]: appendix-01-keywords.md

>### Avainsanat

Rust-kielellä on joukko avainsanoja, jotka on varattu vain kielen käyttöön, kuten monissa muissakin ohjelmointikielissä. Muista, että et voi käyttää näitä sanoja muuttujien tai funktioiden niminä. Useimmilla avainsanoilla on erityinen merkitys, ja käytät niitä erilaisiin tehtäviin Rust-ohjelmissasi. Joillakin avainsanoilla ei tällä hetkellä ole mitään toiminnallisuutta, mutta ne on varattu tulevaisuuden mahdollisia lisäyksiä varten.

Voit löytää listan avainsanoista liitteestä A.


