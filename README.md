# Praktikum Effect Custom Hook dan Router

Proyek pendamping modul Word. Jalankan dari folder ini, bukan dari folder paket paling luar.

## Persiapan

1. Instal Node.js 22 versi 22.12 atau lebih baru (atau Node.js 24).
2. Ekstrak seluruh ZIP terlebih dahulu. Jangan membuka index.html dengan klik ganda.
3. Buka terminal pada folder `praktikum-spa` yang memuat package.json.
4. Instal dependency dan jalankan:

```bash
npm ci
npm run dev
```

Buka alamat yang tercetak di terminal (biasanya http://localhost:5173).
Instalasi pertama memerlukan internet. `node_modules` tidak disertakan; `npm ci`
memasangnya dari package-lock.json. Tidak perlu menjalankan create-vite lagi.
Jika npm.ps1 diblokir PowerShell, gunakan Command Prompt atau npm.cmd.
Hentikan server dengan Ctrl+C.

## Pemeriksaan

```bash
npm run lint
npm run build
npm run preview
```

Preview biasanya di http://localhost:4173. Periksa output terminal.
Build menghasilkan folder dist; preview menjalankan dist secara lokal.
Aplikasi tidak membutuhkan API key, file .env, database, backend, atau gambar eksternal.

## Route dan perilaku

- `/`: beranda.
- `/products`: katalog; `/products?q=Buku` membuka pencarian.
- `/products?category=Buku`: filter kategori (Buku atau Perangkat). Parameter `q`
  dan `category` dapat dipakai bersamaan, misalnya `/products?q=Key&category=Perangkat`.
  Nilai `category` yang tidak dikenal diperlakukan sebagai semua kategori.
- `/products/:id`: detail produk (ID 1 sampai 4).
- `/favorites`: favorit dari state bersama, dengan tombol Hapus semua favorit.
- `/about`: halaman Tentang.
- Route lain menampilkan halaman tidak ditemukan.

Klik favorit pada katalog atau detail akan memperbarui badge navigasi dan halaman
favorit. Data ditulis pada key `praktikum-favorites-v1` di localStorage.
State dipulihkan setelah reload pada origin yang sama. Mengganti host atau port
berarti menggunakan penyimpanan origin berbeda. Untuk reset data, hapus key melalui
Developer Tools lalu reload. Data rusak dipulihkan menjadi array kosong.

Pencarian berasal dari URL. Pengetikan memakai replace agar history tidak berisi
setiap karakter. Tombol Reset membuat entri baru: cari Buku, reset, lalu Back dan
Forward untuk mengamati pemulihan URL dan input.

## Struktur sumber dan hubungan dengan modul

- `src/main.jsx`: StrictMode, BrowserRouter, dan provider.
- `src/App.jsx`: data contoh (dengan kategori), navigasi, route, halaman (termasuk
  About), ProductCard, usePageTitle.
- `src/hooks/usePersistentFavorites.js`: baca data awal, Effect persistensi, toggle,
  dan clearAll.
- `src/hooks/useFavorites.js`: akses state bersama melalui Context.
- `src/context/FavoritesContext.js`: objek Context.
- `src/context/FavoritesProvider.jsx`: satu pemilik state favorit.
- `src/style.css`: tampilan responsif.
- `index.html`: dokumen HTML utama.
- `vite.config.js`: konfigurasi Vite dan plugin React.
- `eslint.config.js`: pemeriksaan JavaScript, Hooks, dan Fast Refresh.
- `package.json` dan `package-lock.json`: dependency dan versi terpasang.

Modul Word menggabungkan hook dan provider pada favorites.jsx untuk kemudahan
penjelasan. Paket runnable ini memisahkannya ke folder hooks dan context supaya
Fast Refresh tidak menandai ekspor campuran. Perilaku dan studi kasus tetap sama.

## Ruang lingkup tugas

Implementasi inti modul dan tugas mandiri wajib sudah dikerjakan:

1. Kategori produk dan filter `category` pada URL. `q` dan `category` saling
   dipertahankan karena setiap perubahan parameter menyalin `URLSearchParams` lama.
2. Tombol Hapus semua favorit pada halaman favorit (`clearAll` di
   `usePersistentFavorites`). Badge, kartu, dan localStorage ikut konsisten, termasuk
   setelah reload.
3. Halaman Tentang (`/about`) dengan judul tab dari custom hook `usePageTitle`.
4. Penjelasan hook berulang dan provider sebagai pemilik state ada di laporan.
5. Lembar pengujian 14 kasus ada di `docs/Lembar_Pengujian.md`.

Sinkronisasi antartab adalah pengayaan opsional dan tidak dikerjakan.
Dua pemanggilan useState terpisah tidak berbagi state; semua halaman membaca
provider yang sama. Tidak ada salinan state favorit pada halaman.

## Versi lingkungan

- Versi dependency terpasang (dari package-lock.json): React dan React DOM 19.3.1,
  React Router 7.18.4, Vite 7.3.7, @vitejs/plugin-react 5.2.0, ESLint 9.39.5.
- Pemeriksaan lint, build, dan pengujian otomatis dijalankan dengan Node.js v22.22.2
  dan npm 10.9.7.
- Versi Node dan npm pada perangkat Anda: isi dari `node --version` dan `npm --version`.

## Deployment

Untuk BrowserRouter, server produksi harus mengembalikan index.html ketika route
aplikasi seperti /products/1 diminta langsung. Contoh konfigurasi Nginx disediakan
pada folder docs paket. Sesuaikan lokasi dist dan server_name dengan server Anda.
Vite dev dan preview bukan server produksi. Deployment tidak dilakukan oleh paket ini.

## Referensi

- https://react.dev/reference/react/useEffect
- https://react.dev/learn/reusing-logic-with-custom-hooks
- https://react.dev/learn/sharing-state-between-components
- https://reactrouter.com/start/declarative/installation
- https://reactrouter.com/start/declarative/routing
- https://vite.dev/guide/
