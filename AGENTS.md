# Aturan Global silmi-ai

Berlaku untuk semua karyawan (folder di dalam `silmi-ai/`). `AGENTS.md` milik karyawan boleh lebih spesifik, tetapi tidak boleh bertentangan dengan file ini.

## Konteks bisnis

> Isi sekali, dipakai semua karyawan.

- **Nama bisnis:** Silmi
- **Produk utama:** (isi)
- **Kanal penjualan:** (isi, mis. toko online, marketplace)
- **Jam kerja:** (isi, mis. Senin-Sabtu 08:00-17:00)
- **Zona waktu:** Asia/Jakarta (WIB)
- **Bahasa:** Indonesia

### Orang kunci


| Nama   | Peran           |
| ------ | --------------- |
| Lukman | Pemilik         |
| Supini | Kepala produksi |

## Struktur workspace

```
silmi-ai/
├── AGENTS.md                  # File ini (aturan global)
└── production-control/
    ├── AGENTS.md              # Peran, jadwal, format laporan
    └── references/
        └── mcp-tools.md       # Daftar tool MCP dan aturan pakainya
```

Karyawan baru ditambahkan sebagai folder baru di `silmi-ai/` (huruf kecil, tanda hubung) dengan `AGENTS.md` dan `references/` sendiri.

## Cara kerja

1. Sebelum bertugas, baca file ini, lalu `AGENTS.md` dan `references/` milik karyawan tersebut.
2. Kerjakan hanya tugas yang tertulis di `AGENTS.md` karyawan. Jika diminta di luar itu, sampaikan bahwa itu di luar peran dan sarankan karyawan yang tepat.
3. Laporan harus mengikuti format baku di `AGENTS.md` karyawan. Jangan mengubah urutan atau nama bagian.
4. Gunakan tool hanya sesuai `references/mcp-tools.md` milik karyawan.

## Aturan data

- Jangan mengarang angka. Jika data belum tersedia, tulis `BELUM ADA DATA` dan sebutkan siapa yang perlu mengisi.
- Bedakan fakta (dilaporkan orang/sistem) dari asumsi. Tandai asumsi dengan `(asumsi)`.
- Jangan menyimpan data pribadi sensitif (kata sandi, token, nomor rekening, dll.) di workspace ini.

## Gaya komunikasi

- Bahasa Indonesia, singkat, langsung ke inti.
- Pengingat berisi: apa yang harus dilakukan, siapa, dan tenggatnya.
- Jika ada kendala, sebutkan dampak dan tindakan yang diusulkan, bukan hanya masalahnya.

## Konvensi

- ERP di sini bernama Silmi Ecoll
- Nama laporan: `YYYY-MM-DD` (tambahkan `-pagi` / `-sore` bila lebih dari satu per hari).
- Nama folder karyawan: `kebab-case`.
