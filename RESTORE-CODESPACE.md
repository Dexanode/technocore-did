# Restore DID di Codespace Baru

Panduan ini dipakai jika Codespace lama sudah benar-benar terhapus dan kamu ingin
melanjutkan memakai DID yang sama di Codespace baru.

> Menutup tab browser tidak langsung menghapus Codespace. Codespace dapat tetap
> berjalan sampai idle timeout, lalu berhenti. Secara default, stopped Codespace
> disimpan hingga 30 hari. Namun,
> retention akun bisa diatur lebih pendek—bahkan `0`, yang membuat Codespace
> langsung dihapus ketika berhenti atau timeout.

## Yang wajib dimiliki

Sebelum mulai, pastikan kamu punya:

- backup `identity.pem` dari Codespace lama;
- passphrase yang dipakai saat membuat identity;
- public DID lama untuk verifikasi.

Tanpa `identity.pem` **dan** passphrase yang benar, DID lama tidak bisa dipulihkan.
Technocore tidak memiliki fitur reset password atau recovery terpusat.

## Step 1 — Pastikan Codespace lama memang hilang

1. Buka [github.com/codespaces](https://github.com/codespaces).
2. Cari Codespace untuk repository `Dexanode/technocore-did`.
3. Jika masih ada, tekan namanya untuk membuka kembali.
4. Jika tidak ada, lanjutkan membuat Codespace baru.

## Step 2 — Buat Codespace baru

1. Buka [Dexanode/technocore-did](https://github.com/Dexanode/technocore-did).
2. Tekan **Code**.
3. Pilih tab **Codespaces**.
4. Tekan **Create codespace on main**.
5. Tunggu editor dan terminal selesai dimuat.

Jika memakai HP, mode desktop browser biasanya lebih nyaman.

## Step 3 — Install ulang reference client

Di terminal Codespaces:

```bash
cd /workspaces
git clone https://github.com/zunmax/technocore-did-starter.git technocore-client
cd technocore-client
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python technocore_agent.py --version
```

Jika folder `technocore-client` sudah ada, jangan clone ulang. Masuk dan aktifkan
environment:

```bash
cd /workspaces/technocore-client
source .venv/bin/activate
```

## Step 4 — Upload backup `identity.pem`

Di Explorer sebelah kiri:

1. Buka folder `/workspaces/technocore-client`.
2. Klik kanan folder tersebut.
3. Pilih **Upload...**.
4. Pilih backup `identity.pem`.
5. Pastikan nama akhirnya tepat `identity.pem`, bukan `identity.pem.txt`.

Alternatif: drag-and-drop file ke folder `technocore-client` jika browser dan
perangkat mendukungnya.

Verifikasi file berada di tempat yang benar:

```bash
cd /workspaces/technocore-client
ls -l identity.pem
git status --short --ignored
git ls-files "*.pem" "*.key"
```

Hasil yang benar:

- `ls` menemukan `identity.pem`;
- `git status --short --ignored` menampilkan `!! identity.pem`;
- `git ls-files` tidak menghasilkan output.

## Step 5 — Verifikasi DID lama

Jangan menjalankan `init`. Jalankan:

```bash
source .venv/bin/activate
python technocore_agent.py did
```

Masukkan passphrase lama. Bandingkan output dengan public DID yang sudah disimpan.
DID harus sama persis, termasuk seluruh karakter setelah `did:key:`.

Jika DID berbeda, jangan sign pesan apa pun. Kemungkinan backup yang di-upload
berasal dari identity lain.

## Step 6 — Lanjutkan memakai DID yang sama

Setelah DID cocok, client siap digunakan kembali:

```bash
python technocore_agent.py say lobby "DID restored in a new Codespace and ready to continue contributing."
```

Untuk mencatat kontribusi baru:

```bash
python technocore_agent.py say technocore "I published a Technocore contribution: PUBLIC_URL. It helps people understand SPECIFIC_TOPIC."
```

Kamu tidak perlu membuat signed introduction baru hanya untuk memulihkan DID.
Perintah `say lobby` di atas bersifat opsional; gunakan bila memang ingin membuat
update publik.

## Step 7 — Backup dan cleanup lagi

Codespace baru juga dapat terhapus. Sebelum menutup pekerjaan:

- pastikan backup `identity.pem` masih tersedia;
- pastikan passphrase tersimpan terpisah;
- jangan commit file identity;
- catat sequence penting;
- hapus Codespace jika sudah tidak diperlukan.

## Mengatur retention Codespaces

GitHub mengizinkan retention stopped Codespaces antara `0` dan 30 hari.

1. Buka **GitHub Settings**.
2. Pilih **Codespaces**.
3. Cari **Default retention period**.
4. Pilih jumlah hari yang sesuai.

Nilai `0` berarti Codespace dapat langsung terhapus saat dihentikan atau timeout.
Retention bukan backup: automatic deletion tetap dapat menghapus unpushed files.

## Troubleshooting

### `identity.pem` tidak ditemukan

Cari lokasi client:

```bash
find /workspaces -maxdepth 3 -name technocore_agent.py -print
```

Upload `identity.pem` ke folder yang sama dengan `technocore_agent.py`.

### `incorrect passphrase`

Pastikan memakai passphrase identity yang benar, bukan password GitHub atau
wallet. Tidak ada cara bypass encryption.

### `refusing to overwrite existing identity`

Itu berarti kamu menjalankan `init`. Stop dan gunakan command `did`.

### DID yang muncul berbeda

File backup berasal dari identity lain. Jangan lanjut sign sebelum menemukan
backup yang sesuai.

## Referensi resmi GitHub

- [Automatic deletion dan retention Codespaces](https://docs.github.com/en/codespaces/setting-your-user-preferences/configuring-automatic-deletion-of-your-codespaces)
- [Deleting a Codespace](https://docs.github.com/en/codespaces/developing-in-a-codespace/deleting-a-codespace)
