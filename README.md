# SJK-ETABSRebarPro

Aplikasi web desain tulangan balok beton bertulang berbasis output ETABS, dikembangkan oleh **PT Struclab Jaya Konsultindo (SJK)**.

---

## 🚀 Cara Deploy ke GitHub Pages

### 1. Buat Repository
- Buka [github.com](https://github.com) → **New repository**
- Nama repo: `sjk-etabs-rebar` (atau sesuai keinginan)
- Visibility: **Private** (disarankan)
- Klik **Create repository**

### 2. Upload File
- Klik **Add file → Upload files**
- Upload `index.html` dan `README.md`
- Klik **Commit changes**

### 3. Aktifkan GitHub Pages
- **Settings → Pages**
- Source: **Deploy from a branch**
- Branch: `main` → folder `/ (root)` → **Save**

### 4. Akses Aplikasi
```
https://<username>.github.io/<nama-repo>/
```

---

## 📋 Alur Kerja Aplikasi

| Langkah | Tab | Fungsi |
|---------|-----|--------|
| 1 | 🔐 Login | Autentikasi role + OTP Engineer + GPS kantor |
| 2 | 📁 Project Setup | Input info project + upload file `.e2k` / `.et` dari komputer |
| 3 | ⚙️ Parameter | Konfigurasi fc', fy, cover, diameter, sengkang, story grouping |
| 4 | 🔩 Longitudinal | Upload Flexural Envelope Excel → Engine M2+M3 → hasil tulangan |
| 5 | ⬡ Shear | Upload Shear Envelope Excel → Engine M4 → hasil sengkang |
| 6 | 📦 Output | Download E2K, laporan, dan simpan project |
| — | 📖 Panduan SNI | Referensi pasal SNI 2847:2019, 1726:2019, 1727:2020 |

---

## ⚙️ Engine Kalkulasi (SNI 2847:2019)

### M2 — Grouping Section
- Klasifikasi otomatis: **G** (induk), **B** (anak), **KG** (kantilever induk), **KB** (kantilever anak)
- Section BB dan KB (tangga) dikecualikan otomatis
- Urutan dimensi: terkecil → terbesar

### M3 — Tulangan Longitudinal
- Diameter tersedia: D10, D13, D16, D19, D22, D25 (konfigurasi user)
- Maks 2 diameter berbeda per penampang
- Cek **As min** (SNI 9.6.1.2), **As maks** (SNI 9.3.3.1)
- Cek **SRPMK**: As tumpuan ≥ As lapangan/4 (SNI 18.6.3.1)
- Cek jarak bersih antar tulangan (SNI 25.2.1)

### M4 — Sengkang
- **Tumpuan**: Vc = 0 (zona gempa SRPMK) → pakai VRebar Av/s dari ETABS langsung
- **Lapangan**: Vs = Vu/φ − Vc (engine hitung sendiri, φ = 0.75)
- Vc = 0.17 · λ · √fc · bw · d
- Spasi tumpuan: fix sesuai konfigurasi (default 100mm)
- Spasi lapangan: dibulatkan ke kelipatan konfigurasi, maks = d/2

---

## 📂 Format File Input

### File E2K / $ET (ETABS)
Upload langsung file text model ETABS. Parser membaca:
- Material properties (fc', fy)
- Frame section (nama, b, h)
- Story list

### Flexural Envelope Excel
Export dari ETABS: **Design → Concrete Frame Design → Tabulate → Concrete Beam Flexure Envelope**

Kolom yang dibaca:
```
Story | Label | UniqueName | Section | Location | As Top (mm²) | As Bot (mm²)
```

### Shear Envelope Excel
Export dari ETABS: **Design → Concrete Frame Design → Tabulate → Concrete Beam Shear Envelope**

Kolom yang dibaca:
```
Story | Label | UniqueName | Section | Location | Shear Force (kN) | VRebar Av/s (mm²/m)
```

---

## 📦 File Output

| File | Isi |
|------|-----|
| `SJK_Penulangan_Balok.txt` | Tabel lengkap tulangan longitudinal + sengkang semua group |
| `SJK_Laporan_Ringkas.txt` | Ringkasan desain, FLAG PASS/FAIL per group |
| `SJK_E2K_v1.e2k` | File E2K update tulangan longitudinal (untuk re-run ETABS) |
| `SJK_E2K_Final.e2k` | File E2K final tulangan longitudinal + sengkang |

---

## 🔐 Sistem Login

| Role | Metode Akses | Lokasi |
|------|-------------|--------|
| Owner | Password | Mana saja |
| Admin | Password | Mana saja |
| Senior Engineer | OTP via Admin + GPS | Kantor SJK |
| Junior Engineer | OTP via Admin + GPS | Kantor SJK |

**Koordinat GPS Kantor SJK:**
> Rukan Paris Golf Lake Residence Blok A No.81, RT.9/RW.14, Cengkareng Tim., Kec. Cengkareng, Jakarta Barat 11730
> Koordinat: -6.132316, 106.7288165 | Radius: 300m

---

## 🛠️ Teknologi

| Komponen | Teknologi |
|----------|-----------|
| Frontend | HTML5 + CSS3 + Vanilla JavaScript |
| Excel Parser | SheetJS (XLSX.js) via CDN |
| Deploy | GitHub Pages |
| Dependencies | Tidak ada (single file) |

---

## 📁 Struktur File

```
sjk-etabs-rebar/
├── index.html    ← seluruh aplikasi (UI + engine kalkulasi)
└── README.md
```

---

## 📌 Catatan Teknis

- Aplikasi berjalan **100% di browser** — tidak ada server backend
- Data tidak dikirim ke server manapun — semua proses lokal di browser user
- File E2K output berformat text, siap di-import langsung ke ETABS
- Engine kalkulasi mengacu **SNI 2847:2019** dan **SNI 1726:2019**

---

*© PT Struclab Jaya Konsultindo — Internal Use Only*
*SJK-ETABSRebarPro v2.0 — Full Engine Edition*
