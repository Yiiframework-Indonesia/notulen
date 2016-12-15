# Diskusi Tentang Query Menggunakan ActiveRecord

## Diketahui:
Model Karyawan(id, nama, email, gender, umur, status_pernikahan, jumlah_anak, status)
status_pernikahan: jomblo, menikah, duda, janda

## Ditanya:
Buatlah query menggunakan activerecord untuk mencari Karyawan yang 
- jenis kelaminnya cewek
- masih gadis atau jika janda maka jumlah anaknya harus 0
- umur dibawah 25 tahun

## Keterangan: sertakan raw sqlnya

# PEMBAHASAN
Pertama, query yang diinginkan oleh soal ini sebagai berikut.

```php
SELECT * FROM `karyawan` WHERE 
    `gender`='cewek'
    AND (`status_pernikahan`='jomblo' OR (`status_pernikahan`='jomblo' AND `jumlah_anak`=0)) 
    AND (`umur` < 25)
```
    
## Jawaban Fredy

### Kode AR
```php
Karyawan::find()
    ->where([
        'and',
        ['gender' => 'cewek'],
        [
            'or',
            ['status' => 'jomblo'],
            [
                'status' => 'janda',
                'anak'   => 0,
            ]
        ],
        'umur < 25',
    ]);
```

### OUTPUT:
```php
SELECT * FROM `karyawan` WHERE 
    (`gender`='cewek') 
    AND ((`status`='jomblo') OR ((`status`='janda') AND (`anak`=0))) 
    AND (umur < 25)
```

### TANGGAPAN:
- Hasilnya sesuai.
- Kode `'umur < 25',` sebaiknya diganti dengan `['<', 'umur', '25']` dengan alasan lebih aman jika nilai umurnya dinamis.

## Jawaban Rusman

### Kode AR
```php
Karyawan::find()
  ->andWhere("gender = 'cewek'")
  ->andWhere("status_pernikahan = 'jomblo'")
  ->orWhere("status_pernikahan = 'janda' AND jumlah_anak = 0")
  ->andWhere("umur < 25"); 
```

### OUTPUT:
```php
SELECT * FROM `karyawan` WHERE 
    (
        ((gender = 'cewek') AND (status_pernikahan = 'jomblo')) 
        OR (status_pernikahan = 'janda' AND jumlah_anak = 0)
    ) 
    AND (umur < 25)
```

### TANGGAPAN:
- Hasilnya kurang sesuai sebab gender cewek tidak berdiri sendiri sehingga memungkinkan dapet data yang bukan cewek yang penting janda dengan jumlah anak 0 (iya sih janda sudah pasti cewek)
- Kode `andWhere("gender = 'cewek'")` sebaiknya diganti dengan `andWhere(["gender","cewek"])` dengan alasan lebih aman jika nilai gendernya dinamis.

## Jawaban Yusuf Ayuba

### Kode AR
```php
Karyawan::find()
    ->select('gender','status_pernikahan','jml_anak')
    ->where([
        'gender' => 'cewek',
        'umur' => 25
    ])
    ->andWhere([
        'status_pernikahan' => 'jomblo',
    ])
    ->orWhere([
        'status_pernikahan' => 'janda',
    ]);
```

### OUTPUT:
```php
SELECT status_pernikahan `gender` FROM `karyawan` 
WHERE 
    (
        ((`gender`='cewek') AND (`umur`=25)) 
        AND (`status_pernikahan`='jomblo')
    ) 
    OR (`status_pernikahan`='janda')
```

### TANGGAPAN:
- Hasilnya tidak sesuai, ada beberapa kesalahan, salah satunya dimungkinkan dapet data janda yang umurnya lebih dari 25 dan anaknya banyak :)
- Penulisan field select yang kurang tepat `select('gender','status_pernikahan','jml_anak')` seharusnya `select('gender,status_pernikahan,jml_anak')`

## Jawaban Febrianto

