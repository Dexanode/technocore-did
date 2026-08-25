# Alur setup yang aman

Panduan ini tool-agnostic. Pilih client yang source-nya dapat diperiksa dan
mendukung Ed25519 `did:key` serta format signed write Technocore.

## 1. Audit sebelum menjalankan

Periksa minimal hal berikut:

- dependency dipin ke versi tertentu;
- private key dibuat dan dienkripsi secara lokal;
- client tidak mengirim private key atau passphrase;
- endpoint network mengarah ke domain yang diharapkan;
- tool menolak menimpa identity yang sudah ada.

## 2. Buat identity baru

Gunakan passphrase unik yang tidak dipakai untuk akun, wallet, atau layanan lain.
Input passphrase seharusnya tidak tampil di terminal.

Catat public DID lengkap. Backup private key terenkripsi dan passphrase secara
terpisah. Jangan menyimpan keduanya di chat, cloud note publik, atau repository.

## 3. Kirim signed introduction

Gunakan room `lobby` dan tulis pesan yang menjelaskan kontribusi yang akan dibuat.
Setelah server menerima pesan, simpan:

- DID pengirim;
- room;
- sequence;
- nonce;
- timestamp.

Pastikan DID pada response sama dengan DID lokal.

## 4. Tangani timeout dengan hati-hati

Timeout tidak selalu berarti write gagal. Sebelum mengirim ulang, baca room dan
cari kombinasi DID + nonce. Mengirim ulang tanpa pemeriksaan dapat menghasilkan
record ganda.

## 5. Pre-publish check

Jika memakai Git:

```powershell
git status --short
git diff --cached
git ls-files "*.pem" "*.key"
```

Perintah terakhir harus kosong. Periksa juga screenshot, video, shell history,
dan file backup yang mungkin tidak memiliki ekstensi `.pem` atau `.key`.
