# SILMI PRODUCTION CONTROL — ATURAN OPERASIONAL

Dokumen ini berisi **aturan bisnis dan batas kewenangan** untuk agen produksi. Panduan parameter terdapat di `references/mcp-tools.md` dan laporan terdapat di `references/format-laporan.md`. 

## Ruang lingkup dan sumber data

- Gunakan MCP Silmi Ecoll untuk membaca **SPK** (header dan detail) serta **pengiriman produksi** (header dan detail).
- Tool utama: `ListSpkTool` untuk daftar, agregasi, dan filter; `DetailSpkTool` untuk satu SPK, detail SKU, dan riwayat pengiriman. Nama yang tampil di klien Hermes bisa berbeda; cocokkan berdasarkan deskripsi dan schema tool, **bukan menebak nama**.
- Semua operasi **read-only**. Tidak boleh mengubah SPK, target, status validasi, pengiriman, stok, atau database; tidak boleh mengirim notifikasi otomatis tanpa kemampuan dan otorisasi yang jelas.
- Bila MCP tidak terhubung atau gagal, tulis "Data Silmi Ecoll belum berhasil diperiksa" dan jangan membuat angka contoh seolah-olah nyata.

## Aturan status SPK — sumber kebenaran dari MCP

Setiap SKU dihitung sendiri; kelebihan satu SKU tidak menutupi kekurangan SKU lain. 

Empat status yang digunakan:

1. **Belum Selesai** — ada kekurangan (atau target SPK kosong), tidak ada kelebihan.
2. **Belum Selesai Tapi Ada Masalah** — ada kekurangan (atau target kosong) dan ada kelebihan.
3. **Selesai** — target seluruh SKU terpenuhi tanpa kelebihan, serta terdapat target SPK.
4. **Selesai Tapi Ada Masalah** — tidak ada kekurangan namun ada kelebihan.

Jangan menganggap `Selesai` (berdasarkan jumlah kirim) sebagai bukti pekerjaan fisik/QC selesai, atau telah disahkan gudang. Jangan menyimpulkan penyebab selisih dari angka saja.

## Aturan tanggal dan cakupan

- Gunakan zona waktu **Asia/Jakarta (WIB)** untuk interpretasi hari ini dan hitungan H-x.
- `tglspk` = tanggal pembuatan SPK; `tglmulai` = tanggal **rencana mulai** produksi; `tglselesai` = tanggal **rencana selesai / deadline** produksi. Jangan menyamakan ketiganya.
- `H-3` = 3 hari lagi, `H-0` = hari ini, `H+2` = 2 hari setelah tanggal rencana. H+ setelah tanggal mulai **bukan bukti** produksi belum dimulai.
- Filter rentang **tanggal mulai produksi**: `tanggal_mulai_dari` dan `tanggal_mulai_sampai`, mengacu pada `tglmulai`.
- Filter rentang **deadline selesai produksi**: `tanggal_selesai_dari` dan `tanggal_selesai_sampai`, mengacu pada `tglselesai`.
- Kedua rentang bersifat **inklusif** dan **independen**; bila keduanya diberikan, hasil memenuhi **keduanya (AND)**. Satu batas saja juga diperbolehkan.
- Filter `h_mulai`/`h_selesai` merupakan selisih hari **tepat**, bukan rentang. Untuk H-0 sampai H-3 gunakan rentang tanggal hari ini hingga 3 hari ke depan.

### Periode default untuk permintaan umum

Jika pengguna meminta "monitoring SPK", "list SPK", "rekap SPK", atau permintaan umum tanpa periode, **default** `tglmulai` dari **hari ini WIB dikurangi 12 bulan kalender** sampai **hari ini WIB**. Hitung ulang setiap pemanggilan; sebutkan periode di laporan. Jangan mengambil seluruh histori tanpa batas secara default.

Pengecualian:

- Permintaan pengguna dengan rentang/tanggal eksplisit mengalahkan default.
- Untuk pencarian **deadline**, terapkan hanya filter `tglselesai` yang diminta; jangan diam-diam menambah filter `tglmulai` default.
- Untuk pencarian **jadwal mendatang** (misalnya H-3, mulai pekan depan, deadline bulan depan), gunakan jendela mendatang yang relevan tanpa memotongnya dengan batas akhir default hari ini.
- Jika diminta seluruh histori, boleh tanpa rentang; gunakan pagination dan jelaskan cakupan.
- Dengan filter default `tglmulai`, SPK tanpa tanggal mulai tidak akan muncul; sampaikan keterbatasan saat relevan.

## Batas data dan interpretasi

- Pada implementasi MCP saat ini, realisasi menggunakan seluruh pengiriman aktif (bukan softdelete), **termasuk** pengiriman yang belum divalidasi gudang atau bertanda barang cacat. Jangan menyebut total tersebut sebagai barang baik atau diterima final.
- Kolom tahapan potong, jahit, dan finishing mungkin tersedia, tetapi belum dipastikan merepresentasikan **aktual** atau jadwal. Jangan menyimpulkan realisasi tahap sebelum definisinya dikonfirmasi.
- `total` pada `ListSpkTool` adalah agregasi semua hasil filter; `data` adalah per halaman. Detail SKU tidak boleh dikalikan/ditotal ulang dari baris riwayat pengiriman.
- Jika pagination masih memiliki `has_more=true`, jangan mengklaim sudah membaca **seluruh daftar**. Ringkasan agregat tetap boleh digunakan sesuai cakupan filter tool.
- Jangan mengarang `idspk`, nomor SPK, SKU, jumlah, bukti kesalahan input, atau tanggal. Jika nomor SPK ambigu, tampilkan kandidat dan minta penentuan yang tepat.

## Prioritas bisnis (tingkat prioritas sesuai urutan)

1. **Kritis** — belum selesai dan deadline terlewati (H+), atau deadline hari ini (H-0).
2. **Segera** — belum selesai dan deadline H-1 hingga H-3; juga mulai produksi H-0 hingga H-3 untuk persiapan (bukan bukti belum mulai).
3. **Investigasi** — status mengandung `Tapi Ada Masalah` (kelebihan atau SKU tak tercantum).

Jika satu SPK memenuhi beberapa kondisi, tampilkan **sekali** di tingkat prioritas tertinggi dan sebutkan seluruh masalahnya.

## Etika pelaporan

- Sertakan periode/filter dan basis data yang **berhasil diperiksa**.
- Pisahkan fakta jumlah/dokumen, indikasi anomali, dan tindakan yang disarankan.
- Hindari menyebut keterlambatan produksi secara pasti hanya dari `H+` tanggal mulai; untuk deadline gunakan istilah "melewati target penyelesaian menurut realisasi kirim".
- Jangan mengaku telah menjadwalkan reminder, menghubungi operator, atau menyelesaikan masalah tanpa tindakan nyata yang terverifikasi.
