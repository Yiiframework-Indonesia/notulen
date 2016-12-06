# RBAC (Role Base Access Contreol)

## Informasi Umum
Sebagaimana kesepakatan sebelumnya bahwa setiap hari senin kita akan diskusi tentang satu topik yang telah ditentukan sebelumnya, dan pada hari senin ini (5/12/2016) topik yang dibahas adalah tentang “RBAC”.

Diharapkan bagi semua member untuk ikut terlibat aktif dalam diskusi ini, kita sama-sama belajar dari best practice yang mungkin pernah dilakukan atau pernah dibaca dan atau dipelajari sebelumnya. Oleh karena diskusi ini tanpa moderator sehingga diharapkan kesadaran bagi para member untuk dapat menciptakan suasana kondusif dan ilmiah sehingga diskusi dapat berjalan dengan baik sesuai dengan harapan kita.

Diskusi pada hari ini akan didahului dengan paparan singkat tentang topik ini oleh Misbahul Munir dan kemudian runtutan jalannya diskusi akan dirangkum oleh notulis Hafid Mukhlasin.

## Paparan Singkat
Perkenalkan, nama saya Misbahul Munir, creator dari “mdmsoft/yii2-admin”, GUI manager for RBAC Yii2. Sesuai dengan hasil poling. Insyaallah hari ini kita akan bahas tentang RBAC. 

### Otorisasi
Berbicara tentang RBAC artinya kita berbicara tentang “otorisasi”. Hal ini karena RBAC adalah “salah satu” cara dari pengaturan otorisasi. 
Pertama-tama, apa itu otorisasi? Otorisasi/Authorization adalah proses yang menjamin agar resource hanya boleh diakses oleh orang yang berhak dan orang yang berhak itu bisa mengakses resource tanpa halangan. 

Tadi disebutkan bahwa RBAC adalah salah satu cara dari pengaturan otorisasi. Ini artinya ada salah-salah cara yang lain selain RBAC. Contoh sederhana pengaturan otorisasi misalnya, memberi flag `admin` pada tabel user. Jika admin, maka boleh melakukan aksi-aksi tertentu. jika bukan admin, maka tidak boleh.

### RBAC
Lalu apa itu RBAC? Kata kuncinya adalah ROLE, Yaitu group/kumpulan dari tugas-tugas tertentu. Artinya, boleh tidaknya seorang user mengakses suatu resource ditentukan oleh punya atau tidaknya user tersebut pada role. Pada prakteknya, role ini dapat memiliki role-role lain sebagai anggotanya. Istilah lain yang berkaitan adalah PERMISSION. Sama seperti role yaitu kumpulan dari tugas-tugas. Hanya saja, jika role lebih berkaitan dengan user-nya, permision sedikit lebih berkaitan dengan job-nya.

