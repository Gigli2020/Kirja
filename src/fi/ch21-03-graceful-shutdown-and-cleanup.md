# Hallittu sammutus ja siivous Rustin verkkopalvelimessa

Kun ohjelma lopetetaan, haluamme usein varmistaa, että kaikki resurssit vapautetaan oikein ja ettei mikään säie jää jumiin. Tässä osiossa lisäämme palvelimeemme hallitun sammuttamisen ja siivouksen (`graceful shutdown`).

## Hallitun sammuttamisen lisääminen

Rustissa voimme hallita säikeiden elinkaarta käyttämällä `Drop`-traitia. Tämä varmistaa, että kaikki palvelinsäikeet lopetetaan siististi, kun palvelin suljetaan.

```rust
use std::sync::{Arc, Mutex, mpsc};
use std::thread;
use std::net::TcpListener;
use std::net::TcpStream;

struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool {
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));

        let mut workers = Vec::with_capacity(size);

        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }

        ThreadPool { workers, sender: Some(sender) }
    }

    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap();
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());

        for worker in &mut self.workers {
            println!("Sammutetaan säie {}", worker.id);

            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv();

            match job {
                Ok(job) => {
                    println!("Worker {} suorittaa työn", id);
                    job();
                },
                Err(_) => {
                    println!("Worker {} lopettaa", id);
                    break;
                }
            }
        });

        Worker { id, thread: Some(thread) }
    }
}

type Job = Box<dyn FnOnce() + Send + 'static>;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);

    for stream in listener.incoming().take(5) {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }

    println!("Palvelin sulkeutuu.");
}

fn handle_connection(mut stream: TcpStream) {
    // Käsitellään yhteys
}
```

## Johtopäätös

Tässä osiossa lisäsimme verkkopalvelimeemme hallitun sammuttamisen, jolloin se voi vapauttaa kaikki resurssit turvallisesti ennen ohjelman päättymistä.

Nyt verkkopalvelimemme on valmis! Olemme rakentaneet tehokkaan ja monisäikeisen palvelimen, joka osaa käsitellä useita pyyntöjä samanaikaisesti ja sammua siististi.

---

Tämä oli Rust-kirjan viimeinen tekninen luku. Onnittelut Rustin opiskelusta!
