## 📝 Format Pengisian File
Lihat format pengisian file [di sini](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian).

## 📹 Tutorial Video
Buka [YouTube](https://www.youtube.com/watch?v=3rY8BhAHRhk).

## 🤔 Cara Penggunaan
#### **Buat Bookmark Baru**:
Tekan Ctrl+D (Windows) / Cmd+D (Mac) untuk membuat bookmark baru.
#### **Salin Kode Bookmarklet (kode JS dibawah ini)**:
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
#### **Masuk ke Aplikasi Enrichment**:
Buka aplikasi enrichment di browser Anda.
#### **Navigasi ke tab "Log Book"**:
Arahkan ke tab "Log Book" dalam aplikasi.
#### **Pilih Bulan yang Akan Di isi**:
Pilih bulan yang sesuai untuk data yang ingin Anda unggah (misalnya: bulan `Juli`).
#### **Jalankan Bookmarklet**:
Klik bookmark yang telah Anda buat sebelumnya.
#### **Masukkan URL File**:
Anda akan diminta untuk memasukkan URL file yang berisi data yang akan diunggah. File tersebut harus tersedia di GitHub dalam format ([di sini](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian)) yang sesuai.
Buka file di GitHub, klik tombol "Raw" untuk mendapatkan URL langsung ke file tersebut.
#### **Salin dan Tempel URL**:
Salin URL file raw dari GitHub dan tempelkan ke prompt yang muncul setelah mengklik bookmark.
#### **Klik OK**:
Klik "OK" pada prompt untuk memulai proses unggah.
#### **Refresh Halaman (F5)**:
Setelah proses selesai, segarkan halaman aplikasi untuk melihat data yang telah diunggah.
