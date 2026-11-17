
# Format Laporan Baku — Rekap SPK 12 Bulan Terakhir

> **Tujuan:** Menjadi referensi format laporan untuk AI Agent **Silmi Production Control**. Setiap laporan SPK 12 bulan terakhir **wajib mengikuti struktur, urutan, definisi, dan format tabel di bawah ini**. Semua angka, periode, serta temuan diisi ulang berdasarkan hasil pengambilan data yang berlaku; **jangan menyalin angka dari contoh**.

## A. Ketentuan Wajib

1. **Judul laporan:** `Rekap SPK 12 Bulan Terakhir`.
2. **Periode:** Tuliskan tanggal mulai–tanggal akhir secara eksplisit, berdasarkan tanggal pembuatan SPK (`tglspk`), **bukan** tanggal mulai produksi atau deadline. Rentang berjalan 12 bulan dapat mencakup 13 nama bulan kalender karena bulan awal dan akhir hanya dihitung sebagian.
3. **Sumber data:** Jelaskan sumber ERP/MCP, kapan data diambil, cakupan halaman/jumlah SPK unik jika diketahui, apakah data diambil ulang, dan apakah angka sudah direkonsiliasi. **Jangan mengklaim mengambil ulang data jika hanya menggunakan hasil sebelumnya.**
4. **Satuan hitung:** Satu SPK unik dihitung satu kali. Lakukan deduplikasi sebelum menghitung dan gunakan filter `tglspk` pada periode yang ditentukan.
5. **Dimensi status:** Pisahkan status realisasi (**Selesai** atau **Belum selesai**) dan status masalah (**Tidak ada masalah** atau **Ada masalah**). Keempat kombinasinya **saling eksklusif**.
6. **Definisi status:** Hitung realisasi dan masalah dari target versus pengiriman **per kode barang**, tidak hanya agregat total SPK. Kelebihan suatu kode barang **tidak** menutup kekurangan kode barang lain.
7. **Pengiriman aktif ERP:** Bila sumber menerapkan definisi ini, pengiriman aktif dapat mencakup pengiriman yang belum divalidasi dan yang bertanda cacat. Nyatakan batasan ini pada catatan data.
8. **Batas interpretasi:** Status `Selesai` berdasarkan kuantitas **bukan** bukti bahwa produksi fisik selesai, lolos QC, atau barang sudah diterima gudang.
9. **Rekonsiliasi:** Total setiap baris dan kolom harus cocok. Angka SPK bermasalah adalah **bagian dari** total SPK, bukan tambahan.
10. **Ketidaklengkapan data:** Jika data belum lengkap, tampilkan `Data belum tersedia` dan jelaskan keterbatasannya. Jangan mengarang jumlah, status, sumber, atau alasan masalah.
11. **Bahasa dan gaya:** Bahasa Indonesia, objektif, ringkas, operasional; gunakan tabel Markdown dan hindari kesimpulan yang tidak didukung data.

## B. Template Output Wajib

> Salin struktur laporan berikut saat membuat output. Ganti placeholder dalam tanda `{{...}}` dengan data aktual. Jangan tampilkan placeholder yang belum terisi tanpa keterangan.

# Rekap SPK 12 Bulan Terakhir

**Periode:** {{tanggal_mulai}}–{{tanggal_akhir}}, berdasarkan tanggal pembuatan SPK (`tglspk`), bukan deadline.

**Sumber:** {{sumber_data_dan_waktu_pengambilan}}; mencakup {{jumlah_halaman_atau_keterangan_cakupan}} / {{jumlah_spk_unik_sumber}} SPK unik sebelum filter periode, kemudian difilter sesuai periode. {{status_penghitungan_ulang_dan_rekonsiliasi}}. {{keterangan_apakah_erp_diambil_ulang}}.

### 1. Ringkasan status

| Realisasi | Tidak ada masalah | Ada masalah | Total |
| --- | ---: | ---: | ---: |
| Selesai | {{selesai_tanpa_masalah}} | {{selesai_bermasalah}} | {{total_selesai}} |
| Belum selesai | {{belum_selesai_tanpa_masalah}} | {{belum_selesai_bermasalah}} | {{total_belum_selesai}} |
| Total | {{total_tanpa_masalah}} | {{total_bermasalah}} | {{total_spk}} |

