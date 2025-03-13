# Sisäinen muuttuvuus: Muokattavien arvojen säilyttäminen muuttumattomuuden sisällä

Rustin `mut`-avainsana hallitsee sitä, voimmeko muuttaa tietyn muuttujan sisältöä. Usein haluamme kuitenkin jakaa tietorakenteen muuttamattoman viitteen useille osille ohjelmaa, mutta silti pystyä muokkaamaan sen sisältöä tietyissä olosuhteissa. **Sisäinen muuttuvuus** (_interior mutability_) mahdollistaa tämän.

## `RefCell<T>`:n käyttö

Rustin `RefCell<T>`-tyyppi mahdollistaa muuttuvan datan käsittelyn ilman, että koko rakenne tarvitsee olla `mut`.

```rust
use std::cell::RefCell;

struct Laskuri {
    laskut: RefCell<i32>,
}

impl Laskuri {
    fn uusi() -> Laskuri {
        Laskuri {
            laskut: RefCell::new(0),
        }
    }

    fn lisää(&self) {
        *self.laskut.borrow_mut() += 1;
    }

    fn arvo(&self) -> i32 {
        *self.laskut.borrow()
    }
}

fn main() {
    let laskuri = Laskuri::uusi();
    laskuri.lisää();
    laskuri.lisää();
    println!("Laskurin arvo: {}", laskuri.arvo());
}
```

## Miksi käyttää `RefCell<T>`?

- Se mahdollistaa **muokattavan tilan** sisällä `struct`, joka itsessään ei ole `mut`.
- `RefCell<T>` sallii **juuri-in-aika -lainauksen** (_runtime borrowing_), jossa virheet havaitaan ajonaikana.

## `Rc<T>` ja `RefCell<T>` yhdessä

`Rc<T>` mahdollistaa tiedon jakamisen useille omistajille, ja `RefCell<T>` antaa mahdollisuuden muokata sitä:

```rust
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
struct Solmu {
    arvo: i32,
    seuraava: Option<Rc<RefCell<Solmu>>>,
}

fn main() {
    let ensimmäinen = Rc::new(RefCell::new(Solmu { arvo: 1, seuraava: None }));
    let toinen = Rc::new(RefCell::new(Solmu { arvo: 2, seuraava: Some(Rc::clone(&ensimmäinen)) }));

    println!("Toinen solmu: {:?}", toinen);
}
```

## Johtopäätös

Sisäinen muuttuvuus tarjoaa voimakkaan tavan käsitellä muuttuvaa dataa muuttumattomuuden sisällä. Se mahdollistaa turvallisen rinnakkaiskäytön tietyissä tilanteissa, mutta sen käyttö tulee tehdä harkiten, sillä ajonaikaiset lainaustarkistukset voivat aiheuttaa virheitä.

---

Tämä oli Rustin sisäisen muuttuvuuden perusteet! Seuraavaksi jatkamme Rustin kehittyneempiin aiheisiin.
