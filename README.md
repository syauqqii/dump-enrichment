# YTTA
- buka inspect element
- buka tab console, paste kode :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- ambil `ID` yang dibutuhkan
- copy kode berikut :
```javascript
const URL = 'https://activity-enrichment.apps.binus.ac.id/LogBook/StudentSave';
const MONTHLY_ID = '';
const LINK_FILE_TXT = '';
clear()
```
> *NOTE: pastikan `MONTHLY_ID` & `LINK_FILE_TXT` terisi.

> untuk pengisian file .txt nya bisa lihat format [klik disini](https://raw.githubusercontent.com/syauqqii/dump-enrichment/main/example.txt).
- paste kode diatas ke tab console yang ada diinspect element
- submit / enter kode yang telah di paste tersebut
- copy -> paste lalu submit kode dibawah ini :
```javascript
fetch(LINK_FILE_TXT)
    .then(response => response.text())
    .then(data => {
        const lines = data.split('\n');
        lines.forEach(line => {
            if (line.trim()) {
                const [date, clockin, clockout, activity, description] = line.split('|');

                if (date && clockin && clockout && activity && description) {
                    const postData = {
                        'model[ID]': '00000000-0000-0000-0000-000000000000',
                        'model[LogBookHeaderID]': MONTHLY_ID,
                        'model[Date]': `${date}T00:00:00`,
                        'model[Activity]': activity,
                        'model[ClockIn]': clockin,
                        'model[ClockOut]': clockout,
                        'model[Description]': description,
                        'model[flagjulyactive]': 'false'
                    };

                    const formBody = new URLSearchParams(postData).toString();

                    fetch(URL, {
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
