# Monisäikeinen verkkopalvelin Rustilla

Edellisessä osiossa rakensimme yksisäikeisen verkkopalvelimen, joka käsittelee yhden yhteyden kerrallaan. Nyt lisäämme rinnakkaisuuden käyttämällä säikeitä ja säikeiden hallintajärjestelmää (thread pool).

## Säikeiden käyttö verkkopalvelimessa

Yksisäikeinen palvelin toimii hyvin yksinkertaisissa tilanteissa, mutta monimutkaisemmissa sovelluksissa haluamme palvella useita asiakkaita samanaikaisesti. Tätä varten käytämme Rustin säikeitä (`std::thread`) sekä viestinvälitystä (`std::sync::mpsc`).

Seuraavassa esimerkissä laajennamme palvelinta lisäämällä säikeet ja viestinvälityksen:

```rust
use std::thread;
use std::sync::{mpsc, Arc, Mutex};
use std::net::TcpListener;
use std::net::TcpStream;

struct ThreadPool {
    workers: Vec<Worker>,
    sender: mpsc::Sender<Job>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool {
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));

        let mut workers = Vec::with_capacity(size);

        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }

        ThreadPool { workers, sender }
    }

    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.send(job).unwrap();
    }
}

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();
            println!("Worker {} suorittaa työn", id);
            job();
        });

        Worker { id, thread: Some(thread) }
    }
}

type Job = Box<dyn FnOnce() + Send + 'static>;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }
}

fn handle_connection(mut stream: TcpStream) {
    // Käsitellään yhteys kuten aiemmassa esimerkissä
}
```

## Yhteenveto

Tässä osiossa lisäsimme monisäikeisyyden verkkopalvelimeen käyttämällä säikeiden hallintajärjestelmää. Nyt palvelimemme voi käsitellä useita yhteyksiä samanaikaisesti, mikä parantaa suorituskykyä ja skaalautuvuutta.

Seuraavaksi tutkimme Rustin loppuyhteenvetoa ja lisäresursseja.
