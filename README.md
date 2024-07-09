## Format Pengisian File
Silahkan [Lihat Format Pengisian File](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian).

## Cara Penggunaan
- Kunjungi halaman activity enrichment app
- Buka tab "Log Book"
- Klik kanan, lalu `Inspect Element`
- Pada `Inspect Element`, buka tab `Console`
- Salin kode dibawah ini :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- Setelah menyalin kode diatas, `paste` kode tersebut di `Console` pada `Inspect Element`
- Akan muncul list monthly ID, kalian `salin ID` yang akan anda isi "Log Book"nya
- Salin kode dibawah ini :
```javascript
const MonthlyID = '';
const FileURL = '';
clear()
```
- Setelah menyalin kode diatas, pastikan anda telah mengisi `MonthlyID` dan `FileURL`, lalu `paste` kode tersebut di `Console` pada `Inspect Element`
|Variabel|Penjelasan|
|---|---|
|MonthlyID|Isi variabel ini dengan yang anda salin di poin ke-7|
|FileURL|Isi variabel ini dengan link file yang anda upload di github lalu cek di bagian raw|
- Salin kode dibawah ini :
```javascript
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
                        'model[LogBookHeaderID]': MonthlyID,
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
                    .then(result => console.log(result))
                    .catch(error => console.error('Error:', error));
                }
            }
        });
    })
    .catch(error => console.error('Error fetching data:', error));
```
- Setelah menyalin kode diatas, `paste` kode tersebut di `Console` pada `Inspect Element`
