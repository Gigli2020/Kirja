# Rust-ohjelmointikieli

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

Tämä arkisto sisältää The Rust Programming Language -kirjan lähdekoodin.

[Kirja on saatavilla painettuna No Starch Pressin kautta][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

Voit myös lukea kirjan ilmaiseksi verkossa. Tarkista kirjan versio uusimmasta [stable], [beta], tai [nightly] Rust-julkaisusta. Huomaa, että näiden versioiden virheitä on saatettu jo korjata tässä arkistossa, koska julkaisut päivittyvät harvemmin.


[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

Lataa kaikki kirjassa esiintyvät koodilistaukset [releases]-osiosta.

[releases]: https://github.com/rust-lang/book/releases

## Vaatimukset

Kirjan kokoaminen vaatii [mdBook]-työkalun, mielellään version mitä
rust-lang/rust käyttää tässä [tässä tiedostossa][rust-mdbook]. Saat sen täältä:

[mdBook]: https://github.com/rust-lang/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --locked --version <version_num>
```

Kirja käyttää myös kahta mdbook-lisäosaa jotka ovat osa tätä arkistoa. Jos et asenna niitä, saat varoituksia rakennusvaiheessa, ja lopputulos ei näytä oikealta, mutta voit silti koota kirjan. Ota lisäosat käyttöön asentamalla ne näin:

```bash
$ cargo install --locked --path packages/mdbook-trpl
```

## Building

To build the book, type:

```bash
$ mdbook build
```

The output will be in the `book` subdirectory. To check it out, open it in
your web browser.

_Firefox:_

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

To run the tests:

```bash
$ cd packages/trpl
$ mdbook test --library-path packages/trpl/target/debug/deps
```

## Contributing

We'd love your help! Please see [CONTRIBUTING.md][contrib] to learn about the
kinds of contributions we're looking for.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Because the book is [printed][nostarch], and because we want
to keep the online version of the book close to the print version when
possible, it may take longer than you're used to for us to address your issue
or pull request.

So far, we've been doing a larger revision to coincide with [Rust Editions](https://doc.rust-lang.org/edition-guide/). Between those larger
revisions, we will only be correcting errors. If your issue or pull request
isn't strictly fixing an error, it might sit until the next time that we're
working on a large revision: expect on the order of months or years. Thank you
for your patience!

### Käännökset

Haluaisimme apua kirjan kääntämisessä! Katso [Translations]-tunniste liittyäksesi käynnissä oleviin käännösprojekteihin. Jos haluat aloittaa uuden kielen käännöksen, avaa uusi issue! Odotamme [mdbook support] monikielistä tukea ennen kuin voimme yhdistää käännöksiä virallisesti, mutta voit silti aloittaa työn.

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang/mdBook/issues/5

## Oikoluku

Voit tarkistaa lähdetiedostojen oikeinkirjoituksen käyttämällä spellcheck.sh-skriptiä, joka löytyy ci-hakemistosta. Se käyttää oikeiden sanojen sanastoa, joka on tallennettu ci/dictionary.txt-tiedostoon.

Jos skripti tuottaa väärän positiivisen virheen (esim. käytit sanaa BTreeMap, jota skripti ei tunnista), lisää kyseinen sana ci/dictionary.txt-tiedostoon (pidä sanat aakkosjärjestyksessä).
