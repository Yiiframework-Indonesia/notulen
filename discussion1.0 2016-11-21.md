# Codeception

## Informasi Umum
Sebagaimana kesepakatan sebelumnya bahwa setiap hari senin kita akan diskusi tentang satu topik yang telah ditentukan sebelumnya, dan pada hari senin ini (21/11/2016) topik yang dibahas adalah tentang “Codeception”.

Diharapkan bagi semua member untuk ikut terlibat aktif dalam diskusi ini, kita sama-sama belajar dari best practice yang mungkin pernah dilakukan atau pernah dibaca dan atau dipelajari sebelumnya. Oleh karena diskusi ini tanpa moderator sehingga diharapkan kesadaran bagi para member untuk dapat menciptakan suasana kondusif dan ilmiah sehingga diskusi dapat berjalan dengan baik sesuai dengan harapan kita.

Diskusi pada hari ini akan didahului dengan paparan singkat tentang topik ini oleh Hafid Mukhlasin dan kemudian runtutan jalannya diskusi akan dirangkum oleh notulis Agung Andika.


## Paparan Singkat
### Latar Belakang
Sebelum kita membahas tentang *codeception* maka alangkah baiknya kita mulai dari latar belakang mengapa kita perlu dengan tools ini. Pada pengembangan aplikasi, seorang developer dituntut untuk bisa membuat aplikasi yang bekerja dengan baik sesuai dengan yang diinginkan, dan untuk memastikan hal itu maka dilakukanlah testing aplikasi sebelum launching. Sehingga suka tidak suka, semua developer pasti melakukan testing aplikasinya bahkan mungkin berulang kali dalam melakukannya.

Umumnya developer akan memulai menulis kode aplikasi, kemudian mengujinya melalui browser, memperbaikinya jika ada error lalu mengujinya lagi, demikian seterusnya hingga aplikasi selesai. Dalam pengujian kadang kala developer perlu menginputkan pada form data tertentu, kemudian mengujinya, jika perlu mengecek apakah data tersebut masuk ke database, dst.

Testing yang dilakukan secara berulang-ulang dan manual ini tentunya tidak efisien dalam hal waktu dan tentunya membosankan. Berangkat dari hal inilah kemudian dibuatlah tools untuk membantu developer melakukan testing secara otomatis (automated testing). 

Tapi ingat bahwa yang otomatis adalah proses testingnya bukan penentuan aturan dari testing. Jadi bukan setelah koding aplikasi selesai lalu developer tiba-tiba menjalankan tools automated testing, namun sebelumnya developer perlu mendifinisikan dulu tentang apa yang mau di-tes atau aturan dari testing.

### Test Driven Development
Supaya lebih efisien, maka automated testing umumnya menggunakan pendekatan konsep Test Driven Development (TDD) atau Behavior Driven Developement (BDD). Kedua konsep ini pada dasarnya sama yaitu pengembangan software yang dikendalikan atau di drive oleh testing / perilaku yang diinginkan terhadap software tersebut. Perbedaanya hanya pada cara penulisan testing dimana TDD menggunakan bahasa program sedangkan BDD menggunakan bahasa manusia.

Sederhananya, konsep ini menganjurkan agar para developer untuk menentukan semua aturan atau perilaku atau kemampuan yang diinginkan dari aplikasi atau modul aplikasi yang hendak dibuat serta mencatatnya. Catatan tersebut kemudian dijadikan acuan untuk membuat atau koding bagian-bagian dari aplikasi. Setelah suatu tahapan koding selesai baru kemudian dilakukan pengujian/testing aplikasi dengan cara mencocokkan catatan perilaku yang diinginkan dengan perilaku aplikasi yang telah dibuat tersebut.

Sehingga jangan salah bahwa TDD/BDD tidak pernah menganjurkan developer untuk melakukan testing terlebih dahulu sebelum koding, apanya yang ditesting lha wong aplikasi aja belum dibuat. Namun menganjurkan developer untuk menulis kode untuk testing sebelum koding. 

### Apakah ini logis? Mengapa hal ini dikatakan efisien?
Jadi gini, umumnya seorang developer sebelum koding akan mencatat (minimal dalam otaknya) tentang apa yang akan dilakukan atau fungsi-fungsi apa yang diinginkan baru kemudian koding sesuai atau mengikuti dengan catatan itu. Baru setelah itu, developer akan menguji (testing) aplikasinya apakah sesuai dengan catatan tersebut.

Nah, konsep TDD dengan automated testingnya hanya menganjurkan developer agar mencatat fitur-fitur atau perilaku yang diinginkan dari aplikasi dalam bentuk kode-kode testing. Sehingga setelah developer selesai koding berdasarkan kode-kode testing maka di developer bisa menjalankan testing secara otomatis. Salah satu alasan menulis kode untuk testing sebelum koding aplikasi adalah akan membuat developer lebih fokus pada apa yang ingin dicapai. 

Inilah mengapa dinamakan Test Driven Development.

### Mengapa Codeception?
Pada prinsipnya, codeception merupakan salah satu tools untuk automated testing aplikasi berbasis PHP. Hal ini artinya ada tools lain yang bisa kita gunakan selain codeception untuk melakukan testing aplikasi kita, misalnya PHPUnit, Behat, SimpleTest, dsb.

Lantas mengapa codeception dan bukan yang lain?

Codeception ini sebenarnya lebih dari sekedar tools, namun dia juga merupakan framework sekaligus best practice untuk testing aplikasi berbasis PHP. Fiturnya cukup lengkap mulai dari unit testing menggunakan PHPUnit, functional testing, acceptance testing (browser testing) menggunakan selenium webdriver, API tetsing, integration testing, BDD, dan yang menarik adalah mendukung banyak framework PHP termasuk Yii sehingga lebih mudah diimplementasikan. 

