```javascript
javascript: (function() {
    let token = "";
    fetch('https://incubator.apps.binus.ac.id/api/auth/session').then(response => response.json()).then(data => {
        if (data.token) {
            token = data.token.toString();
        } else {
            alert('Token tidak ditemukan');
            return;
        }
    }).catch(error => {
        alert('Terjadi kesalahan: ' + error);
        return;
    });

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
                const lines = data.split('\n');
                let hasErrors = false;
                let errorDetails = [];
                let increment = 0;
                const uploads = lines.map((line) => {
                    if (line.trim() && line !== '-') {
                        const [start, endInput, name, description] = line.split('|');
                        const formatDate = (dateStr) => {
                            const [year, month, day] = dateStr.split('-');
                            return `${day}-${month}-${year}`;
                        };
                        increment++;
                        let startDate = start.trim();
                        let endDate = endInput.trim();
                        if (endDate === '.') {
                            endDate = startDate;
                        } else if (!startDate && !endDate) {
                            hasErrors = true;
                            errorDetails.push({
                                line: increment
                            });
                            return Promise.resolve();
                        } else if (!endDate) {
                            hasErrors = true;
                            errorDetails.push({
                                line: increment
                            });
                            return Promise.resolve();
                        } else if (!startDate) {
                            hasErrors = true;
                            errorDetails.push({
                                line: increment
                            });
                            return Promise.resolve();
                        }
                        if (new Date(endDate) < new Date(startDate)) {
                            hasErrors = true;
                            errorDetails.push({
                                line: increment
                            });
                            return Promise.resolve();
                        }
                        if (startDate && endDate && name && description) {
                            const postData = {
                                'start': formatDate(startDate),
                                'end': formatDate(endDate),
                                'name': name.trim(),
                                'description': description.trim(),
                                'pic': picInput.value.trim(),
                                'predecessor_id': ''
                            };
                            return fetch('https://incubator-backend.apps.binus.ac.id/api/startup/activity/add-planning', {
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
                                body: JSON.stringify(postData),
                                credentials: 'same-origin'
                            }).then(response => response.json()).catch(error => {
                                hasErrors = true;
                                console.error('Error upload data:', error);
                            });
                        }
                    }
                    return Promise.resolve();
                });
                Promise.all(uploads).then(() => {
                    if (hasErrors) {
                        if (errorDetails.length > 0) {
                            alert("Upload gagal untuk data berikut karena kesalahan:\n" + errorDetails.map(e => `\nLine: ${e.line}`).join(''));
                        } else {
                            alert("Beberapa upload gagal. Silakan periksa [CONSOLE] untuk detailnya.");
                        }
                    } else {
                        alert("Semua data berhasil diupload!");
                    }
                });
            }).catch(error => {
                alert("ERROR: Periksa inspect element di TAB [CONSOLE]");
                console.error('Error mengambil data:', error);
            });
        }
    }
})();
```
