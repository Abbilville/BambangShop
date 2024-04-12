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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
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
1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?** <br>
Pada Observer design patterns, interface (atau trait pada rust) biasanya digunakan untuk mendefinisikan method-method atau services yang akan digunakan. Namun, pada kasus ini (BambangShop), penggunaan Interface atau Trait tidak terlalu diperlukan karena tidak ada banyak variasi dalam behaviour dari observer yang diharapkan. Sebagai gantinya, satu struktur Model yang mencakup data tentang observer dan behaviour yang diperlukan untuk memberi tahu mereka sudah cukup. Oleh karena itu, dalam kasus ini, menggunakan satu struktur Model untuk observer sudah cukup.<br>
2. ****id** in **Program** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?** <br>
Penggunaan Vec (list) mungkin sudah sufficient jika kita hanya perlu menyimpan ID atau URL unik dari pelanggan. Namun, menggunakan DashMap (map/dictionary) seperti yang digunakan saat ini lebih disarankan karena memberikan keuntungan dalam kecepatan akses dan manipulasi data. Dengan DashMap, kita dapat dengan mudah memeriksa keberadaan ID atau URL yang sudah ada, dan juga dapat dengan cepat mengakses atau memperbarui entri yang ada. Ini dapat menjadi lebih efisien dan memungkinkan kinerja yang lebih baik ketika skala aplikasi meningkat.<br>
3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (**SUBSCRIBERS**) static variable, we used the **DashMap** external library for **thread safe HashMap**. Explain based on your understanding of design patterns, do we still need **DashMap** or we can implement Singleton pattern instead?** <br>
Pada Singleton Pattern, satu class hanya memiliki satu instance di seluruh aplikasi. Dalam kasus ini, menggunakan DashMap untuk SUBSCRIBERS static variable adalah pilihan terbaik karena memungkinkan kita untuk memiliki akses ke struktur data yang sama dari mana pun dalam aplikasi, dan memastikan bahwa itu aman secara bersamaan. DashMap menyediakan solusi yang baik untuk keamanan concurrency dalam multithreaded environment, yang mana sangat penting dalam pengembangan program Rust. Meskipun Singleton pattern dapat diimplementasikan dalam Rust, penggunaannya mungkin tidak diperlukan dalam kasus ini karena DashMap secara efektif menyediakan fungsionalitas yang diperlukan sambil mempertahankan keamanan bersama. Oleh karena itu, penggunaan DashMap adalah pilihan yang tepat untuk kebutuhan SUBSCRIBERS static variable dalam aplikasi BambangShop.


