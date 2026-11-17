# Referensi MCP Silmi Ecoll Silmi — Monitoring Produksi

Dokumen ini menyatakan kontrak input/output berdasarkan kode Laravel MCP Tool yang telah dibahas. **Periksa schema tool yang sungguh terekspos pada koneksi Hermes**, karena nama prefiks tool bisa berbeda.

## ListSpkTool

Parameter opsional:

| Parameter | Arti |
|---|---|
| `idspk` | ID SPK positif yang diketahui |
| `nomorspk` | Sebagian nomor SPK, maksimum 5 karakter |
| `brandorder` | Sebagian teks brand order |
| `status_spk` | Salah satu dari 4 status SPK |
| `tanggal_mulai_dari` / `tanggal_mulai_sampai` | Batas inklusif pada kolom `produksi.tr_spk.tglmulai` |
| `tanggal_selesai_dari` / `tanggal_selesai_sampai` | Batas inklusif pada kolom `produksi.tr_spk.tglselesai` |
| `h_mulai` / `h_selesai` | Filter integer tepat: `3` H-3, `0` H-0, `-2` H+2 |
| `limit` / `page` | Maks. 100, default 50, halaman mulai 1 |

Tidak ada filter `tglspk` pada versi tool terbaru. Jika kedua kelompok filter tanggal diisi, digabung dengan **AND**.

Hasil penting: `filter`, `total` (ringkasan seluruh hasil filter), `pagination`, dan `data` (baris SPK per halaman). `data` memuat `idspk`, `nomorspk`, `tglmulai`, `tglselesai`, `h_mulai_spk`, `h_selesai_spk`, `status_spk`, `total_spk`, `total_pengiriman`, `total_kurang`, `total_lebih`, `item_kurang`, `item_lebih`, `item_di_luar_spk`.

Contoh permintaan periode **mulai produksi**:

```json
{"tanggal_mulai_dari":"2026-11-01","tanggal_mulai_sampai":"2026-11-15","page":1,"limit":50}
```

Contoh permintaan periode **deadline**:

```json
{"tanggal_selesai_dari":"2026-11-10","tanggal_selesai_sampai":"2026-11-30"}
```

Contoh kedua rentang (AND):

```json
{"tanggal_mulai_dari":"2026-11-01","tanggal_mulai_sampai":"2026-11-15","tanggal_selesai_dari":"2026-11-15","tanggal_selesai_sampai":"2026-11-30"}
```

## DetailSpkTool

- Wajib: `idspk` (integer ID SPK dari Silmi Ecoll).
- Opsional: `limit_pengiriman` (1–300, default 100), `page_pengiriman` (mulai 1).
- Output: `found`, `spk` (header/tahapan/status), `total`, `data_barang` (1 baris per kodebarang), `pagination_pengiriman`, `riwayat_pengiriman` (transaksi per halaman).
- Jika `found=false`, jangan mengarang detail SPK.

Contoh:

```json
{"idspk":123,"limit_pengiriman":100,"page_pengiriman":1}
```

## Aturan sumber data yang harus selalu disebut bila relevan

- Pengiriman aktif dihitung walau belum valid atau ditandai cacat.
- Validasi header dan detail ada pada riwayat; bukan filter kuantitas default.
- H-x dari tanggal rencana menggunakan zona waktu Asia/Jakarta.
- MCP tidak menyediakan bukti selesai fisik/QC dan belum mengirim reminder otomatis.
- Data aktual hanya ada setelah MCP Silmi Ecoll berhasil dihubungkan dan dipanggil.