![ilustrasi rbac](http://i.imgur.com/98aieOt.jpg)
Berikut ini ilustrasinya

Misal ada beberapa tugas/job yang ingin diatur aksesnya. Yaitu: Create lebur, update lembur, delete lembur, aprove lembur, reject lembur, Create cuti, update cuti, delete cuti, aprove cuti, reject cuti. Maka kita kelompokkan job-job ini pada groupnya
- Lembur: create lembur, update lembur, delete lembur.
- Cuti: create cuti, update cuti, delete cuti.
- AproveLembur: aprovelembur, reject lembur.
- AproveCuti: aprove cuti, reject cuti.
Kemudian hak lembur dan cuti ini kita berikan kepada role karyawan. Lalu hak aprove kita berikan role manager. Kemudian user-user yang ada akan diberikan role sesuai dengan jabatan/posisi yang melekat padanya. 

### Koding
Bagaimana penggunaannya di dalam program? misal di action `cuti/aprove`, maka kita tambahkan cheking
```php
if($user->can('cuti/aprove')){
}
```
system akan mengecek otoritas user berdasarkan hirarki role yang dia punya. jika user adalah manager, berarti dia punya permission `aproveCuti`, dalam permision `aproveCuti` itu ada permisisn `cuti/aprove`. berarti user tersebut boleh mengakses action 'cuti/aprove' 

### ROLE vs PERMISION vs ROUTE
Banyak yang beranggapan bahwa RBAC di Yii adalah berbasis route atau action (dan beberapa orang tidak menyukai itu). Misal sebagian orang ingin membatasi role berdasarkan module atau controller. Anggapan bahwa RBAC yii berbasis route adalah anggapan yang bisa dikatakan salah. Dan anggapan ini kebanyakannya dibentuk dari ext yiiright atau yii2-admin. Yang benar adalah, RBAC yii adalah berbasis ROLE/PERMISSION di mana pengecekannya menggunakan method `User::can()`.
Lalu mengapa yii2-admin mendasarkan pengecekannya pada route/action? Ini hanya karena pendekatan kemudahan. Di mana hanya dengan configurasi sekali, maka pengecekan akan diaplikasikan di seluruh bagian aplikasi.

Mungkin sekian pembukaannya. Kalau tambah bingung, silakan saling bertanya satu sama lain.

> **Pro Tip**
> Jika ada yg kurang sreg dg RBAC yii sehingga mmbuat access control sendiri maka kekurang sregan tadi agak salah alamat.. sebab yii tidak mmbuat konsep rbac.. yii hanya mnerapkan konsep rbac yg sudah mnjadi standard umum pengaturan hak akses. RBAC itu sendiri yang merupakan konsep tersendiri.https://en.wikipedia.org/wiki/Role-based_access_control

> RBAC punya sifat dasar deny, allow. Kalau gak dideskripsikan berarti di deny


### Prons & Cons
**Cons:**
- Konsep RBAC menurut sebagian dev kurang bersahabat, sehingga beberapa dev memilih membuat pengaturan hak akses sendiri dibandingkan dengan menggunakan konsep RBAC yang ditawarkan Yii
- Performa buruk

**Prons:**
- RBACnya yii2 ini sangat solid.
- Yii2 admin merupakan implementasi RBAC Yii dengan beberapa inovasi sehingga lebih mudah dan bersahabat

## FAQ
Beberapa pertanyaan dan jawaban
- Tanya Jawab 1
  - Tanya: Sebelumnya maaf buat para master, dari awal saya kurang suka dengan konsep RBAC, saya merasa konsep ini terlalu teknis sampai hak akses berdasarkan nama action, Saya rasa kurang cocok kalau digunakan langsung oleh user, untuk aplikasi yang beberapa form mungkin tidak terlalu masalah, tetapi kalau sudah ratusan form, tentunya bisa sedikit merepotkan. Awalnya saya mengembangkan akses user berdasarkan main form (yang terdapat CRUD) seperti contoh aplikasi yang sudah pernah saya share yang masih menggunakan yii1. Kemudian ternyata untuk aplikasi yang cukup besar cara ini cukup merepotkan, akhirnya saya buat hak akses berdasarkan unit. Pertimbangannya setiap unit memiliki balasan main form dan ada puluhan unit, kalau di gunakan tiap main form bisa ratusan pilihan hak akses, apalagi kalau tiap action,
  - Jawab: bisa pakai yii2-adminnya om munir, sudah sangat bersahabat. Ga pake nama action kan juga bisa, RBACnya yii2 ini sangat solid. Kyakny ini hanya masalah preferensi, imo justru berbasis action itu lbh baik karena bisa kasi outorisasi lbh detail. Lagian kan bisa pake all action dg "*"

- Tanya Jawab 2
  - Tanya: Bagaimana implementasinya user bisa akses multi departemen ditiap2 departemen tersebut user memiliki group akses tersendiri , dimana ditiap group ini diatur akses tiap modulnya apakah read , update, create atau delete. Tambahan lg user bisa multi lokasi?
  - Jawab: Konsep RBAC emang berjenjang seperti itu. hanyasaja RBAC hanya mengatur sampe level routing bukan tabel..Pembedaan departemen pasti ada di tabel..
Di Yii ada yg namanya permission dan role. Anggap saja departemen itu role grup access itu permission. Role (HR), Permission (admin karyawan),  Sub permission(create update delete add) Nanti di Yii diset role HR punya permission admin karyawan, Dan admin karyawan diset punya permission crud, Dan user assign role HR

- Tanya Jawab 3
  - Tanya : Seorang manager adalah juga karyawan, Apakah usernya dibedakan, jadi ada 2 user untuk karyawan dan manager, Sehingga kalau manager itu ingin cuti dia harus login pakai account user karyawan 
  - Jawab: tidak perlu dibedakan, tapi di assign 2 role, sebagai karyawan dan sebagai manager

  - Tanya: Apakah tidak saling bertentangan jika di assign 2 role, soalnya satu role hanya boleh create dan tidak boleh approve sedangkan yang satunya boleh approve?
  - Jawab: pada saat approve, role user nya gak memenuhi syarat, tapi role managernya memenuhi syarat, apa yang salah? begini kita harus melihatnya "role cuti bukan melarang aprove cuti. hanya tidak memberi hak aprove cuti" jadi kalau user itu diassign dengan role lain, maka haknya yg lain pun akan didapat

  - Tanya: Jadi otomatis punya hak approve ya jika user biasa diberi role approve cuti, misalnya si manager lagi dinas luar atau berhalangan masuk jadi approval si manager bisa diberikan sementara ke user penggantinya?
  - Jawab: iya seperti itu... tapi biasanya ada tambahan lain yang melekat dari ROLE, yaitu RULE. Contoh RULE misalnya, seorang manager hanya boleh meng-aprove cuti dari bawahannya. jadi tidak semua manager boleh mengaprove semua cuti. imho, lebih ke arah kebutuhan proses bisnis saja. yg jelas jika role karyawan & manager dipisah atau digabung bisa diterapkan dgn menggunakan RBAC. 

- Tanya Jawab 4
  - Tanya: Mohon untuk installasinya bisa diperjelas lagi tertuma konfig php sama tabelnya?  K bykan error rbac class not found blum masuk dokumentasi installasi
  - Jawab: Ada di guide penjelasannya, intinya cuma config component rbac. Sama migration (kl pake dbrbac) dan by default  Yii ga ada gui rbac nya. Cuma ada abstrak nya, Tp ttp masih bisa dipake.

- Tanya Jawab 5
  - Tanya: Kenapa rbac bisa diakses dengan can()?
  - Jawab: can() ini kan sebuah method yg berfungsi untuk cek apakah si user punya permission nya atau tidak.

- Tanya Jawab 6
  - Tanya: Oh ya om fredy ini kalau gak salah kemarin, yg bilang bikin rbac/permission control itu diletakan dimodel dimana disitu juga ada/dapat mendifinisikan create button, sehingga dinamapun tidak terjadi keselip sebuah kondisi yg seharusnya tidak ada menjadi ada, kemarin secara langsung ane mencoba menerapkan itu sampe membuat abstract class juga yg mengextends active record, tapi ketika sampe ditengan, ini yg ane batesin model apa controller sih?
  - Jawab: Yg dibatesin controller action & link/button nya saja

  - Tanya: Bagaimana kita membatasi sebuah controller dari model?
  - Jawab: bisa saja seperti ini:
```php
function actionUpdate(){
...
if($model->checkAccess('update')==false){
 throw new ForbiddenHttpException($model->getError('update'));
}
...
}
```
  - Tanya: sangat manual sekali.. :) gak kebayang begitu kompleknya kode di controller, repot sekali ya?
  - Jawab: betul. karena niatnya wrapper Logic Filter bukan RBAC. Karena filter logic sendiri udah agak repot. jadi klo mau nerapin RBAC agak kewalahan. 

