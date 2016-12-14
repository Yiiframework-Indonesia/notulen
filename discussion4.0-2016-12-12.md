# Continuous Integration / Continuous Deployment (CI/CD)

## Informasi Umum
Insyaallah besok senin tgl 12 Desember 2016 seperti biasa kita akan mngadakan diskusi via whatsapp group ini dg topik "Continous Integration". Paparan awal akan disampaikan oleh Petra Barus, sedangkan notulis oleh Muhammad Alfan. Untuk mnsukseskan diskusi ini, kami berharap partisipasi dari rekan2 sekalian melaui pertanyaan, tanggapan, jawaban atau testimoni terkait topik ini. oogling ttg topik ini juga perlu bagi yg bener2 belum mngenal ttg topik ini. Demikian informasi ini harap mnjadi maklum.

## Paparan Singkat
### Pengantar
Continuous Integration / Continuous Deployment (CI/CD) adalah sebuah praktek di mana kode yang dikembangkan oleh anggota tim dikumpulkan ke dalam repositori bersama secara rutin beberapa kali dalam satu hari (CI) kemudian kode yang sudah diverifikasi dan disetujui ini langsung dideploy ke production secara rutin pula (CD). Tujuan utama dari praktek CI ini adalah agar bug-bug yang terjadi dapat dideteksi sedini mungkin dan dapat ditanggulangi langsung. Selain itu praktek CD bertujuan agar fitur-fitur yang dikembangkan dapat langsung dideliver ke pengguna/product owner. Hal ini memudahkan mendapatkan feedback dari pengguna seperti bug report, ketidaksesuaian requirement, dan juga perubahan model business atau requirement.

Praktek utama dalam melakuan CI/CD (Referensi diambil di Martin Fowler) dan ini akan menggunakan contoh-contoh yang lazim dilakukan sekarang. Nama teknologi yang digunakan tidak harus mengikuti dan bisa disesuaikan dengan kebutuhan.

**1. Gunakan sebuah repositori utama.**
Biasanya tim menggunakan sebuah repositori Git/SVN/Mercurial yang dapat diakses oleh semua anggota tim. Salah satu contoh workflow yang populer yakni **fork workflow** , akan ada terdapat sebuah git repositori utama yang akan difork oleh tiap anggota tim. Setelah anggota tim selesai melakukan sebuah pekerjaan, commit change akan diserahkan ke repo utama dalam bentuk pull request yang akan direview oleh anggota tim lain.

**2. Lakukan build yang diotomatisasi.**
Proses build di sini biasanya terdiri dari proses static analysis dan juga kompilasi (untuk compiled language seperti Java atau C++). Untuk bahasa PHP biasanya proses build juga yang dilakukan antara lain
  1. Static analysis
Pada proses ini kode akan dianalisa untuk melihat apakah sesuai standar dan juga ada potensial problem.
  2. Library packaging
Di sini biasanya akan melakukan download kode-kode library dari Composer yang versi production bukan development.
  3. Asset compilation
Proses ini akan melakukan download kode-kode CSS atau JS yang sudah minified.
  4. Template engine compilation
Proses ini akan melakukan kompilasi kode-kode template engine (Smarty dll).

Sebisa mungkin proses-proses diatas dapat dieksekusi dengan mudah, semudah menjalankan satu terminal command atau mengklik satu tombol.

**3. Buat agar build melakukan test pada diri sendiri**
Tidak hanya melakukan kompilasi, proses build di atas juga harus menyertakan proses testing yang otomatis, misalnya langsung menjalankan PHPUnit atau Codeception.

**4. Setiap commit harus dibuild di mesin integrasi.**
Selain dilakukan di lingkungan lokal masing-masing anggota tim, proses build dari commit changes juga harus dibuild di mesin integrasi. Mesin ini sering disebut sebagai CI Server. Teknologi yang digunakan biasanya adalah Jenkins/Hudson, atau ada banyak service-service berbayar seperti Travis, Bamboo, dll.

**5. Jaga agar build tetap cepat**
Agar bisa langsung dicoba oleh pengguna, proses build ini harus dibatasi waktunya. Idealnya tidak lebih dari 10 menit. Biasanya proses yang memakan waktu lama adalah proses testing. Untuk mengatasi ini biasanya proses testing dapat dipecah-pecah dan dilakukan secara paralel.

