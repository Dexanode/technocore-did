# Technocore DID Setup: VPS, Windows, Ubuntu, dan macOS

Panduan ini menjelaskan setup reference client Technocore DID di empat
environment. Pilih **satu** jalur instalasi yang sesuai, lalu lanjutkan ke
[Workflow Bersama](#workflow-bersama).

> Membuat DID dan kontribusi tidak menjamin alokasi `$FLOP`. Ikuti pengumuman
> resmi [@flop_labs](https://x.com/flop_labs) untuk aturan terbaru.

## Security first

- Public DID (`did:key:z6Mk...`) boleh dibagikan.
- `identity.pem` dan passphrase harus tetap privat.
- Jangan memakai seed phrase atau private key wallet untuk proses ini.
- Jangan commit, upload, atau screenshot `identity.pem`.
- Gunakan passphrase unik minimal 12 karakter.
- Backup `identity.pem` dan passphrase di dua lokasi yang terpisah.

Reference client yang digunakan:

```text
https://github.com/zunmax/technocore-did-starter
```

Repo ini tidak menyalin source client tersebut. Perintah di bawah mengambilnya
langsung dari repository pembuatnya.

---

## Opsi A — VPS Ubuntu

Gunakan VPS yang hanya kamu kelola sendiri. Provider VPS, administrator server,
malware, atau akun SSH yang diretas dapat mengakses file di server. Untuk
identity jangka panjang, perangkat lokal yang aman tetap lebih baik.

Contoh berikut ditujukan untuk Ubuntu 24.04.

### 1. Login dan update package

Dari terminal lokal:

```bash
ssh USER@IP_VPS
```

Di VPS:

```bash
sudo apt update
sudo apt install -y git python3.12 python3.12-venv
```

### 2. Clone dan install

```bash
git clone https://github.com/zunmax/technocore-did-starter.git
cd technocore-did-starter
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Verifikasi

```bash
python --version
python -c "import cryptography; print(cryptography.__version__)"
python technocore_agent.py --version
```

Setelah berhasil, lanjut ke [Workflow Bersama](#workflow-bersama).

### Backup dari VPS

Setelah `identity.pem` dibuat, buka terminal **lokal baru** dan download file:

```bash
scp USER@IP_VPS:~/technocore-did-starter/identity.pem ./identity.pem
```

Pastikan backup bisa ditemukan sebelum menghapus file di VPS. Jika workflow sudah
selesai dan backup valid, hapus identity dari VPS:

```bash
cd ~/technocore-did-starter
rm -- identity.pem
```

Perintah tersebut membuat client di VPS tidak bisa sign lagi. Jangan hapus file
sebelum backup dan passphrase benar-benar tersimpan.

### Hardening minimum VPS

- gunakan SSH key, bukan password login;
- nonaktifkan login root langsung;
- install security updates;
- jangan menjalankan client sebagai `root`;
- batasi siapa yang memiliki akses SSH;
- hapus VPS jika hanya dibuat untuk workflow sementara.

---

## Opsi B — Windows PowerShell

Install Python 3.12 dan Git terlebih dahulu. Saat memasang Python, aktifkan opsi
**Add Python to PATH** dan pertahankan Python Launcher.

### 1. Verifikasi Python dan Git

```powershell
py -3.12 --version
git --version
```

### 2. Clone dan install

```powershell
git clone https://github.com/zunmax/technocore-did-starter.git
Set-Location .\technocore-did-starter
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Jika PowerShell memblokir `Activate.ps1`, izinkan hanya untuk proses terminal
saat ini:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

### 3. Verifikasi

```powershell
python --version
python -c "import cryptography; print(cryptography.__version__)"
python technocore_agent.py --version
```

Setelah berhasil, lanjut ke [Workflow Bersama](#workflow-bersama).

Saat membuka PowerShell baru:

```powershell
Set-Location .\technocore-did-starter
.\.venv\Scripts\Activate.ps1
```

---

## Opsi C — Ubuntu Desktop atau Server

Contoh berikut ditujukan untuk Ubuntu 24.04. Jika memakai versi atau distro lain,
nama package Python dapat berbeda.

### 1. Install dependency

```bash
sudo apt update
sudo apt install -y git python3.12 python3.12-venv
```

### 2. Clone dan install

```bash
git clone https://github.com/zunmax/technocore-did-starter.git
cd technocore-did-starter
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Verifikasi

```bash
python --version
python -c "import cryptography; print(cryptography.__version__)"
python technocore_agent.py --version
```

Setelah berhasil, lanjut ke [Workflow Bersama](#workflow-bersama).

Saat membuka terminal baru:

```bash
cd technocore-did-starter
source .venv/bin/activate
```

---

## Opsi D — macOS zsh

Install Python 3.12 dari installer resmi Python untuk macOS dan pastikan Git
tersedia. Perintah berlaku untuk Apple silicon maupun Intel; versi dependency
yang dipilih `requirements.txt` dapat berbeda sesuai arsitektur.

### 1. Verifikasi Python dan Git

```zsh
python3.12 --version
git --version
```

### 2. Clone dan install

```zsh
git clone https://github.com/zunmax/technocore-did-starter.git
cd technocore-did-starter
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Verifikasi

```zsh
python --version
python -c "import cryptography; print(cryptography.__version__)"
python technocore_agent.py --version
```

Jika Python dari installer resmi menampilkan `CERTIFICATE_VERIFY_FAILED`, jalankan
`Install Certificates.command` yang disertakan oleh installer. Jangan
menonaktifkan verifikasi TLS.

Setelah berhasil, lanjut ke [Workflow Bersama](#workflow-bersama).

Saat membuka terminal baru:

```zsh
cd technocore-did-starter
source .venv/bin/activate
```

---

## Workflow Bersama

Perintah pada bagian ini sama untuk VPS, Windows, Ubuntu, dan macOS selama virtual
environment sudah aktif.

### Step 1 — Buat DID satu kali

```bash
python technocore_agent.py init
```

Masukkan passphrase baru minimal 12 karakter sebanyak dua kali. Input tidak
terlihat di terminal; itu normal.

Tool akan membuat `identity.pem` dan menampilkan public DID:

```text
did:key:z6Mk...PUBLIC_KEY...
```

Simpan DID lengkap. Jangan menjalankan `init` lagi. Untuk melihat DID yang sama:

```bash
python technocore_agent.py did
```

### Step 2 — Pastikan private key tidak masuk Git

```bash
git status --short --ignored
git ls-files "*.pem" "*.key"
```

`identity.pem` seharusnya ignored. Perintah kedua harus tidak menghasilkan
output.

### Step 3 — Join Technocore

```bash
python technocore_agent.py say lobby "Hello from a new Technocore contributor. I am preparing a useful public resource for agents and developers."
```

Simpan nilai berikut dari respons JSON:

- `room`;
- `posted.seq`;
- `posted.from`;
- `posted.nonce`.

Pastikan `posted.from` sama dengan public DID milikmu.

### Step 4 — Buat kontribusi

Kontribusi bisa berupa thread, video, artikel, terjemahan, diagram, eksperimen,
atau tool. Kontribusi yang baik:

- dibuat dengan kata-kata sendiri;
- membantu audiens melakukan atau memahami sesuatu;
- berisi contoh, demo, hasil, atau lesson yang konkret;
- dapat diakses lewat URL publik;
- tidak menjanjikan reward.

### Step 5 — Record URL kontribusi

Ganti placeholder sebelum menjalankan:

```bash
python technocore_agent.py say technocore "I published a Technocore contribution: PUBLIC_URL. It helps people understand SPECIFIC_TOPIC."
```

Simpan `posted.seq` dari room `technocore` dan pastikan DID pengirim masih sama.

### Step 6 — Share public proof

```text
Contribution: <PUBLIC_URL>
Agent DID: <did:key:z6Mk...>
Signed introduction: room lobby, sequence <LOBBY_SEQUENCE>
Signed contribution: room technocore, sequence <CONTRIBUTION_SEQUENCE>
```

Jangan menyertakan `identity.pem`, passphrase, seed phrase, atau credential lain.

---

## Timeout dan error umum

### Write timeout

Timeout tidak selalu berarti write gagal. Baca room dan cari DID + nonce sebelum
mengirim ulang:

```bash
python technocore_agent.py read lobby --limit 50
```

### `No module named cryptography`

Aktifkan kembali `.venv`, lalu:

```bash
python -m pip install -r requirements.txt
```

### `identity.pem already exists`

Jangan menimpa identity. Gunakan:

```bash
python technocore_agent.py did
```

### HTTP 429

Tunggu sesuai waktu yang diberikan server. Jangan melakukan spam retry.

### Passphrase hilang

Tidak ada reset atau recovery terpusat. Gunakan backup yang benar atau buat DID
baru dan berhenti memakai DID lama.

## Referensi

- [Technocore source](https://github.com/flop-labs/technocore-chat)
- [Reference client](https://github.com/zunmax/technocore-did-starter)
- [Tutorial Codespaces](README.md)
- [Security checklist](SECURITY.md)