### Kode AR
```php
Karyawan::find()
->filterWhere([
   "AND",
   ["jenis_kelamin"=>"wanita"],
   [
      "OR",
      ["status_pernikahan"=>"jomblo"],
      [
         "AND",
         ["status_pernikahan"=>"janda"],
         ["jumlah_anak"=>0],
      ],
      ["<","umur",25]
   ]
])
```

### OUTPUT:
```php
SELECT * FROM `karyawan` 
WHERE 
    (`jenis_kelamin`='wanita') 
    AND (
        (`status_pernikahan`='jomblo') 
        OR ((`status_pernikahan`='janda') AND (`jumlah_anak`=0)) 
        OR (`umur` < 25)
    )
```

### TANGGAPAN:
- Hasilnya tidak sesuai, bisa dimungkinkan dapet wanita menikah dengan umur dibawah 25 tahun :) mau?
- Perbaikan sedikit sebaiknya umur dikeluarkan dari kalang OR, sebagai berikut
```php
Karyawan::find()
->filterWhere([
   "AND"
   ["jenis_kelamin"=>"wanita"],
   [
      "OR",
      ["status_pernikahan"=>"jomblo"],
      [
         "AND",
         ["status_pernikahan"=>"janda"],
         ["jumlah_anak"=>0],
      ],
   ]
   ["<","umur",25]
])
```

## Jawaban Indratmo

### Kode AR
```php
Karyawan::find()
    ->where('gender = :gender', [':gender' => 'cewek'])
    ->andWhere('status = :status', [':status' => 'gadis'])
    ->orWhere('status_pernikahan = :status_pernikahan AND jumlah_anak = :jumlah_anak', [':status_pernikahan' => 'janda',':jumlah_anak' => 0])
    ->andWhere('umur < :umur', [':umur' => 25]);
```

### OUTPUT:
```php
SELECT * FROM `karyawan` WHERE 
    (
        ((gender = 'cewek') AND (status = 'gadis')) 
        OR (status_pernikahan = 'janda' AND jumlah_anak = 0)
    ) 
    AND (umur < 25)
```

### TANGGAPAN:
- Hasilnya kurang sesuai sebab gender cewek tidak berdiri sendiri sehingga memungkinkan dapet data yang bukan cewek yang penting janda dengan jumlah anak 0 (iya sih janda sudah pasti cewek)
- Perbaikan sedikit sebaiknya status gender dan status pernikahan  dan jumlah anak dijadikan satu kalang.
- Mengapa pake titik dua? teringat pendefiniian variabel pada prepare statement PDO kah? :)

## Jawaban Huhu & Peter

### Kode AR
```php
Karyawan::find()
    ->select('nama,gender,umur')
    ->where('gender = "cewek"
      and (status_pernikahan = "jomblo" or (status_pernikahan = "janda" and jumlah_anak = 0))
      and umur < 25' 
    );     
```

### OUTPUT:
```php
SELECT `nama`, `gender`, `umur` FROM `karyawan` WHERE 
    gender = "cewek" 
    and (
        status_pernikahan = "jomblo" or (status_pernikahan = "janda" and jumlah_anak = 0)
    ) 
    and umur < 25
```

### TANGGAPAN:
- Hasilnya sudah sesuai
- Hanya saja penulisan seperti itu kurang fleksibel dan rawan terkait keamanan jika tidak hati2
- Sebaiknya gunakan array

## Jawaban Jovi

### Kode AR
```php
$criteria = new CDbCriteria;
$criteria->condition = 'jenis_kelamin = "perempuan"';
$criteria->addCondition('status_pernikahan = "gadis" OR (status_pernikahan = "janda" AND anak = 0)');
$criteria->addCondition('umur < 25');
$karyawan = Karyawan::model()->findAll($criteria);
```

### OUTPUT:
```php
Entahlah
```

### TANGGAPAN:
- Saya kurang tau kalo yii 1

## Kesimpulan 

Dari semua jawaban diatas, yang sesuai adalah jawabannya Fredy, Huhu, dan Peter. Namun yang paling saya rekomendasikan adalah punya om Fredy
dengan alasan Yii2 AR style serta lebih aman

Dibahas oleh Hafid Mukhlasin 15 Desember 2016 14:26 WIB
