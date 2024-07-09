### Catatan Penting :
Jika sudah kalian isi semua, pastikan menghapus baris yang terdapat `-`, karena pada baris itu saya set bahwa hari tersebut adalah libur (minggu) secara default di website enrichment.

Contoh :
```
Dari :
----------------------------------------------------
2024-06-01|CLOCK_IN|CLOCK_OUT|ACTIVIY|DESCRIPTION
-
2024-06-03|CLOCK_IN|CLOCK_OUT|ACTIVIY|DESCRIPTION

Menjadi :
----------------------------------------------------
2024-06-01|CLOCK_IN|CLOCK_OUT|ACTIVIY|DESCRIPTION
2024-06-03|CLOCK_IN|CLOCK_OUT|ACTIVIY|DESCRIPTION
```

### Penjelasan
|Variabel|Penjelasan|
|---|---|
|`CLOCK_IN`|ganti dengan `jam+"am"` / set `OFF` jika kalian libur / ada keperluan lain|
|`CLOCK_OUT`|ganti dengan `jam+"pm"` / set `OFF` jika kalian libur / ada keperluan lain|
|`ACTIVITY`|ganti dengan `aktifitas kalian`|
|`DESCRIPTION`|ganti dengan `deskripsi kalian`|

### Contoh Pengisian :
- Jika tidak libur
`2024-06-01|09:30 am|05:00 pm|Belajar|Belajar dengan menonton youtube`

Contoh tampilan diatas, jika diupload ke enrichment app :
|DATE|CLOCK IN|CLOCK OUT|ACTIVITY|DESCRIPTION|
|---|---|---|---|---|
|2024-06-01|09:30 am|05:00 pm|Belajar|Belajar dengan menonton youtube|

- Jika libur / ada keperluan lain (tanpa aktifitas / deskripsi)
`2024-06-01|OFF|OFF|OFF|OFF`

Contoh tampilan diatas, jika diupload ke enrichment app :
|DATE|CLOCK IN|CLOCK OUT|ACTIVITY|DESCRIPTION|
|---|---|---|---|---|
|2024-06-01|OFF|OFF|OFF|OFF|

- Jika libur / ada keperluan lain (dengan aktifitas / deskripsi)
`2024-06-01|OFF|OFF|Sakit|Sakit demam tinggi`

|DATE|CLOCK IN|CLOCK OUT|ACTIVITY|DESCRIPTION|
|---|---|---|---|---|
|2024-06-01|OFF|OFF|Sakit|Sakit demam tinggi|
