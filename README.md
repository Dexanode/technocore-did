# Technocore DID via GitHub Codespaces

Tutorial lengkap Bahasa Indonesia untuk membuat DID, join Technocore, membuat
kontribusi, mencatatnya sebagai signed message, dan membagikan public proof—cukup
dari browser HP atau tablet, tanpa PC/laptop.

> **No guaranteed airdrop.** Menyelesaikan tutorial ini bukan jaminan mendapat
> `$FLOP`. Eligibility dan reward mengikuti aturan resmi
> [@flop_labs](https://x.com/flop_labs).

## Sebelum mulai: pahami risikonya

GitHub Codespaces adalah komputer cloud, bukan perangkat lokal. Private key akan
dibuat sementara di Codespace milik akun GitHub kamu.

- Jangan gunakan seed phrase atau private key wallet.
- Jangan gunakan password akun sebagai passphrase DID.
- Jangan commit atau push `identity.pem`.
- Download backup `identity.pem`, lalu hapus Codespace setelah selesai.
- Siapa pun yang menguasai akun GitHub/Codespace dan passphrase berpotensi
  menguasai DID tersebut. Aktifkan 2FA GitHub.

Kalau punya PC pribadi yang aman, membuat identity secara lokal tetap pilihan
yang lebih baik. Jalur Codespaces ini ditujukan untuk pengguna tanpa PC/laptop.

## Apa itu DID?

DID di tutorial ini adalah identitas publik berbasis Ed25519:

```text
private key → public key → did:key:z6Mk...
     ↓
sign: room | nonce | normalized text
     ↓
Technocore memverifikasi signature
     ↓
server memberikan timestamp + sequence
```

- `identity.pem` adalah private key terenkripsi. Selalu privat.
- `did:key:z6Mk...` adalah public DID. Boleh dibagikan.
- `nonce` membedakan signed write dan membantu mencegah replay.
- `sequence` adalah nomor record yang diberikan server setelah pesan diterima.

Signature membuktikan pemegang private key menandatangani payload. Signature
tidak otomatis membuktikan nama asli, reputasi, kepemilikan akun X, atau jaminan
reward.

---

## Step 1 — Buka GitHub Codespaces

Pastikan sudah login GitHub, lalu:

1. Buka [repo ini](https://github.com/Dexanode/technocore-did).
2. Tekan tombol **Code**.
3. Pilih tab **Codespaces**.
4. Tekan **Create codespace on main**.
5. Tunggu sampai editor VS Code dan terminal terbuka di browser.

Jika memakai HP, aktifkan mode desktop browser agar terminal lebih mudah dipakai.
Codespaces memakai kuota compute/storage akun GitHub. Hapus Codespace setelah
selesai agar tidak terus memakai storage.

## Step 2 — Download reference client

Repo tutorial ini tidak menyertakan atau menyalin source code pihak lain. Kita
akan mengambil reference client langsung dari repository pembuatnya ke folder
kerja sementara.

Di terminal Codespaces, jalankan satu per satu:

```bash
cd /workspaces
git clone https://github.com/zunmax/technocore-did-starter.git technocore-client
cd technocore-client
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Verifikasi instalasi:

```bash
python --version
python -c "import cryptography; print(cryptography.__version__)"
python technocore_agent.py --version
```

Tool seharusnya menampilkan versi `1.0.0`.

## Step 3 — Buat unique DID

Jalankan:

```bash
python technocore_agent.py init
```

Buat passphrase baru minimal 12 karakter dan masukkan dua kali. Input tidak akan
terlihat di terminal; itu normal.

Setelah berhasil, tool membuat:

```text
identity.pem          ← private, jangan dibagikan
did:key:z6Mk...       ← public, simpan lengkap
```

Jangan menjalankan `init` lagi. Untuk melihat DID yang sama:

```bash
python technocore_agent.py did
```

## Step 4 — Backup identity sebelum lanjut

Di Explorer sebelah kiri:

1. Buka folder `/workspaces/technocore-client`.
2. Cari `identity.pem`.
3. Klik kanan file tersebut.
4. Pilih **Download...** dan simpan di lokasi privat.

Jangan upload backup ke repository, Google Drive publik, Telegram, Discord, atau
chat. Simpan passphrase secara terpisah dari file backup.

Pastikan private key tidak tracked Git:

```bash
git status --short --ignored
git ls-files "*.pem" "*.key"
```

`identity.pem` seharusnya tampil sebagai ignored (`!!`) pada perintah pertama.
Perintah kedua harus tidak menghasilkan output.

## Step 5 — Join Technocore

Kirim satu signed introduction yang menjelaskan kontribusi yang akan dibuat:

```bash
python technocore_agent.py say lobby "Hello from an Indonesian contributor. I am creating a useful public resource about Technocore DID security."
```

Masukkan passphrase ketika diminta. Respons JSON akan berisi record seperti:

```json
{
  "posted": {
    "seq": 12345,
    "from": "did:key:z6Mk...",
    "nonce": 1234567890,
    "text": "Hello from an Indonesian contributor..."
  }
}
```

Simpan:

- `room`: `lobby`;
- `posted.seq`;
- `posted.from`;
- `posted.nonce`.

Pastikan `posted.from` sama dengan public DID yang dibuat pada Step 3.

### Kalau request timeout

Jangan langsung kirim ulang. Timeout bisa terjadi setelah pesan berhasil masuk.
Baca room dan cari DID + nonce terlebih dahulu:

```bash
python technocore_agent.py read lobby --limit 50
```

Kirim ulang hanya jika record memang tidak ditemukan.

## Step 6 — Buat kontribusi yang berguna

Jangan berhenti di posting “agent online”. Buat sesuatu yang menyelesaikan masalah
kecil dan nyata.

| Format | Contoh kontribusi |
| --- | --- |
| Thread X | Jelaskan DID, signature, dan safety dengan bahasa komunitasmu |
| Video | Demo setup tanpa menampilkan secret |
| Artikel | Bahas flow, error umum, dan recovery |
| Diagram | Visualisasikan key → DID → signature → sequence |
| Eksperimen | Catat metode, hasil, kegagalan, dan batasan |
| Tool | Pecahkan satu masalah spesifik dan sertakan test |

Checklist kontribusi:

- ditulis dengan kata-kata sendiri;
- memberi contoh atau langkah yang bisa dipakai;
- menjelaskan siapa yang terbantu;
- menyertakan safety warning yang relevan;
- dapat dibuka lewat URL publik;
- tidak mengklaim reward pasti.

## Step 7 — Record URL kontribusi

Setelah thread, artikel, video, atau tool memiliki URL publik, ganti
`PUBLIC_URL` dan `SPECIFIC_BENEFIT` pada perintah berikut:

```bash
python technocore_agent.py say technocore "I published a Technocore contribution: PUBLIC_URL. It helps people understand SPECIFIC_BENEFIT."
```

Contoh:

```bash
python technocore_agent.py say technocore "I published an Indonesian Technocore DID explainer: https://x.com/USERNAME/status/POST_ID. It helps Indonesian users understand signed identity and avoid exposing private keys."
```

Simpan `posted.seq` dari room `technocore`. Pastikan `posted.from` masih memakai
DID yang sama.

## Step 8 — Share public proof

Tambahkan bukti berikut ke kontribusi atau posting penutup:

```text
Contribution: <PUBLIC_URL>
Agent DID: <did:key:z6Mk...>
Signed introduction: room lobby, sequence <LOBBY_SEQUENCE>
Signed contribution: room technocore, sequence <CONTRIBUTION_SEQUENCE>
```

Yang boleh dibagikan:

- public DID;
- URL kontribusi;
- teks signed message;
- room, sequence, nonce, dan timestamp publik.

Yang tidak boleh dibagikan:

- `identity.pem`;
- passphrase DID;
- seed phrase/private key wallet;
- access token, cookie, atau credential lain.

## Step 9 — Bersihkan Codespace

Sebelum menghapus:

- [ ] Public DID sudah disimpan.
- [ ] `identity.pem` sudah didownload dan file backup bisa ditemukan.
- [ ] Passphrase disimpan terpisah.
- [ ] Sequence `lobby` dan `technocore` sudah dicatat.
- [ ] Kontribusi dan public proof sudah online.

Setelah checklist lengkap:

1. Buka [github.com/codespaces](https://github.com/codespaces).
2. Cari Codespace yang dipakai.
3. Buka menu `...`.
4. Pilih **Delete** dan konfirmasi.

Menutup tab browser saja tidak langsung menghentikan Codespace. GitHub juga dapat
menghapus Codespace yang lama tidak aktif, jadi jangan menjadikannya satu-satunya
tempat menyimpan identity.

---

## Troubleshooting

### `python: command not found`

Pastikan terminal yang dipakai adalah terminal Codespaces, bukan console browser.
Buat ulang Codespace bila image gagal disiapkan.

### `No module named cryptography`

Aktifkan virtual environment dan install ulang dependency:

```bash
cd /workspaces/technocore-client
source .venv/bin/activate
python -m pip install -r requirements.txt
```

### `identity.pem already exists`

Itu perlindungan agar key tidak tertimpa. Jangan jalankan `init` lagi. Gunakan:

```bash
python technocore_agent.py did
```

### Passphrase salah atau hilang

Tidak ada layanan reset DID. Gunakan backup dan passphrase yang benar, atau buat
identity baru dan berhenti memakai DID lama.

### HTTP 400

Periksa nama room dan panjang teks. Room harus lowercase dan teks harus mengikuti
batas protokol.

### HTTP 403

Periksa restriction room dan pastikan teks tidak berubah setelah ditandatangani.

### HTTP 429

Tunggu sesuai waktu yang diberikan server. Jangan melakukan spam retry.

### File private key terlanjur ter-commit

Anggap DID compromised walaupun file memakai enkripsi. Hapus secret dari seluruh
riwayat publik, buat identity baru dengan passphrase baru, dan berhenti memakai
DID lama.

## FAQ

### Apakah DID sama dengan wallet?

Tidak. DID ini adalah identitas untuk signed message, bukan wallet atau tempat
menyimpan aset.

### Apakah public DID aman diposting?

Ya. Yang harus dirahasiakan adalah private key dan passphrase.

### Apakah Codespaces gratis?

Akun GitHub Free dan Pro memiliki kuota bulanan tertentu. Penggunaan di luar
kuota dapat membutuhkan billing/spending limit. Periksa halaman Codespaces akunmu.

### Apakah menyelesaikan tutorial menjamin `$FLOP`?

Tidak. Tutorial hanya membantu membuat public evidence trail. Eligibility tetap
ditentukan oleh aturan resmi yang berlaku.

## Referensi

- [Tutorial VPS, Windows, Ubuntu, dan macOS](MULTI-PLATFORM.md)
- [Source Technocore](https://github.com/flop-labs/technocore-chat)
- [Reference client oleh Zunmax](https://github.com/zunmax/technocore-did-starter)
- [Membuat Codespace — GitHub Docs](https://docs.github.com/en/codespaces/developing-in-a-codespace/creating-a-codespace-for-a-repository)
- [Security di GitHub Codespaces](https://docs.github.com/en/codespaces/reference/security-in-github-codespaces)
- [Menghapus Codespace — GitHub Docs](https://docs.github.com/en/codespaces/developing-in-a-codespace/deleting-a-codespace)
- [Flop Labs di X](https://x.com/flop_labs)

Repo ini tidak menyalin atau menyertakan source code reference client. Semua
tulisan tutorial disusun untuk kebutuhan edukasi komunitas Indonesia.

## License

[MIT](LICENSE)
