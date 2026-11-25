<div align="center">

# 🛡️ source2-obfuscator

**Obfuscator Python 2 mode — ringan, tanpa dependensi, siap pakai di Termux & Linux.**

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Termux%20%7C%20Linux-32a852)
![Mode](https://img.shields.io/badge/Mode-Sumber%20%26%20Penuh-8a2be2)
![Dependency](https://img.shields.io/badge/Dependency-0-brightgreen)

</div>

---

## ✨ Tentang

`source2.py` adalah tool untuk melindungi script Python kamu dari recode dan bongkar pasang. Tersedia dua mode: hasil yang tetap terbuka sebagai source `.py`, atau hasil yang terkunci total dengan enkripsi berlapis.

> 📦 File `source2.py` di repo ini adalah **versi release yang sudah ter-obfuscate** memakai tool ini sendiri. Download, jalankan, selesai — source aslinya tidak dibagikan.

---

## 🚀 Mulai Cepat

```bash
# 1. Download
curl -L https://raw.githubusercontent.com/RyotaXD/source2-obfuscator/main/source2.py -o source2.py

# 2. Obfuscate script kamu
python3 source2.py bot.py hasil.py --mode=penuh

# 3. Jalankan hasilnya — langsung jalan, tanpa kunci ribet
python3 hasil.py
```

---

## 🎛️ Dua Mode

### 1️⃣ Mode Sumber — hasil tetap file `.py` terbuka

Cocok kalau hasil obfuscate masih perlu dibaca, diedit, atau di-import orang lain.

- 🔀 Nama variabel lokal & parameter diacak (sadar scope & closure)
- 🔤 String disamarkan jadi `bytes` / base64
- 🔢 Angka dipecah jadi operasi hitung
- 🗑️ Diselipkan kode sampah yang tidak berpengaruh
- ✅ Nama class & fungsi publik **dipertahankan** — file hasil tetap bisa di-import

```bash
python3 source2.py bot.py
# keluar: bot_obf.py
```

### 2️⃣ Mode Penuh — hasil terkunci total

Cocok untuk script yang dijual atau dibagikan ke publik.

- 💧 Watermark kepemilikan nempel di dalam payload
- 🧬 **Anti-pycdc** — payload disuntik code object palsu yang membuat decompiler error di tengah jalan
- 🔐 Rantai perlindungan: compile → marshal → kompres (zlib/lzma/bz2 acak) → byte dibalik → XOR keystream SHA-256 terbalik → putaran enkripsi kedua → segel HMAC
- 🧩 Payload **dipecah 2–4 bagian**, disebar acak ke seluruh file, diselipi blob umpan jebakan
- 🎭 Stub loader bertampilan program biasa (header + konfigurasi palsu)
- 🛡️ **Anti-tamper** — kunci payload diikat ke sidik jari AST semua fungsi di file; dipatch di titik mana pun, kunci langsung salah dan file menolak jalan
- 🔑 Kunci nempel di file secara default — pengguna akhir tinggal menjalankan

```bash
python3 source2.py bot.py hasil.py --mode=penuh
```

---

## ⚙️ Opsi Lengkap

| Flag | Keterangan |
|:-----|:-----------|
| `--mode=sumber\|penuh` | Pilih mode (default: `sumber`) |
| `--seed=N` | Hasil bisa diulang persis. Tanpa seed, setiap build berbeda (polimorfik) |
| `--acak-fungsi` | Mode sumber: nama fungsi publik ikut diacak *(file hasil tidak bisa di-import pakai nama lama)* |
| `--tanpa-sampah` | Mode sumber: tanpa kode sampah, hasil lebih ramping |
| `--lapis=1-10` | Mode penuh: stub dibungkus bertingkat *(2–3 sudah cukup)* |
| `--kunci=XXX` | Mode penuh: kunci tidak nempel di file, dibaca dari env `RYXD_KEY` saat program jalan |
| `--anti-debug` | Mode penuh: file menolak jalan jika ada debugger nempel *(default mati)* |

Contoh pemakaian kunci terpisah:

```bash
python3 source2.py bot.py hasil.py --mode=penuh --kunci=RAHASIA
RYXD_KEY=RAHASIA python3 hasil.py
```

---

## 📋 Syarat

- Python **3.9+**
- **Tanpa dependensi** — murni library bawaan Python

---

> [!WARNING]
> **Catatan penting**
> - File hasil mode penuh **jangan dibuka lalu disimpan ulang** — sidik jarinya berubah dan file akan menolak jalan. Itu memang anti-tamper-nya yang bekerja.
> - File hasil mode penuh terkunci ke **versi Python yang sama** dengan saat build (marshal). Build di Python 3.12 → jalankan di 3.12 juga.
> - Obfuscation menaikkan biaya bongkar, bukan membuatnya mustahil. Untuk kode yang dijual, kombinasikan dengan lisensi per pembeli dan watermark.

---

## 👤 Author & Kontak

**RyotaXD**

- 🐙 GitHub — [github.com/RyotaXD](https://github.com/RyotaXD)
- 💬 WhatsApp — [+62 838-6274-3192](https://wa.me/6283862743192)
- 📘 Facebook — [Dunia Cyber Security](https://www.facebook.com/people/Dunia-Cyber-Security/61596957327073/)

<div align="center">

*Dibuat untuk melindungi code sendiri. Gunakan dengan bijak.* ⭐

</div>
