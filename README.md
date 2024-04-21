# TUTORIAL 8 ADPRO  (Event Driver Architecture)
#### Daffa Mohamad Fathoni (2206824161)
#### Advanced Programming B / GEN

<hr>

## REFLECTION

>a. How many data your publlsher program will send to the message broker in one
run?

Program publisher akan mengirimkan lima data (pesan) ke message broker dalam satu jalankan. Ini dapat dilihat dari lima panggilan `publish_event` di dalam fungsi `main`, tetapi dengan masing-masing memiliki data yang berbeda.

>b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber
program, what does it mean?

Ya, URL `"amqp://guest:guest@localhost:5672"` sama dengan yang digunakan dalam program subscriber. Ini berarti program publisher dan program subscriber akan menggunakan pengaturan yang sama untuk terhubung ke server AMQP.

- `amqp://` menunjukkan protokol yang digunakan, yaitu AMQP.
- `guest:guest` adalah kredensial login default yang digunakan. Dalam konteks ini, guest adalah nama pengguna dan guest adalah kata sandi.
- `localhost:5672` adalah alamat host dan port yang digunakan untuk terhubung ke server AMQP. Dalam hal ini, koneksi akan dilakukan ke mesin yang sama (`localhost`) di port standar AMQP (`5672`).

## Running RabbitMQ as message broker.
![image](https://github.com/fathonidf-Adpro/tutorial-1/assets/105644250/d9763999-653d-4b17-a172-bd1e42be2776)

## Monitoring chart based on publisher.
![image](https://github.com/fathonidf-Adpro/tutorial-8-publisher/assets/105644250/808293e4-21f0-45d2-91f2-91df64d50250)
Ketika di `cargo run` untuk subscriber dan publisher, maka akan muncul dalam RabbitMQ bahwa terdapat 1 subcriber memiliki koneksi pada message broker tersebut, terlihat pada gambar di atas bagian connection

Hal ini terjadi karena pada bagian code 
```rust
fn main() {
    let listener = CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
    _ = listener.listen("user_created".to_owned(),
        UserCreatedHandler{}, crosstown_bus::QueueProperties {
            auto_delete: false, durable: false, use_dead_letter: true });

    loop {
    }
}
```
mengakses message broker pada localhost untuk username dan password guest.

![image](https://github.com/fathonidf-Adpro/tutorial-8-publisher/assets/105644250/1a17fb7b-127a-4311-99c3-8b34a799d037)

Ketika dijalankan cargo run lagi pada publisher, maka akan terlihat pada gambar kanan bahwa ditampilkan (dalam hal ini println!()) 5 message berdasarkan kode

```rust
fn main() {
    let mut p = CrosstownBus::new_queue_publisher("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
    _ = p.publish_event("user_created".to_owned(),
        UserCreatedEventMessage { user_id: "1".to_owned(), user_name:"2206824161-Amir".to_owned() });
    _ = p.publish_event("user_created".to_owned(),
        UserCreatedEventMessage { user_id: "2".to_owned(), user_name:"2206824161-Budi".to_owned() });
    _ = p.publish_event("user_created".to_owned(),
        UserCreatedEventMessage { user_id: "3".to_owned(), user_name:"2206824161-Cica".to_owned() });
    _ = p.publish_event("user_created".to_owned(),
        UserCreatedEventMessage { user_id: "4".to_owned(), user_name:"2206824161-Dira".to_owned() });
    _ = p.publish_event("user_created".to_owned(),
        UserCreatedEventMessage { user_id: "5".to_owned(), user_name:"2206824161-Emir".to_owned() });
}
```
yang berada di publisher,
dan
```rust
impl MessageHandler<UserCreatedEventMessage> for UserCreatedHandler {
    fn handle(&self, message: Box<UserCreatedEventMessage>) -> Result<(), HandleError> {
        let ten_millis = time::Duration::from_millis(1000);
        let now = time::Instant::now();

        // thread::sleep(ten_millis);

        println!("In Dafton’s Computer [2206824161]. Message received: {:?}", message);
        Ok(())
    }

    fn get_handler_action(&self) -> String {
        "Handling user creation events".to_string()
    }
}
```
yang berada di subscriber.