**a.** Program publisher mengirim 5 *message* ke *message broker* dalam satu kali *run*. Masing-masing *message* berisi objek `UserCreatedEventMessage` yang memiliki dua *field* bertipe `String`: `user_id` dan `user_name`. Karena serialisasi menggunakan Borsh, ukuran total data tergantung panjang isi string, tetapi secara kasar, setiap *message* berisi sekitar 4 byte untuk panjang string + isi string itu sendiri. Estimasi kasar total data yang dikirim sekitar 200–300 byte.

**b.** URL `amqp://guest:guest@localhost:5672` pada publisher dan subscriber menunjukkan bahwa keduanya terhubung ke *message broker* (RabbitMQ) yang sama, yaitu yang berjalan secara lokal di komputer (`localhost`) melalui *port* 5672, menggunakan kredensial default (`guest:guest`). Ini berarti publisher mengirim pesan ke broker yang sama dengan yang digunakan subscriber untuk menerima pesan tersebut.

## Running RabbitMQ as message broker.
![Running RabbitMQ](./images/running-rabbitmq.jpg)

## Sending and Processing Event

![/run-publisher](./images/run-publisher-console.jpg)

![/run-subscriber](./images/run-subscriber-console.jpg)

Ketika dijalankan `cargo run` pada project `publisher`, program ini mengirimkan 5 event bertipe `UserCreatedEventMessage` ke RabbitMQ melalui koneksi `amqp://guest:guest@localhost:5672`. 

Kemudian, `subscriber` yang sudah berjalan sebelumnya akan menerima semua event tersebut dan langsung memprosesnya menggunakan handler `UserCreatedHandler`. Setiap pesan dicetak ke terminal sebagai bukti bahwa proses komunikasi melalui message broker berhasil.

Kedua aplikasi publisher dan subscriber menggunakan koneksi AMQP yang sama untuk berkomunikasi dengan RabbitMQ.