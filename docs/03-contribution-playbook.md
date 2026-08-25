# Contribution playbook

Tujuannya bukan menghasilkan posting sebanyak mungkin. Buat satu resource yang
mengurangi kebingungan atau membantu orang melakukan sesuatu dengan benar.

## Pilih masalah kecil yang nyata

Contoh:

- pengguna mengira DID sama dengan wallet;
- pemula tidak paham bedanya DID, signature, nonce, dan sequence;
- video tutorial tanpa sengaja menampilkan private key;
- pengguna mengirim ulang write setelah timeout dan membuat duplikat;
- komunitas lokal membutuhkan penjelasan dalam bahasanya sendiri.

## Format kontribusi

| Format | Isi yang berguna |
| --- | --- |
| Thread | Jelaskan satu konsep dengan contoh dan safety note |
| Video | Demo proses tanpa menampilkan secret |
| Artikel | Uraikan flow, kegagalan umum, dan recovery |
| Diagram | Visualisasikan key → DID → signature → sequence |
| Eksperimen | Dokumentasikan metode, hasil, error, dan batasan |
| Tool | Pecahkan satu masalah spesifik dan sertakan test |

## Record kontribusi

Setelah kontribusi memiliki URL publik:

1. buat signed message di room `technocore`;
2. masukkan URL dan manfaat spesifik kontribusi;
3. pastikan DID response sama dengan DID introduction;
4. simpan sequence record;
5. tambahkan bukti tersebut ke posting publik.

Wording yang jelas lebih berguna daripada klaim besar. Contoh:

```text
I published an Indonesian explainer: PUBLIC_URL. It helps new users distinguish
a public DID from private key material and avoid exposing secrets.
```
