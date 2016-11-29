# RESTful Web Services

## Informasi Umum
Sebagaimana kesepakatan sebelumnya bahwa setiap hari senin kita akan diskusi tentang satu topik yang telah ditentukan sebelumnya, dan pada hari senin ini (28/11/2016) topik yang dibahas adalah tentang "RESTful Web Services".

Diharapkan bagi semua member untuk ikut terlibat aktif dalam diskusi ini, kita sama-sama belajar dari best practice yang mungkin pernah dilakukan atau pernah dibaca dan atau dipelajari sebelumnya. Oleh karena diskusi ini tanpa moderator sehingga diharapkan kesadaran bagi para member untuk dapat menciptakan suasana kondusif dan ilmiah sehingga diskusi dapat berjalan dengan baik sesuai dengan harapan kita.

Diskusi pada hari ini akan didahului dengan paparan singkat tentang topik ini oleh Yuda Sukmana dan kemudian runtutan jalannya diskusi akan dirangkum oleh notulis Aan Budi.

# Catatan
Paparan awal yg akan saya post tidak bermaksud menggurui. Itu hanya pembukaan saja. Selebihnya diskusi supaya mengalir. Diharapkan tidak hanya tanya jawab saja tapi juga ada testimoni siapa yg sudah pernah mnggunakan atau pengalaman belajar RESTful Web Services di Yii2.


## Paparan Singkat
### Latar Belakang Web Services (Programmable Web)
Website yang kita buat tentu kita desain agar  manusia dapat dengan mudah dan nyaman untuk mengakses informasi yang ada didalamnya. Namun,  bagaimana jika yang mengakses website kita adalah sebuah program komputer? Sudah bukan rahasia umum, jika programmer sering menggunakan cara tidak resmi untuk mendapatkan informasi yang ada dalam sebuah website  dengan teknik screen-scraping misalnya. Namun sayang, program komputer tidak se-fleksibel manusia dalam hal menafsirkan data yang ada dalam sebuah website. Maka, muncullah teknologi seperti RSS, XML-RPC, SOAP, RESTful Web Services, dll. Teknologi-teknologi tersebut dibuat untuk membuat website kita lebih mudah dibaca atau ditafsirkan oleh program komputer.

### Apa Itu RESTful Web Services?

RESTful Web Services mengacu pada sebuah cara atau gaya dalam membangun sebuah sistem web services dengan memanfaatkan semua fitur/potensi yang dimiliki oleh teknologi HTTP (Hypertext Transfer Protcol) dan URI (Uniform Resource Identifier). Berbicara tentang RESTful Web Services maka kita juga harus berkenalan dengan ROA (Resource Oriented Architecture), yaitu sebuah pondasi/panduan dasar untuk membangun RESTful Web Services. Arsitektur ini  diperkenalkan oleh  Leonard Richardson dan Sam Ruby dalam bukunya yang berjudul RESTful Web Services yang diterbikan oleh OâReilly Media tahun 2007 (tips: buku wajib dibaca bagi yang ingin tahu lebih dalam mengenai konsep RESTful Web Services dan  ini open book jadi bisa didownload secara gratis dan legal).

### Pondasi Dasar dari ROA

-  Resources
Dalam ROA resources merupakan bagaimana kita menyebut sesuatu objek yang penting, dibutuhkan oleh client, bentuknya bisa bisa berupa baris dalam tabel database, document, hasil dari menjalankan sebuah algoritma, dll. Lebih lanjut, jika kita lihat dari MVC paradigma, maka resources mengacu pada Model.

- URIs dan Addressability
URI (Uniform Resource Identifier) sangat berkaitan erat dengan resources, karena setiap resource harus memiliki URI, secara sederhana URI dapat dikatakan sebagai nama dan alamat dari sebuah resource yang membuatnya bisa diakses melalui web. Contohnya https://api.domain.com/v1/posts/34, dengan adanya URI tersebut kita dapat dengan mudah mengakses sebuah resource yaitu post dengan id 34.

- Statelessness
Statelessness berarti setiap HTTP request yang diproses di server tidak tergantung dengan HTTP request sebelumnya. Setiap HTTP request harus mengirimkan informasi yang dibutuhkan agar request tersebut bisa diproses di server. 
Orang sering bingung membedakan antara resource state dan application state (session), resource state mengacu pada data yang harus kita simpan diserver, sedangkan application state mengacu pada user session (client session). Dan state yang harus kita jauhkan dari RESTful server kita adalah application state dengan kata lain RESTful Web Services sebaiknya didesain tanpa menggunakan session.
Mengapa statelessness ini penting? 
Karena memudahkan dalam proses horizontal scaling, saat satu server tidak cukup kita tinggal setup load balancer, tambah server kedua, ketiga, dan seterusnya. Proses ini lebih mudah dilakukan tanpa adanya session (client state/application state) yang tersimpan diserver. (lebih lanjut baca halaman 222 di buku RESTful Web Services).