#### Reflection Publisher-2
1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?** <br>
Seperti yang kita tahu dari prinsip SOLID yaitu SRP atau **Single Responsibility Principle** yang mana sebuah class hanya boleh berfungsi sesuai dengan kelasnya saja sehingga kita perlu memisahkan **Service** dan **Repository** dari Model karena Model hanyalah class untuk kita mendefinisikan modelnya, bukan untuk operasinya. Hal ini sesuai juga dengan prinsip ***Design Principle*** yang mana kita harus memisahkan part yang berbeda dari semestinya. Dalam MVC asli, Model mengetahui segala sesuatu yang terkait dengan data, mulai dari business logics, validasi, data itu sendiri, bahkan hingga operasi penyimpanan data. Dengan memisahkan business logics ke Service dan operasi penyimpanan data ke Repository, Model sekarang hanya bertanggung jawab sebagai "representatif" atau struktur dari sebuah set data. Ini kemudian dapat dengan mudah diubah menjadi tabel SQL dan sebaliknya karena tidak perlu menginisialisasi metode atau bidang yang tidak terkait lainnya.<br>
2. **What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?** <br>
Jika kita hanya menggunakan Model tanpa memisahkan concern menjadi Service dan Repositori. Model (Product, Subscriber, Notification) akan bertanggung jawab atas representasi data dan business logics, menghasilkan class dengan banyak tanggung jawab yang melanggar SRP. Hal ini dapat menyebabkan peningkatan kompleksitas, keterkaitan erat antara komponen, dan kesulitan dalam memelihara dan memperluas codebase. Selain itu, interaksi antara model-model yang berbeda akan menjadi lebih rumit dan terkait erat. Misalnya, jika sebuah Product perlu memberi tahu Subscriber tentang perubahan, itu mungkin langsung berinteraksi dengan objek Subscriber untuk mengirimkan Notification, melanggar enkapsulasi dan menghasilkan kode yang sulit dipahami. Secara keseluruhan, tanpa pemisahan concern yang tepat, kompleksitas kode untuk setiap model akan meningkat secara signifikan, membuatnya lebih sulit dipahami, diuji, dan dipelihara. <br>
3. **Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.** <br>
Menggunakan Postman membantu saya untuk mengetest program yang dibuat pada tutorial ini dengan mengirimkan response ke endpoint API sehingga kita dapat mengsimulasikannya walaupun belum ada HTML-nya. Berikut fitur yang mungkin akan saya gunakan di tugas kelompok nantinya:
- **Sending Requests**, fitur ini memungkinkan kita dengan mudah mengirim permintaan HTTP ke endpoint API dengan parameter yang dapat disesuaikan seperti header, parameter query, dan request body.
- **Automated Testing**, fitur ini memungkinkan kita membuat dan menjalankan rangkaian pengujian otomatis di Postman untuk memverifikasi perilaku endpoint API, termasuk pengujian untuk response status code, response body content, dan lainnya
- **Environment Variables**, fitur ini memungkinkan untuk mendefinisikan dan menggunakan environment variable, yang membuat mudah untuk beralih antar environment yang berbeda (misalnya, development, staging, production) saat menguji API.

#### Reflection Publisher-3
1. **Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and **Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?** <br>
Dalam kasus tutorial ini, kita menggunakan variasi Push model dari Observer Pattern. Hal ini terlihat dari deskripsi bahwa notificiation akan dikirimkan kepada subscriber yang subscribe melalui HTTP POST request yang dipicu oleh pembuatannya, promosi, atau penghapusan produk. <br>
2. **What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)** <br>
Jika kita menggunakan variasi Pull model dari Observer Pattern untuk kasus tutorial ini, maka ada plus minus nya. <br>
Advantages:
- Dalam model Pull, Subscriber hanya akan mengambil data saat mereka memerlukannya, mengurangi penggunaan sumber daya jaringan dan komputasi.
- Subscriber memiliki kontrol penuh atas kapan mereka ingin mengambil data, sehingga dapat menghindari pengambilan data yang tidak perlu.<br>

Disadvantages:
- Dalam model Pull, pelanggan harus secara aktif meminta pembaruan, yang dapat menyebabkan keterlambatan dalam mendapatkan informasi baru. Ini dapat menjadi tidak efisien dalam kasus di mana pembaruan harus segera diterima.
- Implementasi Pull model mungkin memerlukan lebih banyak logika pada sisi pelanggan untuk mengelola permintaan dan pembaruan data secara efisien.<br>

3. **Explain what will happen to the program if we decide to not use multi-threading in the notification process.** <br>
Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notification, maka notification akan diproses secara berurutan dan secara serial. Ini berarti setiap notification akan diproses satu per satu, menunggu hingga notification sebelumnya selesai sebelum memproses notification berikutnya. Akibatnya, jika ada banyak notification yang harus dikirim, proses ini bisa menjadi lambat dan menyebabkan penundaan dalam memberikan respons kepada pengguna. Dengan menggunakan multi-threading, kita dapat memproses notification secara paralel, mempercepat prosesnya dan meningkatkan responsivitas aplikasi. Tanpa multi-threading, aplikasi mungkin akan terasa lambat dan kurang responsif terutama saat mengirim banyak notification.
