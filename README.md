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

You can download the Postman Collection JSON from the module handout.

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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
### Reflection Publisher-1

1.  Dalam kasus BambangShop, kalau hanya ada satu jenis subscriber, sebenarnya satu Model struct saja sudah cukup untuk digunakan. Namun, tujuan dari pola Observer adalah agar sistem bisa mendukung berbagai jenis observer dengan perilaku berbeda. Dengan menggunakan trait, desain jadi lebih fleksibel dan mudah dikembangkan ke depannya. Jadi meskipun saat ini terlihat sederhana, penggunaan trait tetap lebih baik. Hal ini juga sesuai dengan prinsip *program to interface, not implementation*.

2.  Vec memang bisa dipakai untuk menyimpan data subscriber, tetapi kurang efisien jika kita sering melakukan pencarian atau penghapusan berdasarkan nilai unik seperti id atau url. Dengan DashMap, kita bisa mengakses data langsung berdasarkan key tanpa perlu iterasi satu per satu. Selain itu, performanya juga lebih baik saat jumlah data bertambah.

3.  Singleton hanya memastikan bahwa data subscriber hanya memiliki satu instance dalam aplikasi. Namun, itu tidak menjamin keamanan saat diakses oleh banyak thread. Karena aplikasi bisa menangani banyak request secara bersamaan, kita tetap membutuhkan struktur data yang thread-safe. DashMap sudah menyediakan hal tersebut, sehingga tetap diperlukan meskipun konsep Singleton digunakan.

#### Reflection Publisher-2
1. Service dan Repository dipisah supaya tanggung jawabnya tidak bercampur. Repository fokus ke akses dan penyimpanan data, sedangkan Service menangani alur logic seperti subscribe dan unsubscribe. Dengan pemisahan ini, kode jadi lebih terstruktur, lebih mudah dibaca, dan lebih aman saat ada perubahan karena tidak semua bagian saling terikat langsung.

2. Kalau semua dimasukkan ke Model, maka satu class harus menangani terlalu banyak hal sekaligus: data, penyimpanan, dan logic. Ini membuat Program, Subscriber, dan Notification saling bergantung langsung dan meningkatkan kompleksitas. Akibatnya, kode jadi lebih sulit dipahami, sulit di-maintain, dan rawan error ketika ada perubahan kecil.

3. Postman membantu testing API tanpa perlu membuat frontend. Kita bisa langsung kirim request seperti subscribe dan unsubscribe, lalu cek response dari server. Fitur seperti collection memudahkan penyimpanan endpoint, environment variable membantu mengganti URL dengan cepat, dan history membantu melacak request sebelumnya. Ini membuat proses testing jadi lebih cepat dan efisien selama development.

#### Reflection Publisher-3
