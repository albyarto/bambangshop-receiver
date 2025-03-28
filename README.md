# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
> In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

RwLock<> digunakan untuk melakukan sinkronisasi akses ke *Vec* dari *Notifications* agar beberapa thread dapat membaca data secara bersamaan tanpa saling mengganggu. Hal ini memberikan keuntungan karena proses membaca jauh lebih umum terjadi daripada proses penulisan. RwLock<> memungkinkan banyak *reader* (pembaca) berjalan secara paralel, tetapi hanya mengizinkan satu *writer* (penulis) pada satu waktu.  
Jika kita menggunakan Mutex<> sebagai gantinya, akses ke *Vec* akan sepenuhnya *blocking*, baik saat membaca maupun menulis. Hal ini membuat proses kurang efisien karena hanya satu thread yang bisa mengakses data dalam satu waktu, bahkan ketika hanya terjadi operasi baca. Oleh karena itu, RwLock<> lebih sesuai karena memberikan fleksibilitas dan kinerja yang lebih baik saat ada banyak operasi baca.

> In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Rust tidak mengizinkan kita langsung memodifikasi static variable tanpa menggunakan mekanisme seperti RwLock<> atau Mutex<> karena Rust dirancang untuk menjaga *safety* (keamanan memori dan data) dengan menghindari *data race*. Dalam Java, kita bisa memodifikasi static variable dengan static function, tetapi pendekatan ini berisiko menyebabkan kondisi balapan (*race condition*). Rust menghindari risiko ini dengan memastikan bahwa akses ke static variable yang dapat berubah harus disinkronisasi dengan alat sinkronisasi eksplisit seperti RwLock<> atau Mutex<>. Dengan demikian, Rust memberikan jaminan keamanan dan ketahanan terhadap *threading* issues, yang tidak selalu terjamin dalam bahasa seperti Java.

#### Reflection Subscriber-2

> Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Ya, saya telah mengeksplorasi file *src/lib.rs* dan mempelajari beberapa bagian tambahan dari kode tersebut. Salah satu hal yang menarik dari eksplorasi ini adalah penggunaan *lazy_static* untuk mendeklarasikan *REQWEST_CLIENT* dan *APP_CONFIG* sebagai variabel static. Pendekatan ini membuat client HTTP dan konfigurasi aplikasi tetap tersedia di seluruh bagian aplikasi tanpa perlu mendeklarasikannya kembali di berbagai tempat. Selain itu, saya juga mempelajari bagaimana *AppConfig* mengambil variabel lingkungan dari file `.env` menggunakan pustaka `dotenvy` dan mengintegrasikannya dengan *Figment*, yang mempermudah konfigurasi berbasis lingkungan.

> Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Observer Pattern sangat memudahkan penambahan *subscriber* baru karena setiap *subscriber* hanya perlu melakukan *subscribe* ke tipe produk tertentu, dan sistem akan secara otomatis mengirimkan notifikasi ketika terjadi perubahan terkait produk tersebut. Hal ini memungkinkan penerapan yang fleksibel, karena kita hanya perlu menambah *subscriber* tanpa mengubah logika inti dari Main app.  

Jika kita ingin menambahkan lebih dari satu instance Main app, meskipun ini mungkin sedikit lebih rumit daripada menambahkan *subscriber*, tetap bisa dilakukan dengan menyesuaikan konfigurasi jaringan dan URL *root* yang sesuai pada masing-masing instance. Dengan skema notifikasi HTTP berbasis endpoint, menambahkan instance Main app tambahan tidak akan terlalu sulit, selama endpoint Main app dapat diakses oleh *subscriber*.


> Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Saya telah mencoba meningkatkan dokumentasi Postman Collection dengan menambahkan deskripsi yang lebih jelas untuk setiap endpoint, *subscribe*, *unsubscribe*, dan *list*. Dokumentasi tambahan ini berguna, terutama saat mencoba memahami bagaimana berbagai bagian aplikasi saling berkomunikasi. Selain itu, dokumentasi ini membantu saat melakukan debug karena memudahkan dalam melacak aliran data dan respon dari aplikasi.