- Tanya Jawab 7
  - Tanya: Sy dulu pk SRBAC yii-1, tiap action harus unik. Nah utk RBAC di yii-admin / mimin gitu juga ga ya? Terakhir sy pakai Action harus unik. Ga tau klo ada versi terbaru. Jd misal dikasih akses actionCreate di controller A. Nah dia bisa akses actionCreate di controller lain. Kecuali klo namanya di ubah.
  - Jawab: tidak harus.

- Tanya Jawab 8
  - Tanya: kenapa query ini muncul banyak banget, kayaknya  hampir/bahkan semua yang memanggil User::can() query ini di eksekusi ? 
```php
SELECT * FROM `tbl_auth_item` WHERE `name`='admin'
```
kalau ada 30 User::can() mungkin query itu akan muncul di log sekitar 30 kali juga,, dan sebagai info name=admin disitu bertindak sebagai "role" atau dalam tabel auth_item ber type = 1, menyambung tadi, adakah cara untuk meminimalis query tersebut, seperti apa yang dilakukan om Mdm untuk bagian rule
https://github.com/mdmsoft/yii2-admin/blob/master/components/DbManager.php
  - Jawab: Nah. Ini alasan sy ga pakai SRBAC. Bahkan query exev time bisa lbh besar dr 1000ms utk ratusan user role. 🙊😬 (agak OOT). Pake cache untuk mengatasi performa RBAC.

