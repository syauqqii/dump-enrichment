# YTTA
- buka inspect element
- buka tab console, paste kode :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- ambil id bulan yang dibutuhkan
- copy kode berikut :
```javascript
const url = 'https://{ANU_YTTA}/LogBook/StudentSave';
var data = {
    'model[ID]': '00000000-0000-0000-0000-000000000000',
    'model[LogBookHeaderID]': '{PASTE_ID_BULAN_DISINI}',
    'model[Date]': '2024-04-{GANTI_TEXT_INI_JADI_TANGGAL}T00:00:00',
    'model[Activity]': '{GANTI_TEXT_INI_JADI_ACTIVITY}',
    'model[ClockIn]': '{GANTI_TEXT_INI_JADI_JAM} am',
    'model[ClockOut]': '{GANTI_TEXT_INI_JADI_JAM} pm',
    'model[Description]': '{GANTI_TEXT_INI_JADI_DESCRIPTION}',
    'model[flagjulyactive]': 'false'
};

const formBody = new URLSearchParams(data).toString();

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
```
- paste kode diatas ke tab console yang ada diinspect element
- submit / ENTER, kelar
