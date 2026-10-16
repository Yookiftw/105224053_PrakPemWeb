# Dokumen Teknis Modul 3 – TypeScript dan Komponen React

Nama/NIM      : Syauqi / [Isi NIM kamu]
Repositori    : [Isi tautan repositori GitHub kamu]

## 1. Tipe Data Produk
- **Isi `lib/tipe.ts`:**
  ```typescript
  export type Kategori = "Atasan" | "Bawahan" | "Aksesoris" | "Lainnya";
  export type Ukuran = "S" | "M" | "L" | "XL" | "All Size";

  export interface ProdukBajuBekas {
    id: string;
    namaProduk: string;
    harga: number;
    kategori: Kategori;
    ukuran: Ukuran;
    deskripsi?: string;
    merek?: string;
  }

- Alasan pemilihan union type serta properti opsional:
    Union type (Kategori dan Ukuran) dipilih untuk membatasi nilai yang bisa dimasukkan agar hanya sesuai dengan opsi yang tersedia, sehingga mencegah kesalahan ketik (typo) saat mengisi data. Properti opsional (menggunakan tanda ?) pada deskripsi dan merek diterapkan karena pada praktiknya, baju bekas (thrifting) tidak selalu memiliki merek yang jelas atau membutuhkan deskripsi tambahan.

## 2. Struktur Komponen
![Tabel Komponen](Nomor-2.png)

- Alasan pemecahan komponen dan penempatan "use client":
    Halaman dipecah menjadi komponen seperti SiteHeader dan KartuItem untuk mengurangi duplikasi kode sehingga satu bentuk UI bisa dipakai berulang kali hanya dengan mengirimkan tipe props yang berbeda[cite: 29]. Direktif "use client" ditempatkan pada komponen DaftarItem dan ItemTerbaru karena komponen tersebut memerlukan pengelolaan interaksi secara interaktif di peramban, seperti penggunaan hook useState, useEffect, dan event handler (onChange, onClick)[cite: 31, 38]. Komponen lain dibiarkan sebagai Server Component agar dirender lebih cepat di server.

## 3. State dan Interaksi
![Props dan State DaftarItem](Nomor-3.png)

- Penjelasan alur dari event hingga render ulang:
    Ketika pengguna berinteraksi (misalnya mengetik di kolom pencarian atau mengeklik tombol), sebuah event handler (seperti onChange atau onClick) akan terpicu. Fungsi ini kemudian memanggil setter function dari useState (misalnya setKataKunci) untuk memperbarui state. Setelah state berubah, React akan mendeteksi perubahan tersebut dan secara otomatis melakukan render ulang (re-render) pada komponen tersebut beserta komponen anak (child)-nya untuk menampilkan antarmuka yang sesuai dengan data terbaru.

## 4. Pengambilan Data
![item.json](Nomor-4.png)

- Hasil pengujian kondisi memuat, berhasil, dan gagal:

    - Memuat: Saat throttling disetel ke "Slow 4G", status state bernilai "memuat", lalu antarmuka menampilkan teks "Memuat data..." sementara fetch() sedang berjalan menunggu respons dari server[cite: 38, 39].

    - Berhasil: Saat jaringan normal, fetch() berhasil mengambil data dari rute /data/item.json. Status berubah menjadi "berhasil", dan fungsi setItems() memperbarui state sehingga tiga data baju terbaru berhasil dirender di layar[cite: 38].

    - Gagal: Saat alamat pengiriman diganti menjadi rute yang tidak valid, server merespons dengan status 404. Karena fungsi mengecek !res.ok, kode melemparkan eror secara eksplisit, ditangkap oleh blok catch, status berubah menjadi "gagal", dan pesan "Data gagal dimuat" tampil[cite: 38, 39].

## 5. Verifikasi
![Output CLI](Nomor-5.png)

## 6. Kendala dan Penyelesaian
- Kendala: Sempat terjadi kebingungan mengenai error penempatan hook useState pada komponen Server Component bawaan.

- Penyelesaian: Berdasarkan tabel penanganan masalah (troubleshooting), saya menyelesaikan ini dengan menambahkan direktif "use client"; pada baris paling atas di dalam berkas komponen terkait.

## 7. Catatan Pemanfaatan AI
- Alat: AI LLM (Gemini)

- Perintah utama: Meminta panduan pembuatan draf struktur Dokumen Teknis, penyusunan tipe data produk TypeScript dengan interface, serta penjelasan teknis React state dan pengambilan data.

- Bagian yang digunakan: Bagian 1 (definisi tipe ProdukBajuBekas), deskripsi alur interaksi di Bagian 3, dan analisis siklus fetching di Bagian 4.

- Cara memverifikasi: Saya membaca kembali teori dan penjelasannya untuk memastikan telah sesuai dengan pedoman di slide praktikum, lalu menyesuaikan sintaks interface dengan spesifikasi data toko Jubel Baju Bekas secara manual di VS Code, dan menjalankan kompilasi TypeScript untuk memeriksa keabsahannya.