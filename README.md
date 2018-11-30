### Docker - Deploying Your First Docker Container
---

Pengertian Docker
---
Docker menggambarkan diri mereka sebagai "platform terbuka untuk pengembang dan sysadmin untuk membangun, mengirim, dan menjalankan aplikasi terdistribusi". 

---

1. Menjalankan Container
---
mengidentifikasi nama Gambar Docker yang dikonfigurasi untuk menjalankan Redis. Dengan Docker, semua kontainer dimulai berdasarkan Gambar Docker. Gambar-gambar ini mengandung semua yang diperlukan untuk meluncurkan proses; host tidak memerlukan konfigurasi atau dependensi apa pun.

---

untuk mencari gambar untuk Redis, dapat memasukkan perintah 
    
    docker search redis

Gambar 1

dapat diidentifikasi bahwa Redis Docker Image disebut redis dan ingin menjalankan rilis terbaru. Dalam kasus ini Redis merupakan database. kemudian jika ingin menjalankannya sebagai layanan latar belakang sementara dia terus bekerja, maka luncurkan container di latar belakang yang menjalankan turunan Redis berdasarkan gambar resmi.

---

Docker CLI memiliki perintah yang disebut run yang akan memulai kontainer berdasarkan Image Docker. 
    
    docker run -d redis

Gambar 2

Secara default, Docker akan menjalankan versi terbaru yang tersedia. Jika versi tertentu diperlukan, itu bisa ditetapkan sebagai tag, misalnya, versi 3.2 akan menjadi buruh pelabuhan menjalankan-redis: 3.2.

---
2. Menemukan Container Berjalan
---

    docker ps 
perintah diatas mencantumkan semua kontainer yang sedang berjalan, gambar yang digunakan untuk memulai kontainer. Perintah ini juga menampilkan nama dan ID yang friendly yang dapat digunakan untuk mencari informasi tentang masing-masing kontainer.

Gambar 3

    docker inspect <friendly-name|container-id>
perintah memberikan detail lebih lanjut tentang penampung yang berjalan, seperti alamat IP

    docker logs <friendly-name|container-id>
peintah yang akan menampilkan pesan yang ditulis oleh container

---
3. Akses Redis
---

Perintah untuk menjalankan Redis di background, dengan nama redisHostPort pada port 6379 
  
    docker run -d --name redisHostPort -p 6379:6379 redis:latest

Gambar 4

---

**Protip**

Secara default, port pada host dipetakan ke 0.0.0.0, yang berarti semua alamat IP. Anda dapat menentukan alamat IP tertentu ketika 
Anda menentukan pemetaan port, misalnya, -p 127.0.0.1:6379:6379

---
4. Akses Redis
---

kemudian dapat ditemukan bahwa hanya menggunakan opsi -p 6379 memungkinkannya untuk mengekspos Redis tetapi pada port yang tersedia secara acak. untuk menguji port tersebut digunakan perintah
    
    docker run -d --name redisDynamic -p 6379 redis:latest

Gambar 5 

kemudian untuk mengetahui port mana yang akan ditugaskan maka perintahnya adalah 
    
    docker port redisDynamic 6379

gambar 6

kemudian untuk menampilkan informasi pemetaan port menggunakan perintah 
    
    docker ps

gambar 7

---
5. Persisting Data
---

Data apa pun yang perlu disimpan di Host Docker, dan bukan di dalam kontainer, harus disimpan di / opt / docker / data / redis. Perintah lengkap untuk menyelesaikan tugasnya adalah
    
    docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis

Gambar 8

---
6. Menjalankan Kontainer Di Foreground
---

Sebagai contoh, Images Ubuntu dapat menjalankan perintah OS atau menjalankan perintah bash interaktif menggunakan / bin / bash

**contoh**

    docker run ubuntu ps
perintah untuk meluncurkan sebuah container Ubuntu dan mengeksekusi perintah ps untuk melihat semua proses yang berjalan.

gambar 9

menggunakan perintah dibawwah untuk mendapatkan akses ke shell bash di dalam container.
    
    docker run -it ubuntu bash

---
### Docker - Deploy Static HTML Website as Container
---

1. Membuat Dockerfile
---

Images Docker mulai dari Images dasar. Images dasar harus menyertakan dependensi platform yang dibutuhkan oleh aplikasi kita, misalnya, memasang JVM atau CLR.

Images dasar ini didefinisikan sebagai instruksi dalam Dockerfile. Docker Images dibangun berdasarkan isi dari Dockerfile. Dockerfile adalah daftar instruksi yang menjelaskan cara menyebarkan aplikasi kita.

Dalam contoh ini, Images dasar kita adalah versi Alpine dari Nginx. Ini menyediakan server web yang dikonfigurasi pada distribusi Linux Alpine.

---

    FROM nginx:alpine
    COPY . /usr/share/nginx/html
diatas merupakan perintah membuat Dockerfile untuk membangun Images dengan menyalin isi di diatas ke dalam editor.

Gambar 10

keterangan isi tersebut : Baris pertama mendefinisikan Images dasar kami. Baris kedua menyalin isi direktori saat ini ke lokasi tertentu di dalam Container.

---
2. Membuat Images Docker
---

Dockerfile digunakan oleh perintah build Docker CLI. Perintah build menjalankan setiap instruksi dalam Dockerfile. Hasilnya adalah Docker Image yang dibangun yang dapat diluncurkan dan menjalankan aplikasi Anda yang telah dikonfigurasi.

