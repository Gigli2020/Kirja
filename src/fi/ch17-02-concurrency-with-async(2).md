# Asynkroninen rinnakkaisuus Rustissa

Rustin asynkroninen ohjelmointi mahdollistaa tehokkaan rinnakkaisen suorituksen ilman perinteisten säikeiden monimutkaisuutta. Tässä luvussa käsittelemme:

- **Futuureja** ja niiden ajoa asynkronisessa ympäristössä.
- **Asynkronisia tehtäviä (`async` ja `await`)**, jotka mahdollistavat ei-estävän ohjelmoinnin.
- **Viestinvälitystä (`tokio::sync::mpsc`)**, jonka avulla eri tehtävät voivat kommunikoida keskenään.

## `async`-funktioiden käyttö

Asynkroniset funktiot määritellään `async fn` -avainsanalla:

```rust
async fn esimerkki() -> i32 {
    10
}

fn main() {
    let tulevaisuus = esimerkki();
    println!("Tulevaisuus luotu, mutta sitä ei ole vielä suoritettu!");
}
```

Tässä `esimerkki` ei suoritu ennen kuin sitä **awaitataan**:

```rust
use tokio::main;

#[tokio::main]
async fn main() {
    let arvo = esimerkki().await;
    println!("Tulevaisuus suoritettu! Arvo: {}", arvo);
}
```

## Asynkronisten tehtävien rinnakkaisuus

Rustin `tokio`-kirjasto mahdollistaa useiden asynkronisten tehtävien suorittamisen rinnakkain:

```rust
use tokio::task;

#[tokio::main]
async fn main() {
    let tehtävä1 = task::spawn(async {
        println!("Tehtävä 1 alkaa");
    });

    let tehtävä2 = task::spawn(async {
        println!("Tehtävä 2 alkaa");
    });

    tehtävä1.await.unwrap();
    tehtävä2.await.unwrap();
}
```

## Viestinvälitys `mpsc`-kanavalla

Asynkronisessa ohjelmoinnissa tehtävät voivat lähettää tietoa toisilleen `mpsc`-kanavan avulla:

```rust
use tokio::sync::mpsc;

#[tokio::main]
async fn main() {
    let (tx, mut rx) = mpsc::channel(32);

    tokio::spawn(async move {
        tx.send("Terve maailmasta!").await.unwrap();
    });

    if let Some(viesti) = rx.recv().await {
        println!("Vastaanotettu: {}", viesti);
    }
}
```

## Johtopäätös

Tässä luvussa opimme Rustin asynkronisen ohjelmoinnin perusteet. `async` ja `await` mahdollistavat tehokkaan ei-estävän suorituksen, kun taas `tokio`-kirjasto auttaa hallitsemaan tehtäviä ja viestinvälitystä.

Seuraavaksi tutkimme, kuinka voimme yhdistää nämä työkalut suurempiin asynkronisiin järjestelmiin.
