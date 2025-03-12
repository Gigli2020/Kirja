## Futuurit ja asynkroninen syntaksi

Rustin asynkronisen ohjelmoinnin keskeiset elementit ovat _futuurit_ sekä `async`- ja `await`-avainsanat.

_Futuuri_ on arvo, joka ei välttämättä ole vielä valmis, mutta valmistuu jossain vaiheessa tulevaisuudessa. (Tämä sama käsite esiintyy monissa ohjelmointikielissä eri nimillä, kuten _task_ tai _promise_.) Rust tarjoaa `Future`-traitin rakennuspalikaksi, jotta eri asynkroniset operaatiot voidaan toteuttaa eri tietorakenteilla mutta yhteisellä rajapinnalla. Rustissa futuurit ovat tyyppejä, jotka toteut...

Tämä kaikki voi tuntua abstraktilta, joten kirjoitetaan ensimmäinen asynkroninen ohjelmamme: pieni web-skripti, joka vastaanottaa kaksi URL-osoitetta komentoriviltä, hakee molemmat rinnakkain ja palauttaa ensimmäisen valmistuneen tuloksen.

## Ensimmäinen asynkroninen ohjelmamme

Aloitamme yksinkertaisella esimerkillä. Luodaan uusi Rust-projekti nimeltä `hello-async` ja lisätään siihen `trpl`-kirjasto:

```console
$ cargo new hello-async
$ cd hello-async
$ cargo add trpl
```

Tämän jälkeen kirjoitamme ohjelman, joka hakee kahden verkkosivun `<title>`-elementin ja tulostaa ensimmäisen valmistuneen.

### `page_title`-funktion määrittely

Kirjoitetaan funktio, joka ottaa yhden URL-osoitteen parametrina, tekee HTTP-pyynnön ja palauttaa HTML-sivun `<title>`-elementin tekstin:

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-01/src/main.rs:all}}
```

Tässä `async`-avainsana määrittää funktion asynkroniseksi. `trpl::get`-funktiota käytetään verkkosivun hakemiseen, ja `await` odottaa vastauksen valmistumista. Koska verkkosivun sisältö voi olla suuri, `text`-metodi on myös asynkroninen ja vaatii `await`-avainsanan.

Rustin futuurit ovat _laiskoja_: ne eivät tee mitään, ennen kuin niitä odotetaan `await`-avainsanalla. Tämä muistuttaa iteraattorien toimintaa Rustissa, jossa iteraattorit eivät tuota arvoja ennen kuin niitä kutsutaan.

### `await`-avainsanan sijainti

Rustin `await`-avainsana sijoitetaan odotettavan arvon _jälkeen_, ei ennen sitä, mikä eroaa joistakin muista kielistä. Tämä mahdollistaa ketjutettujen metodikutsujen kirjoittamisen siististi:

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-02/src/main.rs:chaining}}
```

Rustin `async fn` -funktio kääntyy itse asiassa tavalliseksi funktioksi, joka palauttaa futuurin. Esimerkiksi seuraava `async fn page_title` -funktio:

```rust
async fn page_title(url: &str) -> Option<String> {
    let text = trpl::get(url).await.text().await;
    Html::parse(&text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

on kääntäjälle sama asia kuin:

```rust
fn page_title(url: &str) -> impl Future<Output = Option<String>> {
    async move {
        let text = trpl::get(url).await.text().await;
        Html::parse(&text)
            .select_first("title")
            .map(|title| title.inner_html())
    }
}
```

Tämä osoittaa, kuinka Rust käsittelee asynkronisia funktioita erillisinä tilakoneina.

### Asynkroninen `main`-funktio ja ajoympäristöt

Rust ei salli `main`-funktion olevan asynkroninen suoraan:

```text
error[E0752]: `main` function is not allowed to be `async`
 --> src/main.rs:6:1
  |
6 | async fn main() {
  | ^^^^^^^^^^^^^^^ `main` function is not allowed to be `async`
```

Tämä johtuu siitä, että asynkroninen koodi vaatii _ajoympäristön_, joka hallitsee sen suorittamista. Koska Rust ei sisällä oletusarvoista async-ajoympäristöä, kehittäjän on valittava sellainen itse. Käytämme `trpl::run`-funktiota, joka käynnistää ajoympäristön ja suorittaa annetun futuurin loppuun:

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch17-async-await/listing-17-04/src/main.rs:run}}
```

Tämän jälkeen voimme suorittaa ohjelman:

```console
$ cargo run -- https://www.rust-lang.org
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `target/debug/async_await 'https://www.rust-lang.org'`
The title for https://www.rust-lang.org was
            Rust Programming Language
```

Nyt meillä on toimiva asynkroninen ohjelma!

### Kahden URL-osoitteen rinnakkainen haku

Lopuksi lisätään ohjelmaan tuki kahden verkkosivun rinnakkaiselle haulle käyttäen `trpl::race`-funktiota:

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch17-async-await/listing-17-05/src/main.rs:all}}
```

Tässä `trpl::race` valitsee ensimmäisen valmistuneen tuleman ja palauttaa sen.

Nyt olemme rakentaneet pienen toimivan web-skripti-ohjelman, joka hakee ensimmäisenä valmistuvan verkkosivun otsikon!