Ada 3 jenis test yang bisa dilakukan dengan menggunakan codeception:
- Unit Testing - untuk memverifikasi bahwa suatu kode aplikasi berjalan sesuai dengan yang diharapkan. CodeCeption menggunakan PHPUnit sebagai backend untuk menjalankan tests.
- Functional Testing - untuk memverifikasi skenario dari perspektif bisnis proses menggunakan browser emulation.
- Acceptance Testing - untuk memverifikasi skenario dari perspektif pengguna menggunakan web browser.

> **Pro Tip**
> Hal yang sering terlupakan dari testing code adalah code coverage analysis. Hal ini berguna untuk mengetahui seberapa jauh jangkauan coverage testing code yang telah kita lakukan. Apakah semua line / condition logic sudah ter-cover test atau tidak.
Codeception di Yii

Yii sudah membuatkan kita template kode testing menggunakan codeception yaitu terletak di folder @app/tests/. Adapun panduannya silahkan baca di @app/tests/README.md. Di folder tests tersebut telah dicontohkan bagaimana membuat unit test, functional test dan acceptance test, mudah sekali.

> **Pro Tip**
> Disarankan menggunakan codeception versi terbaru dengan support gherkin. Testing lebih enak dan tidak harus dikerjakan oleh QA Engineer. Dan bisa langsung ditulis oleh Business Analyst. Info lebih lanjut, sila kunjungi tautan berikut: 
https://github.com/cucumber/cucumber/wiki/Gherkin

### Test Coverage
Prinsipnya, test coverage digunakan untuk mengukur jumlah code / testing dari sebuah satuan test. Di semua jenis test juga bisa diatur coverage. Namun lebih tepat jika pada unit test digunakan untuk fokus test komponen-komponen sehingga analisis coverage dilakukan pada level unit-test. [butuh editing kalimat]

### UAT vs Automated Code Testing
Pada dasarnya semua developer membuat code testing, namun ada yang membuat code testing untuk dijalankan otomatis ada yang tidak. Dikarenakan tenggat waktu pengerjaan project yang terbilang singkat, terkadang kita tidak (sempat) membuat code testing. 

Jika menilik ke dalam UAT (User Acceptance Test), prosedur testing yang ada di dalam UAT adalah panduan (scenario) yang dilakukan tester dengan membandingkan expected condition dan hasil yang didapat (actual result).

> **Pro Tip**
> Test-driven Development, adalah sebuah proses pengembangan software yang bergantung pada siklus tahapan-tahapan kecil yang berulang. Kebutuhan dalam setiap sikul difokuskan ke spesifik test-case dan software/code di-improve untuk lulus dalam pengujian software (testing). TDD menghapus mindset "Yang penting jalan dulu" dan mengutamakan kualitas dan kecepatan deliver.

Perbedaan mendasarnya adalah proses testing yang dilakukan dengan UAT dilakukan manual oleh user, sedangkan dengan memanfaatkan codeception, dilakukan secara otomatis.

### Prons & Cons
**Cons:**
- Memakan waktu, mengingat waktu untuk pengerjaan proyek cukup singkat.
- Ribet, perlu kerja ekstra (2x kerja coding)
- Terlalu overkill untuk project skala kecil.

**Prons:**
- Memudahkan pemantauan jika suatu saat hendak menambahkan fitur, apakah ada BC Break atau tidak.
- Prove / bukti kalau code yang kita tulis sudah melewati test sehingga meningkatkan kepercayaan terhadap code yang kita tulis.

## FAQ

- Tanya: Jadi, apanya yang otomatis jika tetap men-define test terlebih dahulu?
Jawab:
Kode-kode untuk testing yang ditulis akan mengerjakan pengujian sesuai skenario yang sudah di-define. Misal, mengisi form, klik button, dsb dengan memanfaatkan selenium (contohnya) sebagai bot untuk melakukan tugas-tugas tersebut. Selain selenium, juga ada phpbrowser, phantomjs. 
Kelebihan dan kekurangan selenium / phpbrowser / phantomjs??

- Tanya: Apakah kita bisa menggunakan acceptence test utk grab isi web? 
Jawab:
Dengan menggunakan selenium sebagai browser simulation, bisa digunakan untuk scapping web.

- Tanya: Termasuk input captcha, apakah juga bisa di-passing ke user untuk diselesaikan manual oleh user?
Jawab:
Dengan selenium, secara teknis bisa. [note: CAPTCHA, merupakan akronim dari Completely Automated Public Turing test to tell Computers and Humans Apart, jika “mesin” bisa menyelesaikan captcha, maka tugas dari captcha masih belum sempurna. Untuk keperluan testing, solusi menjawab CAPTCHA dengan menggunakan 3rd party services seperti DeathByCaptcha] [butuh referensi lagi]

- Tanya: Kenapa harus menerapkan codeception dalam mengerjakan sebuah project jika ketika develop pasti sudah di-test terlebih dahulu sebelum dikirim ke client?
Jawab:
Pertama yang perlu difahami bahwa TDD (Test-driven Development) merupakan konsep dalam pengembangan software dengan tujuan untuk efisiensi dalam proses software development. Namun, konsep ini belum begitu massive dipakai di dalam negeri, baik perusahaan besar maupun kecil. Baru beberapa perusahaan yang memakai.

- Tanya: Bicara soal level skill? Siapa yang mestinya menulis code testing? Developer pemula atau developer senior? Dengan asumsi yang menulis code adalah developer level menengah.
Jawab:
Idealnya, untuk unit-test ditulis oleh author class tersebut. Sedangkan untuk functional dan acceptance, bisa ditulis oleh yang lain, misalnya QA / Testing Engineer. 

(Yii Addict Indonesia)
