## Format Pengisian :
```
startDate|endDate|title|activity
```

## Contoh Pengisian :
```
2024-09-02|2024-09-02|memasak|memasak rendang yang enak banget!

*ADDITIONAL: atau bisa juga seperti ini contoh dibawah ini

2024-09-02|.|memasak|memasak rendang yang enak banget!

*NOTE: endDate diisi '.' berarti endDate dan startDate sama, jadi tidak perlu ribet memasukkan 2 kali startDate dan endDate
```

## Kode Javascript :
```javascript
javascript: (function() {
    let token = "";

    function loadBootstrap() {
        if (!document.querySelector('link[href*="bootstrap"]')) {
            let link = document.createElement('link');
            link.rel = 'stylesheet';
            link.href = 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css';
            document.head.appendChild(link);
        }
        if (!document.querySelector('script[src*="bootstrap"]')) {
            let script = document.createElement('script');
            script.src = 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js';
            script.onload = createPopup;
            document.body.appendChild(script);
        } else {
            createPopup();
        }
    }

    function unloadBootstrap() {
        let link = document.querySelector('link[href*="bootstrap"]');
        let script = document.querySelector('script[src*="bootstrap"]');
        if (link) link.remove();
        if (script) script.remove();
    }

    function createPopup() {
        let popup = document.createElement('div');
        popup.id = 'progressPopup';
        popup.className = 'modal fade';
        popup.innerHTML = `
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">Proses Upload</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" onclick="destroyPopup()"></button>
                    </div>
                    <div class="modal-body">
                        <div class="progress">
                            <div id="progressBar" class="progress-bar" role="progressbar" style="width: 0%;" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
                        </div>
                    </div>
                </div>
            </div>`;
        document.body.appendChild(popup);
        new bootstrap.Modal(popup).show();
    }

    function updateProgress(current, total) {
        let progressBar = document.getElementById('progressBar');
        let percentage = Math.round((current / total) * 100);
        progressBar.style.width = percentage + '%';
        progressBar.setAttribute('aria-valuenow', percentage);
        progressBar.textContent = percentage + '%';
    }

    function destroyPopup() {
        let popup = document.getElementById('progressPopup');
        if (popup) {
            bootstrap.Modal.getInstance(popup).hide();
            popup.remove();
            unloadBootstrap();
        }
    }

    fetch('https://incubator.apps.binus.ac.id/api/auth/session').then(response => response.json()).then(data => {
        if (data.token) {
            token = data.token.toString();
            mainProcess();
        } else {
            alert('Token tidak ditemukan');
            return;
        }
    }).catch(error => {
        alert('Terjadi kesalahan: ' + error);
        return;
    });

    function mainProcess() {
        let picInput = document.querySelectorAll('input[name="pic"]')[1];

        if (picInput === undefined) {
            alert("Klik Menu [+ Add Activity]");
            return;
        }

        if (!picInput.value.trim()) {
            alert("Silahkan pilih nama anda terlebih dahulu di opsi form bagian PIC yang ada di menu [+ Add Activity]");
            return;
        } else {
            var FileURL = prompt("Input File URL:");
            if (FileURL) {
                fetch(FileURL).then(response => response.text()).then(data => {
                    const lines = data.split('\n').filter(line => line.trim() && line !== '-');
                    let totalLines = lines.length;
                    let processedLines = 0;
                    let hasErrors = false;
                    let errorDetails = [];

                    loadBootstrap();

                    function processNextLine(index) {
                        if (index >= lines.length) {
                            destroyPopup();
                            if (hasErrors) {
                                alert('Proses selesai dengan beberapa kesalahan pada baris: ' + errorDetails.map(e => e.line).join(', '));
                            } else {
                                alert('Proses selesai tanpa kesalahan');
                            }
                            return;
                        }

                        const [start, endInput, name, description] = lines[index].split('|');
                        const formatDate = (dateStr) => {
                            const [year, month, day] = dateStr.split('-');
                            return `${day}-${month}-${year}`;
                        };

                        let startDate = start.trim();
                        let endDate = endInput.trim() === '.' ? startDate : endInput.trim();

                        if (new Date(endDate) < new Date(startDate)) {
                            hasErrors = true;
                            errorDetails.push({ line: index + 1 });
                            processNextLine(index + 1);
                            return;
                        }

                        const postData = {
                            'start': formatDate(startDate),
                            'end': formatDate(endDate),
                            'name': name.trim(),
                            'description': description.trim(),
                            'pic': picInput.value.trim(),
                            'predecessor_id': ''
                        };

                        fetch('https://incubator-backend.apps.binus.ac.id/api/startup/activity/add-planning', {
                            method: 'POST',
                            headers: {
                                'Authorization': 'Bearer ' + token,
                                'Content-Type': 'application/json',
                                'Accept': 'application/json, text/plain, */*',
                                'Accept-Encoding': 'gzip, deflate, br, zstd',
                                'Accept-Language': 'en-US,en;q=0.5',
                                'Connection': 'keep-alive',
                                'Origin': 'https://incubator.apps.binus.ac.id',
                                'Referer': 'https://incubator.apps.binus.ac.id/',
                                'Sec-Fetch-Dest': 'empty',
                                'Sec-Fetch-Mode': 'cors',
                                'Sec-Fetch-Site': 'same-site',
                                'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0',
                                'X-Requested-With': 'XMLHttpRequest'
                            },
                            body: JSON.stringify(postData)
                        }).then(() => {
                            processedLines++;
                            updateProgress(processedLines, totalLines);
                            setTimeout(() => processNextLine(index + 1), 1000);
                        }).catch(error => {
                            hasErrors = true;
                            errorDetails.push({ line: index + 1 });
                            processedLines++;
                            updateProgress(processedLines, totalLines);
                            setTimeout(() => processNextLine(index + 1), 1000);
                        });
                    }

                    processNextLine(0);
                }).catch(error => {
                    alert('Terjadi kesalahan: ' + error);
                });
            }
        }
    }
})();
```
