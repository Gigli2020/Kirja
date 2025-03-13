# Yksisäikeinen verkkopalvelin Rustilla

Tässä osiossa rakennamme yksisäikeisen verkkopalvelimen Rustilla. Tämä antaa pohjan verkkosovellusten kehittämiselle ennen kuin lisäämme rinnakkaisuutta myöhemmin.

## Verkkopalvelimen perustan luominen

Aloitetaan luomalla yksinkertainen palvelin, joka kuuntelee verkkoyhteyksiä ja vastaa yksinkertaisella HTML-sivulla.

```rust
use std::net::TcpListener;
use std::net::TcpStream;
use std::io::prelude::*;
use std::fs;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer).unwrap();

    let contents = fs::read_to_string("hello.html").unwrap();

    let response = format!(
        "HTTP/1.1 200 OK
Content-Length: {}

{}",
        contents.len(),
        contents
    );

    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

## Yhteenveto

Tässä osiossa loimme yksisäikeisen verkkopalvelimen, joka pystyy palvelemaan staattisia HTML-tiedostoja. Seuraavaksi lisäämme rinnakkaisuuden, jotta palvelin voi käsitellä useita yhteyksiä samanaikaisesti.