- Tanya Jawab 9
  - Tanya: jadi bingung...  kalau yang seperti ini dinamakan apa ya?
```php
'access' => [
  'class' => AccessControl::className(),
    'ruleConfig' => [
          'class' => AccessRule::className(),
      ],
  'rules' => [
      [
          'actions' => ['index','lulus','delete','view','cetak'],
          'allow' => true,
          'roles' => ['Admin'],
      ],
      [
        'actions' => ['lulus','registrasi','selesai','cetak'],
        'allow' => true,
        'roles' => ['?'],
      ]
  ],
],
```
  - Jawab: ACL (Access Control List), 

- Tanya Jawab 10
  + Tanya: berarti RBAC itu rule nya sudah di deskripsikan terlebih dahulu baik di database atau di sebuah file ya om?
  - Jawab: Iya
- Tanya Jawab 11
  + Tanya: Seorang guru bisa mengakses halaman daftar murid yg dia ajar siapa aja dimana di halaman ini ada filter tahun ajaran, level kelas, dan ruang kelas.dimana filter nya berupa dropdown dan data nya diambil via ajax. Jd selain guru punya hak akses ke halaman tsb dia jg punya hak akses utk list level kelas dan ruangan kelas yg bersifat read only?
  - Jawab: Kayaknya itu gak dilevel rbac.. Tapi di level konten.. kalau saya ditaroh di loadModel ngecek current user id dan cek groupnya di db.. Kalau groupnya dikasi maka pass kalau gak dikasi maka error 404. Bisa juga buat Helper/komponem yang return true dan false.

  + Tanya: Coba saya detailkan lagi. Guru akses halaman 'guru/default/index' utk halaman list anak2 yg dia ajarkan. Di halaman ini ada filter level kelas misalnya kelas 1, 2, dsb. Kemudian ada filter ruangan kelas misal kelas 1A, 1B dsb. Dimana filter level kelas akan ambil data secara ajax di halaman 'master/levelkelas/list' dan filter ruangan kelas ambil data di halaman 'master/ruangkelas/list'. Berarti kita harus assign hak akses ke guru 'guru/default/index', 'master/levelkelas/list' dan 'master/ruangkelas/list'. Kalau di halaman rbac kita harus centang ketiga item tsb. Nah ada yg punya pengalaman seperti ini?
  - Jawab: Paling enak bikin tabel matrixnya.. User vs fitur Matrix akan membantu merancang strategi rbac-nya. Sy tipe programmer yang merancang fitur per controller... Jadi satu group user mendapat seluruh fitur action dalam satu controller, Itu utk menjawab pertanyaan yang bilang gimana kalau action sy banyak...