**6. Lakukan test di lingkungan yang sama dengan production.**
Perbedaan antara lingkungan yang ada di pengembangan dengan production dapat menjadi potensi untuk masalah. Sebisa mungkin lingkungan di dalam pengembangan, testing, dan production dibuat semirip mungkin. Misalnya menggunakan operating system yang sama hingga ke versinya, database yang sama, package yang diinstal juga sama.

Hal ini memang sangat sulit untuk dicapai, misalnya kalau kita ingin melakukan pengujian di jaringan lambat sementara lingkungan pengujian memliki jaringan cepat.Tetapi dalam banyak hal seperti operating system, libraries, dan installed software/packages, sekarang dengan adanya teknologi virtualisasi seperti Vagrant dan container seperti Docker, ini menjadi sedikit lebih mudah.

**7. Buat agar setiap orang dapat mengakses build terakhir dengan mudah.**
Sebisa mungkin build terakhir mudah diakses oleh semua orang baik anggota tim dan terutama pengguna atau pemilik produk. Misalnya semua orang diberikan URL ke website versi terakhir dan belum masih masuk ke production. Ini akan mempercepat proses testing dan verifikasi.

**8. Semua orang dapat melihat apa yang terjadi.**
Ada baiknya dapat diberikan dashboard berupa website yang dapat diakses oleh semua orang yang isinya berupa informasi-informasi terakhir dari proses integrasi. Misalnya apakah status proses build terakhir berhasil atau tidak, versi terakhir yang sudah dideploy di production, lama waktu untuk menjalanan proses build, siapa yang melakukan commit terakhir.

**9. Lakukan deployment secara otomatis.**
Dengan adanya proses testing dan deployment staging di lingkungan yang berbeda-beda, ada baiknya juga proses deployment ke production juga dilakukan semudah mungkin, semudah mengklik tombol di dashboard. Satu hal yang perlu diimplementasi juga adalah proses roll back, ketika di production ditemukan bug yang fatal, kode di production harus dapat dengan mudah dikembalikan ke versi sebelumnya.

### Secara ringkas, langkah-langkah yang perlu dilakukan adalah :
1. **Setiap anggota tim mengunduh kode ke workspace lokal.**
Dalam git, proses ini dilakukan dengan menggunakan git clone.

2. **Setelah selesai melakukan pekerjaan, kode perubahaan dicommit ke repositori utama.**
Untuk workflow **fork-workflow** , sebelum dicommit ke repositori utama, biasanya anggota tim melakukan pull request agar kode dapat direview terlebih dahulu.

3. **Server CI akan memonitor repositori dan mengunduh kode perubahaan saat ada commit terbaru.**
Server CI seperti Jenkins memiliki fitur agar dapat mengawasi perubahan kode pada sebuah repositori git secara berkala. Namun pada umumnya server-server repositori git (github atau gitlab) biasanya menyediakan hook yang dapat memberitahu secara aktif ke server CI bahwa ada perubahan yang terjadi.
Server ini nantinya akan mengunduh kode untuk dibuid.

4. **Server CI kemudian akan melakukan build dan testing.**

5. **Server CI akan melakukan release untuk artifak yang siap dideploy.**
Artifak ini biasanya berupa zip package yang siap untuk dideploy. Artifak-artifak ini biasanya dikumpulkan di file storage seperti FTP atau tempat-tempat lain.

6. **Server CI akan memberi label pada artifak tersebut.**
Biasanya label tersebut adalah commit version, atau build version, atau software version, atau pun mungkin build timestamp. Tergantung pada kebutuhan. Usahakan agar mudah dibaca dan dipahami.

7. **Server CI akan memberi notifikasi pada tim saat build selesai.**
Notifikasi ini biasanya berupa email atau message di Slack.

8. **Jika ada build atau test gagal Server CI akan memberikan peringatan.**

9. **Tim akan melakukan perbaikan terhadap build atau test yang gagal.**
Jadikan prioritas utama untuk segera melakukan perbaikan juga melihat ada build atau test yang gagal.

10. **Lakukan terus hal-hal di atas berulang kali.**

### Tanggung jawab tim
1. **Selalu mengcommit kode secara rutin (tiap jam, tiap dua jam).**
2. **Jangan mengcommit kode yang rusak.**
3. **Jangan mengcommit kode yang belum ditest.**
4. **Jangan mengcommit kode pekerjaan lain saat build untuk sebuah pekerjaan gagal.**
5. **Pantang pulang sebelum padam (commit terakhir berhasil dibuild).**

### Teknologi-teknologi yang biasanya digunakan
  1. Code Versioning
    a. Git
    b. Mercurial
    c. SVN
    d. Github
    e. Gitlab
    f. Bitbucket