- Representations
Bagaimana resources kita direpresentasikan kepada client inilah yang dimaksud dengan Representations, apakah resources kita akan direpresentasikan dalam format XML, JSON, dan sebagainya. RESTful Web Services yang baik mendukung content negotiation, artinya bagaimana resources kita direpresentasikan kepada client dapat kita atur saat melakukan HTTP request.

- Links and Connectedness
Links and Connectedness mengacu pada bagaimana sebuah representasi dari resources menyajikan link untuk mengakses resources yang lain, contoh paling sederhana adalah pagination link yang disertakan dalam HTTP response.

- The Uniform Interface
The Uniform Interface mengacu pada HTTP verb methods seperti GET, POST, PUT, DELETE, OPTIONS, HEAD, dll. Dalam RESTful web service sebuah URI bisa mendukung banyak http method. 
Contoh untuk URI https://api.domain.com/v1/posts/34, kita dapat memberitahu server bagaimana server tersebut memproses sebuah HTTP request melalui HTTP verb methods. Misalkan kita ingin menghapus resource tersebut maka kita tinggal membuat HTTP request dengan method DELETE ke URI tersebut.
Sebuah URI harus mendukung OPTIONS and HEAD method, dimana OPTIONS memberikan informasi kepada client method apa saja yang didukung, sedangkan HEAD memberikan overview informasi resource dari URI tersebut.

### Membangun RESTful Web Services menggunakan Yii2

Membangun RESTful Web Services menggunakan Yii2 begitu magic. Yii2 sudah dengan baik menerapkan arsitektur ROA (Resource Oriented Architecture) dan terdokumentasi  begitu lengkap di (http://www.yiiframework.com/doc-2.0/guide-rest-quick-start.html). 
Hal yang harus  dilupakan  saat kita akan menggunakan Yii2 untuk membangun Web Services adalah View dari MVC, artinya tidak ada View yang akan kita buat. Kita akan sering main main dengan Model dan sedikitnya dengan Controller dan URL rules configuration. 

- Statelessness
Untuk menjadikan aplikasi Yii2 kita stateless adalah dengan mengkunfigurasi dua properti untuk user pada application component yaitu enableSession menjadi false, dan loginUrl menjadi null pada file konfigurasi kita.

- Links
Tentu Yii2 sudah mendukung salah satu konsep ROA ini, Yii2 akan mengirimkan response Header yang menginformasikan pagination dari resources yang sedang diakses. atau bisa juga kita atur agar link tersebut masuk kedalam response body.

- API Testing
Sebagaimana kita ketahui testing dapat kita lakukan secara manual dan otomatis (UAT vs Automated Code Testing). Begitupun dengan RESTful Web Services yang kita bangun dengan Yii2 dapat kita testing dengan kedua cara tersebut. Secara manual umumnya kita akan menggunakan software api client, yang paling populer adalah chrome app bernama Postman.Secara otomatis, kita  bisa menggunakan codeception (http://codeception.com/for/yii lihat pada bagian API test).
Kita juga dapat melakukan load testing secara otomatis menggunakan https://loader.io, melalui layanan ini, salah satunya kita dapat dengan mudah mengetahui berapa banyak client yang mampu ditangani oleh RESTful server kita dalam waktu bersamaan. 

Referensi:
Dokumentasi Resmi Yii2 (http://www.yiiframework.com/doc-2.0/guide-rest-quick-start.html)
Buku RESTful Web Services (http://restfulwebapis.org/RESTful_Web_Services.pdf)

### FAQ
- Tanya : Best practise penggunaan access token itu bagaimana?
Jawab : Karena restful sifatnya adalah stateless yaitu tidak boleh menyimpan state atau penanda user diserver (misal: session) maka perannya digantikan oleh token.
Pada kasus simple, token itu merepresentasikan seorang user. Sehingga untuk mngakses data maka si user tadi pada setiap request perlu menyertakan token sbg penanda bahwa dia adalah request dari user yg berhak.
- Tanya : Apakah perlu pemisahan berdasarkan versi di API?
Jawab : Umumnya provider besar menggunakan versioning, hal ini untuk menghindari kekacauan pada aplikasi client yang dependet dengan API tersebut
- Tanya : Bagaimana melakukan proteksi pada Restful ?
Jawab : Bisa menggunakan rate-limiting, yaitu dimana menggunakan metode membatasi user yang akses webservice tersebut (misal : 2 request/detik)
- Tanya : Apakah Yii2 sudah support Url Caching?
Jawab :  Yii2 sudah support URL caching.. biasanya ini diimplentasikan untuk actionIndex saja,  aplikasi akan memberitahu client (browser untuk mengcache response melalui HTTP response header). 

(Yii Addict Indonesia)