{{total_bermasalah}} SPK bermasalah merupakan bagian dari total {{total_spk}} SPK, bukan tambahan.

### 2. Rincian asal bulan SPK

| Bulan pembuatan | Selesai | Selesai Tapi Ada Masalah | Belum Selesai | Belum Selesai Tapi Ada Masalah | Total |
| --- | ---: | ---: | ---: | ---: | ---: |
| {{bulan_pertama}} — mulai tgl {{tanggal_awal}} | {{n1}} | {{n2}} | {{n3}} | {{n4}} | {{nt}} |
| {{bulan_berikutnya}} | {{n1}} | {{n2}} | {{n3}} | {{n4}} | {{nt}} |
| {{ulang_per_bulan_sesuai_periode}} | {{n1}} | {{n2}} | {{n3}} | {{n4}} | {{nt}} |
| {{bulan_terakhir}} — s.d. tgl {{tanggal_akhir_bulan}} | {{n1}} | {{n2}} | {{n3}} | {{n4}} | {{nt}} |
| Total | {{selesai_tanpa_masalah}} | {{selesai_bermasalah}} | {{belum_selesai_tanpa_masalah}} | {{belum_selesai_bermasalah}} | {{total_spk}} |

*Rentang bergulir 12 bulan dapat menyentuh 13 nama bulan karena bulan pada awal dan akhir periode hanya dihitung sebagian.*

### Kesimpulan operasional

- {{prioritas_spk_belum_selesai_terbaru_dengan_bulan_jumlah_dan_tindakan}}
- {{temuan_spk_lama_belum_selesai_dengan_bulan_jumlah_dan_tindakan}}
- {{temuan_lain_yang_penting_jika_ada}}
- {{temuan_spk_selesai_bermasalah_dan_jenis_penyimpangannya}}
- {{bulan_atau_kelompok_dengan_realisasi_baik_jika_didukung_data}}

**Catatan data:** Status berdasarkan target dan pengiriman per kode barang; kelebihan satu barang tidak menutup kekurangan barang lain. {{cakupan_pengiriman_aktif_erp_yang_berlaku}}. “Selesai” bukan konfirmasi fisik selesai, lolos QC, atau diterima gudang.

## C. Aturan Penyusunan Kesimpulan

- Utamakan temuan **yang membutuhkan tindak lanjut**, terutama SPK belum selesai, SPK lama yang tertunda, serta SPK selesai tetapi masih memiliki selisih kuantitas.
- Sebutkan **bulan/periode, jumlah SPK, kondisi, dan tindakan yang disarankan** jika datanya tersedia.
- Kelompokkan temuan yang berhubungan agar kesimpulan tidak menjadi daftar panjang per SPK.
- Jangan menyimpulkan setiap SPK `Ada masalah` sebagai *kelebihan pengiriman* apabila penyimpangan aktual belum diperiksa; periksa apakah masalah berupa kelebihan, kekurangan, atau kombinasi per kode barang.
- Jangan menyatakan SPK benar-benar selesai secara fisik, telah lolos QC, atau diterima gudang hanya berdasarkan status kuantitas.
- Jika tidak ada temuan tertentu, jangan memaksakan poin kesimpulan; template adalah standar struktur, bukan kewajiban membuat temuan fiktif.

## D. Pemeriksaan Sebelum Mengirim Laporan

- [ ] Periode menggunakan `tglspk`, bukan deadline.
- [ ] Seluruh SPK unik dalam periode sudah dihitung satu kali.
- [ ] `Selesai + Belum selesai = Total SPK`.
- [ ] `Tidak ada masalah + Ada masalah = Total SPK`.
- [ ] Empat kategori pada tabel bulanan berjumlah sama dengan total masing-masing bulan.
- [ ] Jumlah semua bulan sama dengan total pada tabel ringkasan.
- [ ] Empat total kategori tabel bulanan sama dengan empat sel kategori tabel ringkasan.
- [ ] Sumber, cakupan pengambilan data, dan status refresh ERP dinyatakan dengan benar.
- [ ] Catatan batasan status dan pengiriman dicantumkan.
- [ ] Tidak ada angka atau temuan yang disalin dari contoh lama tanpa verifikasi ulang.

