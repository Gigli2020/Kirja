# Unsafe Rust: Ohitetaan turvallisuustarkistukset

Rust on suunniteltu turvalliseksi ohjelmointikieleksi, mutta joskus ohjelmoijat tarvitsevat suoremman pääsyn muistiin tai haluavat optimoida suorituskyvyn. Tätä varten Rust tarjoaa `unsafe`-lohkot, joiden avulla voi ohittaa joitakin Rustin turvamekanismeja.

## Milloin käyttää `unsafe`-koodia?

Vaikka Rustin normaalit turvamekanismit estävät monia virheitä, joskus tarvitaan matalamman tason hallintaa. `unsafe`-koodi voi:

- Käyttää raakaviittauksia (`raw pointers`)
- Kutsua `unsafe`-merkittyjä funktioita
- Muokata muuttujia, jotka vaikuttavat moniin säikeisiin (`static mut`)
- Käyttää `unsafe`-ominaisuuksia, kuten `extern`-funktiokutsuja

## `unsafe`-lohkon käyttö

Voit käyttää `unsafe`-lohkoa seuraavasti:

```rust
let mut num = 5;

let r1 = &num as *const i32; // Raakaviittaus
let r2 = &mut num as *mut i32; // Muokattava raakaviittaus

unsafe {
    println!("r1 osoittaa: {}", *r1);
    *r2 = 10;
}
```

## `unsafe`-funktiot

Jos funktio voi aiheuttaa ongelmia, se voidaan merkitä `unsafe`:

```rust
unsafe fn vaarallinen_funktio() {
    println!("Tämä on unsafe-funktio!");
}

unsafe {
    vaarallinen_funktio();
}
```

## Johtopäätös

`unsafe`-koodi antaa paljon valtaa, mutta sitä tulee käyttää varoen. Rustin turvallisuusmekanismit estävät monia yleisiä virheitä, mutta tarvittaessa `unsafe` voi tarjota lisäsuorituskykyä ja hallintaa.

Seuraavaksi tutkimme kehittyneitä trait-ominaisuuksia ja tyyppijärjestelmän laajennuksia.
