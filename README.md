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
1. Menurut saya, untuk kasus BambangShop saat ini cukup pakai satu struct Subscriber. Soalnya Subscriber yang ada sekarang hanya menyimpan data seperti url dan name, belum punya perilaku yang berbeda-beda. Interface atau trait biasanya dibutuhkan kalau ada beberapa jenis subscriber dengan cara kerja yang berbeda, tetapi semuanya harus mengikuti aturan yang sama. Jadi, karena di sini subscriber masih sederhana dan hanya sebagai model data, struct saja sudah cukup. Namun, kalau nanti ada banyak jenis subscriber dengan cara notifikasi berbeda, barulah trait akan lebih berguna.

2. Kalau url pada Subscriber memang harus unik, menurut saya DashMap lebih cocok daripada Vec. Vec sebenarnya masih bisa dipakai, tetapi setiap kali ingin mencari, mengecek duplikasi, atau menghapus subscriber, kita harus memeriksa satu per satu isi list. Itu kurang efisien. Dengan DashMap, url bisa dijadikan key, jadi pencarian, penambahan, dan penghapusan data jadi lebih cepat dan lebih sesuai dengan kebutuhan data yang unik. Jadi, Vec masih bisa jalan, tetapi DashMap lebih tepat untuk kasus ini.

3. Menurut saya, Singleton dan DashMap itu berbeda fungsi. Singleton hanya memastikan bahwa data SUBSCRIBERS punya satu instance global yang dipakai bersama. Namun, Singleton saja tidak cukup untuk membuat akses data menjadi aman kalau dipakai banyak thread sekaligus. Nah, DashMap dipakai karena dia thread-safe, jadi lebih aman untuk program Rust yang bisa berjalan secara concurrent. Jadi, jawabannya adalah Singleton saja tidak cukup. Kalau ingin aman untuk banyak thread, kita tetap butuh DashMap atau mekanisme thread-safe lain seperti Mutex atau RwLock.

#### Reflection Publisher-2
1. Menurut saya, Service dan Repository perlu dipisah dari Model supaya setiap bagian punya tanggung jawab yang jelas. Model sebaiknya fokus untuk merepresentasikan data, misalnya atribut dari Program, Subscriber, atau Notification. Repository bertugas mengatur penyimpanan dan pengambilan data, sedangkan Service menangani logika bisnis, misalnya proses subscribe dan unsubscribe. Kalau semua itu dimasukkan ke Model, maka Model akan menjadi terlalu besar dan terlalu banyak tanggung jawab. Akibatnya, kode jadi lebih sulit dibaca, diuji, dan dirawat. Dengan pemisahan ini, struktur program menjadi lebih rapi dan lebih mudah dikembangkan.

2. Kalau hanya menggunakan Model, menurut saya kode akan menjadi lebih rumit karena setiap model harus menangani banyak hal sekaligus. Misalnya, Program bukan hanya menyimpan data program, tetapi juga harus mengatur subscriber dan notifikasi. Subscriber juga bisa ikut menangani proses tertentu, dan Notification bisa ikut bergantung pada data dari model lain. Akibatnya, hubungan antar model menjadi terlalu erat. Kalau ada perubahan pada satu model, model lain juga bisa ikut terdampak. Hal ini membuat kode lebih sulit dipahami, lebih sulit diuji, dan lebih sulit dikembangkan. Jadi, kalau semua hanya ditaruh di Model, kompleksitas tiap model akan meningkat.

3. Ya, saya sudah mencoba mengeksplorasi Postman, dan menurut saya tool ini sangat membantu untuk menguji API yang sedang saya kerjakan. Dengan Postman, saya bisa langsung mengirim request ke endpoint seperti subscribe dan unsubscribe tanpa harus membuat frontend terlebih dahulu. Saya juga bisa mengecek apakah request body, parameter, response, dan status code sudah sesuai harapan. Hal ini sangat membantu saat debugging karena saya bisa lebih cepat menemukan kesalahan pada endpoint atau logika program. Fitur Postman yang menurut saya paling berguna adalah collection untuk menyimpan request, pengaturan body dan query parameter, serta tampilan response yang jelas. Untuk project kelompok atau project ke depan, Postman akan membantu dalam testing API secara lebih cepat, terstruktur, dan berulang.

#### Reflection Publisher-3
