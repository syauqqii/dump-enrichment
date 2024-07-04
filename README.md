# YTTA
- buka inspect element
- buka tab console, paste kode :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- ambil `ID` yang dibutuhkan
- copy kode berikut :
- perhatikan terdapat `ANU.YTTA` yang harus diganti dengan URL activity anu
- perhatikan juga pada `bagian monthly_id`, harus diisi dengan hasil dari kode diatas tadi untuk mengambil ID bulan
```javascript
const url = 'https://ANU.YTTA/LogBook/StudentSave';
const monthly_id = 'PASTE_ID_DISINI';
```
- paste kode diatas ke tab console yang ada diinspect element
- submit / ENTER, kelar
- lalu, copy lagi kode berikut :
- perhatikan pada bagian `LINK_FILE_TXT` harus [disini](https://raw.githubusercontent.com/syauqqii/dump-enrichment/main/example.txt) dengan url ke text yang sesuai, bisa lihat disini untuk contoh formatnya
```javascript
function replaceSpaces(str) {
    return str.trim().split(' ').join('-');
}

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
