## Format Pengisian File
Silahkan [Lihat Format Pengisian File](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian).

## Cara Penggunaan
- Buat bookmark, lalu copy dan paste kode dibawah ini ke dalam value bookmark kalian
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
                            .then(result => console.log(result))
                            .catch(error => console.error('Error:', error));
                        }
                    }
                });
            })
            .catch(error => console.error('Error fetching data:', error));
    }
})();
```
- Masuk ke enrichment apps
- Buka tab "Log Book"
- Buka tab bulan yang akan diisi (misalnya: bulan juli)
- Klik bookmark yang telah dibuat sebelumnya
- Anda akan dimintai link file yang anda upload di github ([Lihat Format](https://github.com/syauqqii/dump-enrichment/tree/main/Format%20Pengisian)) lalu cek di bagian raw
- Copy link atas tersebut (file raw github kalian), lalu pastekan ke prompt yang diminta oleh bookmark yang anda click
- Klik OK
- Refresh halaman website