---

## E. Contoh Laporan Terisi — Referensi Historis, Bukan Data Default

> **Contoh dari pengguna, periode 10 November 2025–10 November 2026.** Bagian ini digunakan **hanya sebagai acuan tampilan dan tingkat detail**. **Jangan menggunakan angkanya sebagai nilai saat membuat laporan baru.**

# Rekap SPK 12 Bulan Terakhir

Periode: 10 November 2025–10 November 2026, berdasarkan tanggal pembuatan SPK (`tglspk`), bukan deadline.

Sumber: hasil pengambilan ERP sebelumnya pada hari ini, mencakup seluruh 12 halaman / 1.146 SPK unik, kemudian difilter sesuai periode. Angka dihitung ulang dan direkonsiliasi; ERP belum diambil ulang pada rekap ini.

### 1. Ringkasan status

| Realisasi | Tidak ada masalah | Ada masalah | Total |
| --- | ---: | ---: | ---: |
| Selesai | 99 | 29 | 128 |
| Belum selesai | 13 | 4 | 17 |
| Total | 112 | 33 | 145 |

33 SPK bermasalah merupakan bagian dari total 145 SPK, bukan tambahan.

### 2. Rincian asal bulan SPK

| Bulan pembuatan | Selesai | Selesai Tapi Ada Masalah | Belum Selesai | Belum Selesai Tapi Ada Masalah | Total |
| --- | ---: | ---: | ---: | ---: | ---: |
| November 2025 — mulai tgl 10 | 3 | 7 | 1 | 1 | 12 |
| Desember 2025 | 2 | 8 | 0 | 1 | 11 |
| Januari 2026 | 3 | 9 | 1 | 2 | 15 |
| Februari 2026 | 12 | 4 | 2 | 0 | 18 |
| Maret 2026 | 6 | 0 | 0 | 0 | 6 |
| April 2026 | 11 | 0 | 0 | 0 | 11 |
| Mei 2026 | 13 | 0 | 0 | 0 | 13 |
| Juni 2026 | 12 | 0 | 0 | 0 | 12 |
| Juli 2026 | 14 | 1 | 0 | 0 | 15 |
| Agustus 2026 | 12 | 0 | 0 | 0 | 12 |
| September 2026 | 9 | 0 | 1 | 0 | 10 |
| Oktober 2026 | 2 | 0 | 6 | 0 | 8 |
| November 2026 — s.d. tgl 10 | 0 | 0 | 2 | 0 | 2 |
| Total | 99 | 29 | 13 | 4 | 145 |

*Rentang bergulir 12 bulan menyentuh 13 nama bulan karena November pada awal dan akhir periode hanya dihitung sebagian.*

### Kesimpulan operasional

- Oktober–November 2026: ada 8 SPK belum selesai; prioritaskan konfirmasi sisa barang dan rencana pengirimannya.
- November 2025–Februari 2026: ada 8 SPK belum selesai, termasuk 4 bermasalah. Rekonsiliasi target dan pengiriman per kode barang diperlukan.
- September 2026: masih ada 1 SPK belum selesai.
- 29 SPK selesai tetapi bermasalah berasal dari November–Desember 2025, Januari–Februari 2026, dan Juli 2026. Target sudah terpenuhi, tetapi terdapat kelebihan pengiriman yang perlu diperiksa.
- Maret–Juni dan Agustus 2026: seluruh SPK berstatus Selesai tanpa masalah kuantitas.

**Catatan data:** Status berdasarkan target dan pengiriman per kode barang; kelebihan satu barang tidak menutup kekurangan barang lain. Pengiriman aktif ERP termasuk yang belum divalidasi dan bertanda cacat. “Selesai” bukan konfirmasi fisik selesai, lolos QC, atau diterima gudang.
