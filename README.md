# YTTA
- buka inspect element
- buka tab console, paste kode :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- ambil `ID` yang dibutuhkan
- copy kode berikut :
- perhatikan juga pada bagian `PASTE_ID_DISINI` yang ada di variable `monthly_id`, harus diisi dengan hasil dari kode diatas tadi untuk mengambil ID bulan
```javascript
const url = 'https://activity-enrichment.apps.binus.ac.id/LogBook/StudentSave';
const monthly_id = 'PASTE_ID_DISINI';

function replaceSpaces(str) {
    return str.trim().split(' ').join('+');
}

clear()
```
- paste kode diatas ke tab console yang ada diinspect element
- submit / ENTER, kelar
- lalu, copy lagi kode berikut :
- perhatikan pada bagian `LINK_FILE_TXT` harus diisi dengan url ke text yang sesuai, bisa lihat [disini](https://raw.githubusercontent.com/syauqqii/dump-enrichment/main/example.txt) untuk contoh formatnya
```javascript
fetch('LINK_FILE_TXT')
    .then(response => response.text())
    .then(data => {
        const lines = data.split('\n');
        lines.forEach(line => {
            const [date, clockin, clockout, activity, description] = line.split('|');
            
            const postData = {
                'model[ID]': '00000000-0000-0000-0000-000000000000',
                'model[LogBookHeaderID]': monthly_id,
                'model[Date]': `2024-04-${date}T00:00:00`,
                'model[Activity]': replaceSpaces(activity),
                'model[ClockIn]': `${clockin} am`,
                'model[ClockOut]': `${clockout} pm`,
                'model[Description]': replaceSpaces(description),
                'model[flagjulyactive]': 'false'
            };

            const formBody = new URLSearchParams(postData).toString();

            fetch(url, {
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
        });
    })
    .catch(error => console.error('Error fetching data:', error));
```
- paste kode diatas ke tab console yang ada diinspect element
- submit / ENTER, kelar
