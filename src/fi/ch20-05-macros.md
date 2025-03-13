# Makrot: Metaprogrammointi Rustissa

Rust tarjoaa makroja, joiden avulla voit kirjoittaa metaprogrammointia: koodia, joka kirjoittaa toista koodia. Rustin makrot eroavat perinteisistä funktioista monin tavoin, ja niitä käytetään monimutkaisten toistuvien tehtävien automatisointiin.

## Miksi makrot?

Makrot mahdollistavat:

- **Toistuvan koodin generoinnin** ilman tarvetta kirjoittaa samaa koodia useita kertoja.
- **Koodin laajentamisen käännösaikana** suorituskyvyn parantamiseksi.
- **Geneerisyyden ylittävän joustavuuden**, koska makrot voivat käsitellä erilaisia määriä argumentteja.

## Makrot vs. funktiot

Makrot voivat käsitellä muuttuvia määriä argumentteja ja suorittaa koodin käännösaikana, kun taas funktiot suoritetaan vasta ajonaikana.

## `macro_rules!`-makrot

Yksinkertaisin tapa luoda makro on käyttää `macro_rules!`. Esimerkiksi:

```rust
macro_rules! toista {
    ($val:expr) => {
        println!("Arvo: {}", $val);
    };
}

fn main() {
    toista!(10);
    toista!("Hei Rust!");
}
```

## Proseduraaliset makrot

Rust tukee myös proseduraalisia makroja, jotka toimivat funktioina ja muokkaavat ohjelman koodia. Näitä käytetään esimerkiksi derive-makrojen ja attribuuttimakrojen kanssa.

```rust
#[proc_macro]
pub fn tuplaa(input: TokenStream) -> TokenStream {
    // Muokataan syötettä ja palautetaan uusi TokenStream
}
```

## Johtopäätös

Makrot ovat tehokas työkalu Rustissa, mutta niitä tulee käyttää harkiten, koska ne voivat tehdä koodista vaikeammin luettavaa. Useimmiten geneeriset funktiot riittävät, mutta makrot ovat hyödyllisiä, kun tarvitaan koodin laajentamista käännösaikana.

---

Seuraavaksi tarkastelemme Rustin kehittyneitä ohjelmointitekniikoita käytännön sovelluksissa.