Kalau ada group user yang punya hak akses berbeda terhadap fitur yang sama, sy lebih baik bikin controller baru

Tanya3‬: Nah seting rbac nya gmn?
Jawab3: Misalnya fitur data karyawan.. bisnis unit bisa CRUD tapi kantor pusat hanya REad Only . Mendingan saya bikin controller baru dan assign rbac khusus holding.. dan remove semua action yg tdk dibutuhkan.

Tanya4: Kalau fitur lain butuh data yg sama apakah di buat controller baru lg?
Jawab4: Jadi sy gak bikin satu controller yang bisa dipake sama unit dan holding.. ribet ntar setting rbacnya... Sy mesti check hingga ke level action.. mana yang boleh mana yg tidak. 

Tanya5: gak pakai prinsip DRY ya om peter? dont repeat yourself
Jawab5: Keliatannya dry tapi efisien di view karena sy gak perlu checkAccess. Ketika holding punya kebutuhan khusus sy gak usah pusing inget2 mana yang punya holding mana yang punya unit. Tapi tehnik ini tidak terjadi di semua fitur.. Untuk fitur Forum yang saya anggap lebih simple.. di level view ada checkaccess utk ngecek ini postingan siapa, boleh atau readonly

Tanya6: Berarti kalau ada 5 fitur yg butuh data yg sama, maka ada 5 controller yg akan menghasilkan data yg sama ya?
Jawab6: Iyah.. tapi saat ini saya paling banyak dua..  Karena satu fitur actionnya banyakk...  Kalau cuman akses tabel master rasanya actionnyanya gak banyak..
Bisa aja lsg disetting dari rbac, Liat kepentingan praktisnya, Rbac punya kemampuan hirarki.. Operation bisa di group, task bisa di grouptask, role juga..

Tanya7: Principal, guru, bagian akademi, bagian psikiater, bagian acs (semacam tata usaha) punya fitur sendiri2 dan kadang ambil data yg sama. Misalnya list ruangan kelas
Jawab7: Tasknya harus didefinisi satu per satu tapi bisa dikelompokkan jadi satu task..

Tanya8: Nah itu maksudnya cara praktis admin cukup assign 1 rbac utamanya, rbac turunan nya otomatis ke assign 🙈😂😂
Jawab8: Bisa dunk, Role bisa punya member role lain..

- **Tanya Jawab 12**
  * Tanya: klo kita desain user yg dibayangan kita user ini nanti akan terdiri dari bbrpa kategori user, apakah kategori2 user tsb cukup dibuat dlm bentuk role aja atau lebih baik kita punya table kategori_user utk menyimpan data kategori usernya. kategori user ini tujuannya nantinya utk membedakan tampilan dashboard dr msg2 kategori user. mohon pencerahannya
  - Jawab: Itu cukup pake role saja

- **Tanya Jawab 13**
  * Tanya: mengapa om peter membuat penamaan controller dengan prefix abjad diawal tiap file?
  - Jawab: itu yii1, tujuannya biar File2 yang related berdekatan.. namun di Yii2 udah gak perlu lagi karena ada namespace so bisa digrouping menggunakan folder

- **Tanya Jawab 14**
  * Tanya: Btw dg role yg begitu banyak tentunya performa app akan trasa berat hanya utk ngurusi rbac saja..
Bagaimana kita2 om pet utk mminimalkan hal itu? Saat di yii 1 dan kmudian setelah migrasi ke yii2?
  - Jawab: dengan menggunakan cache, itulah opsi saya satu2nya.. sebetulnya ada lagi.. ganti ke db server ke postgres .. dari pengalaman ujicoba dengan query yang ruwet.. postgres lebih cepet keluarnya..

