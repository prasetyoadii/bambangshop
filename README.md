# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?** <BR> 
Dalam konteks BambangShop, penggunaan interface atau trait untuk menggambarkan Subscriber tidak diperlukan karena semua Subscriber diwakili oleh satu class dengan perilaku yang sama. Sebuah single model struct sudah cukup untuk menyimpan data observer dan perilaku yang diperlukan untuk memberi tahu. Dengan tidak adanya variasi perilaku yang diharapkan dari observer, penggunaan sebuah model struct menjadi pendekatan yang tepat dan memadai.

2. **id in Product and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?** <BR>
Pada kasus di mana id dan url harus unik, penggunaan DashMap (map/dictionary) direkomendasikan daripada Vec (list). DashMap memungkinkan penggunaan key unik untuk operasi insert, lookup, dan deletion dengan efisiensi tinggi. Hal ini lebih menguntungkan daripada Vec, terutama jika terdapat kemungkinan kesamaan antara URL dan id, yang akan memerlukan dua Vec terpisah untuk menyimpan data dan dapat meningkatkan kompleksitas manajemen data serta memperlambat proses secara keseluruhan.

3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?** <BR>
Dalam konteks pengembangan Rust yang mementingkan keselamatan penggunaan multithreading, penggunaan DashMap untuk static variable SUBSCRIBERS merupakan pilihan terbaik. DashMap memberikan thread safety dan akses yang efisien ke struktur data yang sama dari mana pun dalam aplikasi, menjaga keamanan konkurensi dalam lingkungan multi-threaded. Meskipun pola Singleton dapat diimplementasikan dalam Rust, penggunaan DashMap sudah mencakup fungsionalitas yang diperlukan tanpa memperkenalkan kompleksitas tambahan dari pola Singleton. DashMap adalah opsi yang tepat untuk kebutuhan static variable SUBSCRIBERS dalam BambangShop.

#### Reflection Publisher-2
1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?** <br>
Memisahkan Service dan Repository dari Model adalah tindakan yang mendukung prinsip-prinsip SOLID, terutama Prinsip Single Responsibility (SRP). Dalam pola MVC tradisional, Model bertanggung jawab tidak hanya atas penyimpanan data tetapi juga logika bisnis, yang bertentangan dengan SRP. Dengan memisahkan Service untuk mengatur logika bisnis dan Repository untuk mengelola akses data, kita menerapkan konsep Separation of Concerns dari SOLID untuk menciptakan kode yang lebih fleksibel dan mudah diatur.

2. **What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?** <br>
Penggunaan model saja dalam penanganan data logic dan business logic akan menyebabkan keterkaitan yang tinggi antar bagian, meningkatkan kompleksitas kode dan menyulitkan perawatan serta skalabilitas aplikasi. Tanpa memisahkan Service dan Repository dari model, bagian-bagian akan saling terkait, membuat kode sulit dipelihara dan kompleksitasnya meningkat secara keseluruhan.

3. **Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.** <br>
Penggunaan Postman dalam pengujian API sangat membantu untuk mengirimkan permintaan HTTP dan memeriksa respons yang diterima. Postman memudahkan pengiriman permintaan HTTP dengan data dalam body atau parameter ke URL atau endpoint tertentu. Ini membantu memastikan bahwa endpoint dapat menerima dan merespons data dengan benar, mempermudah pengujian fungsionalitas aplikasi secara keseluruhan.

#### Reflection Publisher-3
1. **Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?** <br>
Dalam kasus tutorial ini, kita menggunakan Push Model dari Observer Pattern untuk sistem notifikasi setiap kali terjadi proses add, delete, atau publish produk. Notifikasi ini dipicu secara langsung oleh tindakan-tindakan tersebut, dan NotificationService mengirimkan notifikasi kepada semua subscriber yang telah berlangganan pada jenis produk terkait.

2. **What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)** <br>
Jika kita mempertimbangkan penggunaan Pull Model sebagai alternatif untuk Subscriber dalam tutorial ini, keuntungan dari Pull Model adalah subscriber hanya mengambil data saat diperlukan, mengurangi penggunaan sumber daya jaringan dan komputasi. Namun, kekurangannya adalah subscriber harus secara aktif meminta pembaruan, yang bisa membuat keterlambatan dalam mendapatkan informasi terbaru. Selain itu, implementasi Pull Model memerlukan penambahan logika di sisi subscriber untuk mengelola request dan pembaruan data dengan efisien.

3. **Explain what will happen to the program if we decide to not use multi-threading in the notification process.** <br>
Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, program akan memproses notifikasi secara berurutan, satu per satu. Proses ini bisa menjadi lambat terutama jika terdapat banyak notifikasi yang harus dikirim, menyebabkan penundaan dalam memberikan respons. Dengan penggunaan multi-threading, notifikasi dapat diproses secara paralel, meningkatkan kecepatan dan responsivitas aplikasi serta efisiensi dalam mengirim banyak notifikasi secara bersamaan.
