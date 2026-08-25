# Memahami DID tanpa jargon berlebihan

## DID bukan wallet

DID adalah identifier publik yang terhubung ke cryptographic key. Format
`did:key:z6Mk...` yang dipakai di sini menunjukkan public key Ed25519 secara
self-contained. Tidak ada saldo, token, atau seed phrase wallet di dalamnya.

## Apa yang sebenarnya ditandatangani?

Untuk signed write, payload dibangun dari:

```text
room|nonce|normalized-text
```

Konsekuensinya:

- mengganti room membuat signature gagal;
- mengganti nonce membuat signature gagal;
- mengganti satu karakter teks membuat signature gagal.

Normalisasi teks harus dilakukan sebelum signing agar client dan server
memverifikasi byte yang sama.

## Fungsi nonce

Nonce membantu membedakan setiap write dan mengurangi risiko replay. Nilainya
harus mengikuti aturan server dan meningkat sesuai implementasi client yang
digunakan. Jangan mengedit nonce setelah signature dibuat.

## Fungsi sequence

Sequence ditentukan oleh server setelah write diterima. Gunakan sequence untuk
menunjuk record tertentu di sebuah room. Sequence membantu navigasi dan evidence
trail, tetapi bukan bukti kriptografi itu sendiri.

## Batas klaim yang masuk akal

Signature membuktikan bahwa pemegang private key menandatangani payload. Ia tidak
otomatis membuktikan nama asli, reputasi, kepemilikan akun X, atau kualitas
kontribusi. Hubungan tersebut membutuhkan bukti tambahan yang transparan.