2. CI Server
    a. Jenkins
    b. Travis CI
    c. Bamboo CI
3. Testing
    a. Codeception
    b. PHPUnit
    c. PHPUnit Coverage
    d. Selenium
4. Coding Standard
    a. PHPCS
    b. PHP Checkstyle
    c. PHPCS Fixer
5. Metrics
    a. Dissect
    b. PHPLoc
6. Analysis
    a. PHPMD
    b. PHPLint
    c. Phan
7. Environment
    a. Vagrant
    b. Docker
8. Platform
    a. AWS Elasticbeanstalk
    b. Azure
    c. Heroku
    d. Cloudbees
9. Scripting
    a. Ansible
    b. Chef
    c. Puppet
    d. Bash
    e. Ant

## FAQ
- Tanya: yg dimaksud server CI itu aplikasi ?
Jawab: Iya aplikasi untuk ngemonitor source code dan ngemanage build process

- Tanya: Teknologi poin 5, metrics itu buat apa ya ?
Jawab: Buat ngeliat kompleksitas kode. misal berapa banyak file, berapa banyak line of code, berapa banyak class, berapa banyak method, berapa banyak inheritance, berapa banyak field dalam class.

- Tanya: Kalau lihat fungsinya, menggunakan CI berarti wajib menerapkan juga versioning, menulis unit testing ?
Jawab: kalau kita lihat github yii2 (yiisoft/yii2).. ketika ada commit maka otomatis langsung ditest oleh scrutinizer klo ga salah.
Jawab2: Ideally iya, emang ini keliatan makan waktu di awal, tapi anggap ini investasi :), kalau di kantor saya, kita nerapin build server. jadi tiap bikin pull request bakal dianalysis dan dirun test apakah PRnya itu bakal ngebreak. setelah itu PR bakal direview. kalau udah dimerge ke master, tetep dirun analysis sama test, terus build deployable artifak, lalu dideploy ke semua server

- Tanya: Unt deploy production spy bisa rollback, berarti production jadi salah satu fork ya ?
Jawab: salah satu branch, iya

- Tanya: kalau server yg harus dideploy banyak, apakah Jenkins/hudson juga bisa ? untuk case cluster misalnya ?
Jawab: saya selama ini pakai rsync untuk deploy ke multi server, dan pakai tags, misal : tags devel untuk server devel, tags live untuk server live, cuman ya gitu, masih merasa gak sesuai jalan yang benar 😬, tapi masih rodo mending daripada deploy manual pakai ftp 😂(pengalaman dari yang menanyakan)

- Tanya: Dari style coding, keefektifannya, terlebih lagi bugs, Gmn itu? Bisa lebih dijelaskan ?
Jawab: Saya menggunakan ci untuk membantu saya dalam memahami best practice pemrograman, Saya menggunakan psr2, dengan menggunakan ci, coding saya akan dicek apakah sudah sesuai dengan psr2 atau belum, Keefektifan code misalnya ada dua/lebih function/file yang sama, Akan lebih baik kalo dijadikan 1, Scrutinizer ci bisa melakukan itu secara otomatis, Dengan menggunakan ci, juga bisa ditentukan apakah kebutuhan fungsional sistem sudah terpenuhi apa belum, Kita hanya perlu mendefinisikan case dalam script testing, Yii2 menggunakan codeception untuk melakukan itu, Testing bisa berupa unit/fungsional/acceptance

- Tanya: Apa itu scrutinizer?
Jawab: scrutinizer itu semacam code review om

- Tanya: Jadi dg scrutinizer itu ketika kita push ke github maka otomatis akan mnjalankan code review? Atau testing?
Jawab: Iya, betul sekali om. Dengan memakai scrutinizer coding akan direview secara otomatis

- Tanya: Review terkait PSR ya? Bukan code testing? Jika bukan code testing.. lalu biasanya pke tools apa supaya code yg kita push ke github otomatis di jalankan testingnya? Scrutinizer juga punya fitur testing juga?
Jawab: Bukan hanya psr om, fitur scrutinizer banyak, ada code quality, pencarian bug dll, Saya memakai scrutinizer sebatas itu, mungkin disini ada yg pernah menggunakannya lebih lanjut, Selain scrutinizer saya juga memakai travis, Tanpa keduanya, kita juga bisa melakukan testing aplikasi sendiri. Di yii2 sudah disediakan


(Yii Addict Indonesia)

[Kembali ke Index](README.md)