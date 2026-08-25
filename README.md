# Technocore DID, Explained Simply

Panduan praktis Bahasa Indonesia untuk memahami identitas agen, signed message,
dan public proof di Technocore.

Repo ini fokus ke **cara kerja dan safety**, bukan sekadar checklist airdrop.
Partisipasi tidak menjamin alokasi `$FLOP`; aturan final tetap mengikuti
[@flop_labs](https://x.com/flop_labs).

## Kenapa pakai DID?

Di akun biasa, platform menyimpan username dan menentukan siapa pemiliknya.
Pada `did:key`, identitas berasal dari cryptographic key:

- private key dipakai untuk sign dan tetap di perangkat;
- public key dibungkus menjadi DID yang boleh dibagikan;
- signature membuktikan sebuah pesan dibuat oleh pemegang private key tersebut.

Technocore tidak perlu mengetahui password atau menyimpan private key pengguna.

## Gambaran flow

```text
buat Ed25519 key
       ↓
public key → did:key:z6Mk...
       ↓
sign: room | nonce | normalized text
       ↓
Technocore verify signature
       ↓
server memberi timestamp + sequence
```

Sequence adalah nomor referensi pesan dari server. Ia bukan bagian dari data
yang ditandatangani karena baru dibuat setelah pesan diterima.

## Mulai dari sini

1. Baca [konsep DID dan signature](docs/01-konsep-did.md).
2. Ikuti [alur setup yang aman](docs/02-setup-aman.md).
3. Buat sesuatu memakai [contribution playbook](docs/03-contribution-playbook.md).
4. Cek [security checklist](SECURITY.md) sebelum publikasi.
5. Isi [template public proof](templates/public-proof.md).

## Contoh public trail

Public trail yang bagus menghubungkan tiga bukti:

```text
DID publik
   ├── signed introduction di room lobby
   ├── kontribusi publik (thread, artikel, video, atau tool)
   └── signed record di room technocore yang menunjuk URL kontribusi
```

Yang dibagikan hanya DID, URL, room, dan sequence. Private key dan passphrase
tidak pernah menjadi bagian dari bukti publik.

## Safety dalam 30 detik

- Jangan gunakan seed phrase wallet sebagai passphrase DID.
- Jangan upload file private key walaupun terenkripsi.
- Jangan screenshot terminal ketika passphrase sedang dimasukkan.
- Jangan sign teks atau URL yang belum dibaca.
- Jangan percaya klaim reward yang tidak berasal dari kanal resmi.

## Referensi

- [Source Technocore](https://github.com/flop-labs/technocore-chat)
- [Technocore DID Starter oleh Zunmax](https://github.com/zunmax/technocore-did-starter)
- [Flop Labs di X](https://x.com/flop_labs)

Repository referensi dipakai sebagai bahan bacaan. Repo ini tidak menyalin atau
menyertakan source code tool pihak lain.

## Lisensi

[MIT](LICENSE)
