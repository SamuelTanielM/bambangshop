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
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

Here are the questions for this reflection:
- >In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

    Single Model Struct cukup karena untuk logika observer dalam BambangShop tampaknya terpusat dalam SubscriberRepository, di mana para subscriber ditambahkan, di-daftar, dan dihapus. Sehingga, observer pattern biasanya diterapkan ketika perlu mendefinisikan sebuah interface atau trait spesifik, karena perilaku pengamatan terkait dengan instance individu.

- >id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
  
    Dalam kasus di mana id pada Program dan url pada Subscriber harus unik, menggunakan DashMap (map/dictionary) seperti yang saat ini digunakan tampaknya lebih tepat. Hal ini karena DashMap memiliki mekanisme built-in untuk memastikan keunikan kunci (id atau url), dan pencarian keberadaan elemen berdasarkan kunci dilakukan dengan efisien (waktu konstan O(1)). Ini sangat berguna saat jumlah data menjadi besar dan performa pencarian menjadi faktor penting.

- >When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
  
    Singleton pattern hanya menjamin bahwa satu instance dari sebuah kelas akan dibuat, namun tidak menjamin thread safety secara langsung. Dalam kasus penggunaan DashMap, kita mendapatkan keuntungan dari thread safety yang disediakan oleh DashMap secara langsung, yang memungkinkan untuk akses dan modifikasi yang aman dari banyak thread secara bersamaan tanpa perlu tambahan implementasi atau pengaturan.

    Jadi, meskipun kita menggunakan Singleton pattern untuk memastikan bahwa hanya ada satu instance dari SUBSCRIBERS, tetapi kita masih membutuhkan DashMap sebagai struktur data yang thread-safe untuk memastikan keamanan akses dari banyak thread secara bersamaan. Oleh karena itu, dalam konteks ini, penggunaan DashMap tetaplah diperlukan.
    
#### Reflection Publisher-2

- >In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

  Dalam pola Model-View-Controller (MVC), meskipun Model bertanggung jawab atas representasi data dan logika bisnis, memisahkan "Service" dan "Repository" dari Model penting karena memungkinkan keterpisahan tanggung jawab yang jelas antar komponen aplikasi. "Service" menangani logika bisnis kompleks atau pemrosesan data yang melibatkan beberapa entitas, sementara "Repository" bertanggung jawab untuk interaksi dengan penyimpanan data. Hal ini mengurangi ketergantungan antar modul, mempermudah pengujian, dan memungkinkan perubahan pada satu komponen tanpa memengaruhi yang lain. Hal ini memenuhi prinsip "**Separation of Concers**"

- >What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

  Akan mengalami peningkatan kompleksitas kode dalam Model itu sendiri dan juga peningkatan ketergantungan antar komponen. Sebagai contoh, dalam kasus BambangShop, jika kita hanya menggunakan Model, entitas seperti Program, Subscriber, dan Notification harus mengurus semua logika bisnis dan interaksi dengan penyimpanan data. Ini dapat menyebabkan penumpukan logika yang kompleks dalam Model dan sulit untuk memodifikasi atau memelihara kode di masa mendatang. Selain itu, jika terjadi perubahan pada logika bisnis atau penyimpanan data, semua perubahan tersebut harus dilakukan di dalam Model, menyebabkan kode menjadi sulit untuk dipelihara dan diperluas atau disebut juga **High Coupling**.    

- >Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

  Postman adalah alat yang sangat berguna untuk menguji dan **mengelola API**. Postman mungkin akan ngebantu saya menguji endpoint API yang dibuat dalam proyek. Beberapa fitur Postman yang saya temukan berguna dari eksplorasi dan juga referensi asdos:

    **HTTP Request**: Memungkinkan saya membuat berbagai jenis permintaan HTTP seperti GET, POST, PUT, DELETE, dan lainnya dengan mudah dan cepat.

    **Tests**: Fitur pengujian bawaan memungkinkan saya menulis skrip pengujian untuk memverifikasi respons dari permintaan API, sehingga saya dapat melakukan pengujian fungsional dengan mudah.

    **Import**: Fitur ini memungkinkan saya mengimport postman yang sudah dibuat.

#### Reflection Publisher-3

- >Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

  Dalam kasus tutorial ini, kita menggunakan variasi Push Model dari Observer Pattern. Dalam Push Model, publisher (NotificationService) mengirimkan data langsung kepada subscribers (Subscriber) ketika terjadi peristiwa yang diamati (seperti pembuatan, promosi, atau penghapusan produk). Dalam hal ini, ketika produk dibuat, dipromosikan, atau dihapus, NotificationService secara aktif mengirimkan notifikasi kepada semua subscribers yang berlangganan jenis produk tersebut. Oleh karena itu, kita menggunakan Push Model di mana data (notifikasi) didorong ke subscribers.

- >What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

  Jika digunakan pakai pull model, keuntungannya adalah meningkatkan privasi dan mengurangi beban pada publisher, karena informasi hanya diberikan kepada subscribers yang meminta. Namun, kerugiannya adalah potensi peningkatan latency dan kompleksitas implementasi yang mungkin terjadi karena subscribers perlu aktif meminta informasi dari publisher secara berkala.Oleh karena itu, pilihan antara Push Model dan Pull Model tergantung pada kebutuhan spesifik aplikasi dan preferensi desain.

- >Explain what will happen to the program if we decide to not use multi-threading in the notification process.

  Proses notifikasi akan dilakukan secara sinkronus, menyebabkan pengiriman notifikasi kepada setiap subscriber terjadi secara berurutan. Hal ini dapat mengakibatkan penurunan kinerja aplikasi karena proses notifikasi menjadi lambat dan dapat menghambat responsivitasnya, terutama jika jumlah subscriber yang besar atau waktu yang dibutuhkan untuk mengirimkan notifikasi kepada setiap subscriber cukup lama. 
