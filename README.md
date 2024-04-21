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