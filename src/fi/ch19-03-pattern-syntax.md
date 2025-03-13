## Rustin mallinmukaisuuden syntaksi

Tässä luvussa tarkastelemme Rustin mallinmukaisuuden (pattern matching) monipuolista syntaksia. Mallit ovat tehokas tapa käsitellä erilaisia tietorakenteita ja arvoja.

### Perusmallit

Rust tukee yksinkertaisia malleja, kuten muuttujien purkamista:

```rust
let (x, y, z) = (1, 2, 3);
```

Tässä `(x, y, z)` vastaa annettua kolmiota `(1, 2, 3)`.

### Mallien käyttäminen `match`-lauseessa

```rust
match some_value {
    Some(x) => println!("Löytyi: {}", x),
    None => println!("Ei arvoa"),
}
```

### Alaviivan `_` käyttö

Jos haluamme jättää huomiotta arvon:

```rust
let (x, _, z) = (1, 2, 3); // Yllä oleva _ tarkoittaa, että keskimmäinen arvo jätetään huomiotta.
```

### Useiden arvojen käsittely yhdellä mallilla

Voimme yhdistää useita vaihtoehtoja käyttämällä `|`:

```rust
match some_value {
    1 | 2 | 3 => println!("1, 2 tai 3!"),
    _ => println!("Jokin muu"),
}
```

### Alueiden määrittely `..=`

Voimme käyttää alueita `..=`:

```rust
match some_value {
    1..=5 => println!("1-5 välillä!"),
    _ => println!("Jokin muu"),
}
```

### Rakenteiden mallintaminen

```rust
struct Point { x: i32, y: i32 }

let p = Point { x: 5, y: 10 };

match p {
    Point { x, y: 10 } => println!("x on {}, y on 10", x),
    _ => println!("Jokin muu"),
}
```

Tässä vain `y == 10` täsmää.

---

Tässä luvussa käsittelimme Rustin mallinmukaisuuden monipuolista syntaksia. Seuraavaksi tutkimme, kuinka voimme yhdistää nämä tekniikat tehokkaan koodin kirjoittamiseen.