Perintah build mengambil beberapa parameter yang berbeda. Formatnya adalah docker build -t <build-directory>. Parameter -t memungkinkan kita untuk menentukan nama untuk images dan tag, yang biasa digunakan sebagai nomor versi. Ini memungkinkan Anda untuk melacak Images yang dibangun dan yakin tentang versi yang sedang dimulai.

    docker build -t webserver-image:v1 .

Gambar 11
perintah yang digunakan untuk membuat Images HTML statis  menggunakan perintah build. Kemudian kita dapat melihat daftar semua Image pada host yang digunakan, menggunakan perintah 
    
    docker images

gambar12

Images yang dibangun akan memiliki nama webserver-gambar dengan tag v1.

---
3. Run
---

Images yang dibangun dapat diluncurkan secara konsisten ke Images Docker lainnya. Saat container diluncurkan, ini dikotori dari proses dan jaringan lain di host. Ketika memulai sebuah container, kita perlu memberikan izin dan akses ke apa yang diperlukan.

Misalnya, untuk membuka dan mengikat ke port jaringan pada host, kita perlu memberikan parameter -p <host-port>: <port-kontainer>.

perintah meluncurkan images yang baru dibangun dengan menyediakan nama dan tag. Karena ini adalah server web, bind port 80 ke host kami menggunakan parameter -p.
    
    docker run -d -p 80:80 webserver-image:v1

gambar13

Begitu mulai, kita akan dapat mengakses hasil port 80 melalui perintah 
    
    curl docker

gambar14

Untuk membuat permintaan di browser, gunakan URL   https://2886795343-80-cykoria02.environments.katacoda.com/ . Anda sekarang memiliki situs web HTML statis yang dilayani oleh Nginx.

---
### Docker - Building Container Images
---

Docker Images
---

Docker Images dibuat berdasarkan Dockerfile. A Dockerfile mendefinisikan semua langkah yang diperlukan untuk membuat Docker Images dengan aplikasi Anda dikonfigurasi dan siap dijalankan sebagai container. Images itu sendiri mengandung segalanya, mulai dari sistem operasi hingga dependensi dan konfigurasi yang diperlukan untuk menjalankan aplikasi Anda.

---
1. Base Images
---

Images dasar ini digunakan sebagai dasar untuk perubahan tambahan Anda untuk menjalankan aplikasi Anda. Misalnya, dalam skenario ini, kami mengharuskan NGINX untuk dikonfigurasi dan berjalan pada sistem sebelum kami dapat menerapkan file HTML statis kami. Karena itu kami ingin menggunakan NGINX sebagai gambar dasar kami.

Dockerfile adalah file teks sederhana dengan perintah pada setiap baris. Untuk menentukan images dasar, kami menggunakan instruksi FROM <nama-gambar>: <tag>

gambar15

---

**Membuat Dockerfile**
---
Baris pertama Dockerfile harus FROM nginx: 1.11-alpine

membuat perubahan pada editor Dockerfile. Di dalam lingkungan, Dockerfile baru akan dibuat dengan konten editor.

---
2. Running Commands
---

RUN <command> memungkinkan Anda untuk menjalankan perintah apa pun seperti yang Anda lakukan pada prompt perintah, misalnya menginstal paket aplikasi yang berbeda atau menjalankan perintah build. Hasil RUN dipertahankan ke Images sehingga penting untuk tidak meninggalkan file yang tidak perlu atau sementara pada disk karena ini akan dimasukkan dalam Images.

COPY <src> <dest> memungkinkan Anda untuk menyalin file dari direktori yang berisi Dockerfile ke images kontainer. Ini sangat berguna untuk kode sumber dan aset yang ingin Anda terapkan di dalam kontainer Anda.

gambar16

---
3. Exposing Ports
---

Dengan file ,kami disalin ke dalam images kami dan setiap dependensi yang diunduh, Anda perlu menentukan aplikasi port mana yang harus dapat diakses.

Menggunakan perintah EXPOSE <port> Anda memberi tahu Docker port mana yang harus dibuka dan dapat diikat juga. Anda dapat menentukan beberapa port pada satu perintah, misalnya, EXPOSE 80 433 atau EXPOSE 7000-8000

Tugas
---

Kami ingin server web kami dapat diakses melalui port 80, tambahkan baris EXPOSE yang relevan ke Dockerfile.

Gambar17

---
4. Building Containers
---

Menggunakan perintah **docker build** untuk membuat Images. Anda dapat memberi nama images yang friendly dengan menggunakan opsi -t <name>.
kamu dapat menggunakan perintah dibawah untuk melihat list images yang berada di lokal
    
    docker images

gambar18

---
5. meluncurkan Images Baru
---

Jalankan contoh images yang baru Anda bangun menggunakan hasil ID dari perintah bangun atau nama panggilan yang Anda tetapkan.
NGINX dirancang untuk dijalankan sebagai layanan latar belakang sehingga Anda harus menyertakan opsi -d. Untuk membuat server web dapat diakses, ikat ke port 80 menggunakan p 80:80

Contoh :
    
    docker run -d -p 80:80 <image-id|friendly-tag-name>

Anda dapat mengakses server web yang diluncurkan melalui hostname hostname. Setelah meluncurkan kontainer, dengan perintah 
    
    curl -i http://docker

untuk mengecek container jalankan perintah 
    
    docker ps

---
