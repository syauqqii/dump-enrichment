## Format Pengisian File
Silahkan [Lihat Format Pengisian File](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian).

## Tutorial Video
[KLIK SINI DEK](https://www.youtube.com/watch?v=3rY8BhAHRhk)

## Cara Penggunaan
1. <b>Buat Bookmark Baru</b>:
 - Buka browser Anda dan buat bookmark baru.
 - Untuk browser seperti Chrome atau Firefox, Anda dapat melakukannya dengan mengklik ikon bintang di bilah alamat atau menekan `Ctrl+D` (Windows) / `Cmd+D` (Mac).
2. <b>Salin Kode Bookmarklet</b>:
 - Salin kode JavaScript di bawah ini:
```javascript
javascript:(function(){
    var FileURL = prompt("Input File URL:");

    if (FileURL) {
        fetch(FileURL)
            .then(response => response.text())
            .then(data => {
                const lines = data.split('\n');
                lines.forEach(line => {
                    if (line.trim()) {
                        const [date, clockin, clockout, activity, description] = line.split('|');

                        if (date && clockin && clockout && activity && description) {
                            const postData = {
                                'model[ID]': '00000000-0000-0000-0000-000000000000',
                                'model[LogBookHeaderID]': document.querySelector('ul[id="monthTab"] li.current a').getAttribute('onclick').split("'")[1],
                                'model[Date]': `${date}T00:00:00`,
                                'model[Activity]': activity,
                                'model[ClockIn]': clockin,
                                'model[ClockOut]': clockout,
                                'model[Description]': description,
                                'model[flagjulyactive]': 'false'
                            };

                            const formBody = new URLSearchParams(postData).toString();

                            fetch('https://activity-enrichment.apps.binus.ac.id/LogBook/StudentSave', {
                                method: 'POST',
                                headers: {
                                    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
                                    'X-Requested-With': 'XMLHttpRequest'
                                },
                                body: formBody,
                                credentials: 'same-origin'
                            })
                            .then(response => response.json())
                            .then(result => {
                                alert("Successfully Uploaded!");
                                console.log(result);
                            })
                            .catch(error => {
                                alert("ERROR: Check inspect element [CONSOLE] TAB");
                                console.error('Error:', error);
                            });
                        }
                    }
                });
            })
            .catch(error => {
                alert("ERROR: Check inspect element [CONSOLE] TAB");
                console.error('Error fetching data:', error);
            });
    }
})();
```
 - Tempelkan kode ini ke dalam field URL pada bookmark baru Anda.
3. <b>Masuk ke Aplikasi Enrichment</b>:
 - Buka aplikasi enrichment di browser Anda.
4. <b>Navigasi ke "Log Book"</b>:
 - Arahkan ke tab "Log Book" dalam aplikasi.
5. <b>Pilih Bulan yang Akan Diisi</b>:
 - Pilih bulan yang sesuai untuk data yang ingin Anda unggah (misalnya: bulan `Juli`).
6. <b>Jalankan Bookmarklet</b>:
 - Klik bookmark yang telah Anda buat sebelumnya.
7. <b>Masukkan URL File</b>:
 - Anda akan diminta untuk memasukkan URL file yang berisi data yang akan diunggah. File tersebut harus tersedia di GitHub dalam format yang sesuai. [Lihat Format](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian)
 - Buka file di GitHub, klik tombol "Raw" untuk mendapatkan URL langsung ke file tersebut.
8. <b>Salin dan Tempel URL</b>:
- Salin URL file raw dari GitHub dan tempelkan ke prompt yang muncul setelah mengklik bookmark.
9. <b>Klik OK</b>:
 - Klik "OK" pada prompt untuk memulai proses unggah.
10. <b>Segarkan Halaman</b>:
 - Setelah proses selesai, segarkan halaman aplikasi untuk melihat data yang telah diunggah.
