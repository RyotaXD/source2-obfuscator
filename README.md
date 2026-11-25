# source2-obfuscator

Obfuscator Python buatan **Bayu Putra Tama / RyotaXD**. Satu file, tanpa library tambahan, jalan di Termux & Linux.

> File `source2.py` di repo ini adalah versi **release yang sudah ter-obfuscate** (dilindungi pakai tool ini sendiri). Tinggal download dan pakai — source aslinya tidak dibagikan.

## Dua Mode

### 1. Mode Sumber (default)
Hasilnya tetap file `.py` yang terbuka: bisa dibaca, diedit, dan di-import seperti biasa — tapi isinya ruwet.

- Nama variabel lokal & parameter diacak (sadar scope & closure)
- String disamarkan (`bytes` / base64)
- Angka dipecah jadi operasi hitung
- Diselipkan kode sampah yang tidak berpengaruh
- Nama class & fungsi publik **dipertahankan**, jadi file hasil tetap bisa di-import

```bash
python3 source2.py bot.py
# keluar: bot_obf.py
```

### 2. Mode Penuh
Hasil mode sumber dilanjutkan sampai terkunci total:

- Watermark kepemilikan (nama & sosmed) nempel di dalam payload
- **Anti-pycdc**: payload disuntik code object palsu yang bikin decompiler (pycdc dkk) error di tengah jalan
- Compile → marshal → kompres (zlib/lzma/bz2, acak) → byte dibalik → XOR keystream SHA-256 terbalik → putaran enkripsi kedua → segel HMAC
- Payload **dipecah 2–4 bagian** dan disebar acak di seluruh file, diselipi blob umpan (payload palsu jebakan)
- Dibungkus stub loader yang tampilannya kayak program biasa (header + konfigurasi palsu)
- **Anti-tamper**: kunci payload diikat ke sidik jari AST semua fungsi di file. File dipatch di titik mana pun → kunci salah → payload jadi sampah → file menolak jalan
- Kunci nempel di file (default): pembeli tinggal jalanin, tanpa ribet kunci

```bash
python3 source2.py bot.py hasil.py --mode=penuh
python3 hasil.py   # langsung jalan
```

## Opsi

| Flag | Fungsi |
|---|---|
| `--mode=sumber\|penuh` | Pilih mode (default: sumber) |
| `--seed=N` | Hasil bisa diulang persis. Tanpa seed, tiap build beda-beda (polimorfik) |
| `--acak-fungsi` | Mode sumber: nama fungsi publik ikut diacak (file hasil tidak bisa di-import pakai nama lama) |
| `--tanpa-sampah` | Mode sumber: tanpa kode sampah, hasil lebih ramping |
| `--lapis=1-10` | Mode penuh: stub dibungkus bertingkat. 2–3 sudah cukup |
| `--kunci=XXX` | Mode penuh: kunci TIDAK nempel di file; dibaca dari env `RYXD_KEY` saat program jalan |
| `--anti-debug` | Mode penuh: file menolak jalan kalau ada debugger nempel (default mati) |

Contoh kunci terpisah:

```bash
python3 source2.py bot.py hasil.py --mode=penuh --kunci=RAHASIA
RYXD_KEY=RAHASIA python3 hasil.py
```

## Syarat

- Python **3.9+**
- Tanpa dependensi apa pun

## Catatan Penting

- File hasil mode penuh **jangan dibuka lalu di-save ulang** — sidik jarinya berubah dan file akan menolak jalan. Itu memang anti-tamper-nya bekerja.
- File hasil mode penuh terkunci ke **versi Python yang sama** dengan versi saat build (marshal). Build di Python 3.12 → jalankan di 3.12 juga.
- Obfuscation menaikkan biaya bongkar, bukan membuatnya mustahil. Untuk kode yang dijual, kombinasikan dengan lisensi per pembeli & watermark.

## Kredit

- Author: **Bayu Putra Tama / RyotaXD**
- GitHub: [github.com/RyotaXD](https://github.com/RyotaXD)
- Facebook: Bayu Putra Tama
- WhatsApp: +62 838-6274-3192

Dipakai untuk melindungi code sendiri. Gunakan dengan bijak.
