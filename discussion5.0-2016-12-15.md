# Diskusi Tentang ActiveRecord

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
### AR
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

### OUTPUT:
SELECT * FROM `karyawan` WHERE (`gender`='cewek') AND ((`status`='jomblo') OR ((`status`='janda') AND (`anak`=0))) AND (umur < 25)

### TANGGAPANKU:
Kode `'umur < 25',` sebaiknya diganti dengan `['<', 'umur', '25']`

