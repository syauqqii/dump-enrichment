# YTTA
- buka inspect element
- buka tab console, paste kode :
```javascript
document.querySelectorAll('a[onclick]').forEach(element => { let monthText = element.textContent.trim(); let match = element.getAttribute('onclick').match(/tabClick\('([^']+)'\)/); if (match) console.log(`${monthText}: ${match[1]}`); });
```
- ambil id bulan yang dibutuhkan
- copy kode berikut :
```javascript
const url = 'https://HOST_URL_PASTE_SINI/LogBook/StudentSave';
```
- pastikan `HOST_URL_PASTE_SINI` telah diisi sesuai dengan anu, ytta 😊
- paste kode tersebut di tab console yang ada diinspect element
- enter / submit kodenya di console
- lalu, lakukan copy lagi kode berikut ini :
```javascript
var monthly_id = '';
var date = 1;
var activity = '';
var clockin = '09:30';
var clockout = '05:00';
var description = '';

var data = {
    'model[ID]': '00000000-0000-0000-0000-000000000000',
    'model[LogBookHeaderID]': monthly_id,
    'model[Date]': '2024-04-'+date+'T00:00:00',
    'model[Activity]': activity,
    'model[ClockIn]': clockin+' am',
    'model[ClockOut]': clockout+' pm',
    'model[Description]': description,
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