- **Tanya Jawab 15**
  * Tanya: Btw mmbuat app yg komplek seperti itu.. bagaimana om pet merancang rbacnya.. khususnya pengaturan rolenya biar ga tumpang tindih?
  - Jawab: Minimal ada matrix. salah satunya adalah saya nggak gitu banyak pake Operation/Route .. lebih mudah dikontrol dari controller..
  * Tanya: Gabungan yak.. rbac cuman level controller aja selebihnya ACL?
  - Jawab2: fully rbac... ACL hanya saya pake di webservice API-nya.. karena relatif nggak banyak aturan
  
- **Tanya Jawab 16**
  * Tanya: Baik, mnurut temen2 yg pernah pke yii2 admin.. fitur  apa yg perlu ditambah / dikurangi? Ataukah kalian masih mngkustomisasi yii2 admin? Mngapa?
  - Jawab: kalau saya yii2-admin sudah sangat sesuai kebutuhan... gak ada yang perlu dicustom.. bahkan dibanding yii-right, yii2-admin jauh lebih simple dan lebih lengkap.

- **Tanya Jawab 17**
  * Tanya: How to caching rbac data?
  - Jawab: saya sempat riset, rbac berbasis file ternyata lebih cepat daripada yang berbasis db... ide liar aja sih, yang didb diexport ke file dan rbac baca dari file.. good idea or not? saya sih punya keyakinan kalau ini bakal bikin performance naik 5x lipat..  suka serem saya liat profiling.. tanpa rbac aja, dari yii1-nya aja udah banyak bikin query.. untung yii2 udah nggak gitu..

- **Tanya Jawab 18**
  * Tanya: Apakah temen pnggna yii2 admin.. mnduga bahwa cak munir sebagai foundernya lupa mngimplementasikan cache?
  - Jawab: Cache merupakan optimalisasi..kalo udah ada cache duluan berarti melakukan premature optimalization CMIIW.. Namun Kalo misalnya efeknya udah jelas, dan effortnya gak susah,, sebenernya gk masalah

- **Tanya Jawab 19**
  * Pernyataan: rbac bisa pakai module/controller/*
  - Tangapan: ini juga contoh salah paham rbac. banyak yg saling tertukar pengertian antara RBAC dan yii2-admin. Fitur itu ada di yii2-admin dan bukan di RBAC

## Testimoni & Opinion
- Klo saya pernah pakai SRBAC. Karna agak kurang puas jadinya bikin auth sendiri yg mengatur level control hingga action. Syntax nya tetep paka accessRules. Tp rules dan role di atur pke database sendiri. yii1
- Trus dr perfomance pengambilan query bagusan bikinan sendiri dibanding SRBAC. Karna pk AR dan struktur table nya ga ada relation kali ya. Aq bikin pk DAO / Query builder. Biar cepet. yii1
- Kalo dulu di yii1 ane bikin mekanismenya buat class components extend CFilter, trus check di function preFilter. Lalu di setiap file controller lampirin satu baris kode buat panggil class component tadi. Selama ini sih jalan, cuma gak tau efektif menurut kawan2 atau gak?
- Basicnya sy cuma pake RBAC buat pembatasan akses controller/*, jadi yg bisa masuk cm authenticated user. 😁 Selebihnya fitur2 yg lain blm dicoba.
- Mending bikin alternatif pengaturan hak akses sendiri, yaitu hanya kode logic biasa yg dikumpulin ke suatu class. satu model independent dibuat satu class. sekaligus buat generate tombol operasi yg tersedia u/ tiap record.
- Model RBAC lain yang lebih "bersahabat" sepertinya konsep yang digunakan dalam easyiicms. Konsep admin loginnya juga sedikit berbeda.
- Memahami RBAC paling enak pake study kasus drpd teori..
- Saya pake yii2 admin tapi saya tambah user group supaya assign user dengan role yg sama lebih mudah

(Yii Addict Indonesia)

[Kembali ke Index](README.md)
