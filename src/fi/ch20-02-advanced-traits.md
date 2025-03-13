## Kehittyneet trait-ominaisuudet

Luvussa 10 käsittelimme traitien perusominaisuuksia, mutta Rust tarjoaa myös kehittyneempiä tapoja hyödyntää niitä. Tässä luvussa syvennymme:

- **Assosioidut tyypit**: Traitit voivat määritellä tyypin paikanpitäjän, jonka toteuttajat täyttävät.
- **Oletusarvoiset geneeriset tyypit**: Traitit voivat tarjota oletusarvoja geneerisille parametreille.
- **Operaatioiden ylikuormitus (`operator overloading`)**: Voit mukauttaa operaatioiden (kuten `+`) toimintaa.
- **Tarkasti määritetty syntaksi (`fully qualified syntax`)**: Määritetään tarkasti, minkä traitin metodia kutsutaan, kun usealla traitilla on sama metodi.
- **Ylitraitit (`supertraits`)**: Trait voi vaatia, että tyyppi toteuttaa myös toisen traitin.
- **Newtype-malli (`newtype pattern`)**: Toteutetaan ulkoinen trait ulkoiselle tyypille uuden wrapper-tyypin avulla.

### Assosioidut tyypit

Assosioidut tyypit mahdollistavat tyypin määrittelyn traitin sisällä:

```rust
trait Esimerkki {
    type Tyyppi;

    fn palauta(&self) -> Self::Tyyppi;
}

struct Luku;

impl Esimerkki for Luku {
    type Tyyppi = i32;

    fn palauta(&self) -> Self::Tyyppi {
        42
    }
}
```

Tämä mahdollistaa traitin toteuttamisen eri tavoin ilman geneeristen parametrien käyttöä.

### Traitien oletustoteutusten ohittaminen

Voit määritellä oletusmetodeja traitille ja ohittaa ne tarvittaessa:

```rust
trait Tervehdys {
    fn sano_hei(&self) {
        println!("Hei!");
    }
}

struct Henkilö;

impl Tervehdys for Henkilö {
    fn sano_hei(&self) {
        println!("Moi!");
    }
}
```

Tässä `Henkilö`-tyyppi ohittaa oletustoteutuksen ja määrittelee oman version metodista.

### Operaatioiden ylikuormitus

Rust ei salli omien operaattorien luontia, mutta standardikirjasto tarjoaa operaatioihin liittyviä traitteja, kuten `std::ops::Add`. Voit esimerkiksi ylikuormittaa `+`-operaattorin kahden `Point`-instanssin yhteenlaskemiseksi:

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 1, y: 2 };
    let p2 = Point { x: 3, y: 4 };
    let p3 = p1 + p2;
    println!("{:?}", p3); // Tulostaa: Point { x: 4, y: 6 }
}
```

### Tarkasti määritetty syntaksi

Jos eri traitit tai tyyppi itse määrittelevät saman nimisen metodin, Rust vaatii tarkempaa syntaksia oikean metodin valintaan:

```rust
trait Pilotti {
    fn aja(&self);
}

trait Kuljettaja {
    fn aja(&self);
}

struct Henkilö;

impl Pilotti for Henkilö {
    fn aja(&self) {
        println!("Lentäjä ajaa lentokonetta!");
    }
}

impl Kuljettaja for Henkilö {
    fn aja(&self) {
        println!("Kuljettaja ajaa autoa!");
    }
}

impl Henkilö {
    fn aja(&self) {
        println!("Henkilö kävelee!");
    }
}

fn main() {
    let henkilö = Henkilö;

    henkilö.aja(); // Kutsuu Henkilö-tyypin metodia

    // Kutsutaan tarkasti määritellyllä syntaksilla
    Pilotti::aja(&henkilö);
    Kuljettaja::aja(&henkilö);
}
```

Tässä `Henkilö` voi käyttää eri traitien `aja`-metodeja käyttämällä `Pilotti::aja(&henkilö)`-syntaksia.

### Ylitraitit (`Supertraits`)

Joskus trait tarvitsee toisen traitin metodien käyttöä. Esimerkiksi `OutlinePrint`-traitin käyttö vaatii, että tyyppi toteuttaa myös `Display`-traitin:

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();
        println!("**********");
        println!("*        *");
        println!("* {} *", output);
        println!("*        *");
        println!("**********");
    }
}

struct Point {
    x: i32,
    y: i32,
}

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl OutlinePrint for Point {}

fn main() {
    let p = Point { x: 1, y: 3 };
    p.outline_print();
}
```

### Newtype-malli (`Newtype Pattern`)

Rustin orpopelisääntö (`orphan rule`) estää meitä toteuttamasta ulkoisia traitteja ulkoisille tyypeille suoraan. Tämä voidaan kiertää luomalla uusi wrapper-tyyppi:

```rust
use std::fmt;

struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

fn main() {
    let lista = Wrapper(vec![String::from("Hei"), String::from("Maailma")]);
    println!("{}", lista); // Tulostaa: [Hei, Maailma]
}
```

Tässä `Wrapper` mahdollistaa `Display`-traitin toteuttamisen `Vec<String>`-tyypille ilman, että tarvitsee muuttaa `Vec<String>`:n toteutusta.

---

Tässä luvussa käsittelimme edistyneitä trait-ominaisuuksia Rustissa, kuten assosioituja tyyppejä, operaatioiden ylikuormitusta, tarkasti määriteltyä syntaksia, ylitraitteja ja newtype-mallia. Seuraavaksi tutkimme Rustin kehittyneitä tyypinhallintatekniikoita.
