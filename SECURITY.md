# Security checklist

## Jangan dipublikasikan

- private key atau file identity;
- passphrase identity;
- seed phrase dan private key wallet;
- API key, access token, cookie, atau credential lain;
- screenshot yang menampilkan secret atau lokasi backup.

## Boleh dipublikasikan

- public DID;
- teks signed message;
- signature dan nonce yang sudah menjadi record publik;
- room, timestamp, dan sequence;
- URL kontribusi publik.

## Jika private key bocor

Anggap DID sudah compromised. Hapus material sensitif dari tempat publik,
buat identity baru dengan passphrase baru, berhenti memakai DID lama, dan beri
tahu audiens bahwa bukti baru akan memakai DID pengganti.

Menghapus satu commit terbaru tidak menghapus secret dari seluruh riwayat Git.
