# WStore Service Application

Katalog aplikasi WStore (AstralFusionOS). Repo ini dikelola maintainer. Developer lain tidak upload sendiri: kirim aplikasinya ke maintainer, maintainer yang memasukkan.

## Struktur

```
<dev>/<app>/
├── app.json     metadata (wajib)
├── icon.svg     ikon 48x48, svg atau png
├── index.html   isi aplikasi
└── ...          css, js, gambar (path relatif: ./style.css)
```

- Nama folder dev = nama developer di store. ID app = `dev/app`.
- Store cuma baca pola `<dev>/<app>/app.json` (tepat dua tingkat).
- Folder berawalan `_` atau `.` (misal `_template`) tidak muncul di store.

## Untuk developer: minta app dimasukkan

1. Bikin app web statis (HTML, CSS, JS). Tanpa build, tanpa SDK. Mulai dari `_template/hello/`.
2. Tes: buka `index.html` langsung di browser.
3. Kirim ke maintainer: zip folder app, atau link repo-nya. Sertakan nama developer yang mau ditampilkan.
4. Maintainer review, lalu memasukkannya. Setelah push, app muncul di store maksimal 10 menit.
5. Mau update versi? Kirim lagi file barunya.

Aturan app:
- Semua file di satu folder, path relatif, jangan absolut (`/style.css`).
- Jalan di jendela (iframe) AstralFusionOS: `desktop` harus muat di jendela ukuran variabel, `mobile` harus muat di layar HP.
- Data diurus app sendiri (misal `localStorage`). Belum ada SDK dari store.
- Jangan taruh token, password, atau data pribadi.

## Untuk maintainer: cara memasukkan app (step by step)

1. Terima zip atau link repo dari developer. **Baca kodenya dulu**: cari `fetch`/`XMLHttpRequest` ke domain asing, `eval`, script dari CDN yang tidak dikenal, dan pencurian data. App dijalankan di origin yang sama dengan app lain, jadi satu app jahat bisa membaca `localStorage` app lain.
2. Bikin folder `<dev>/<app>/` (huruf kecil, tanpa spasi dan tanda kutip).
3. Taruh semua file app di folder itu, plus `icon.svg` dan `app.json`.
4. Isi `app.json`:

```json
{
  "name": "My App",
  "category": "Utilities",
  "summary": "Satu kalimat pendek.",
  "description": "Penjelasan lebih panjang.",
  "version": "1.0.0",
  "notes": "Perubahan di versi ini.",
  "platform": ["desktop", "mobile"],
  "icon": "icon.svg",
  "entry": "index.html"
}
```

5. Tes: buka `index.html` di browser. Cek `app.json` valid (JSON rusak = app dilewati).
6. Commit dan push ke `main`. Di GitHub bisa juga lewat Add file > Upload files, atau drag folder.
7. Pastikan GitHub Pages aktif: Settings > Pages > Deploy from a branch > `main` > `/ (root)`. Cek `https://biannn12890.github.io/WStoreServiceApplication/<dev>/<app>/` terbuka.
8. Tunggu maksimal 10 menit, buka store, app muncul.

Update versi: ganti file app, naikkan `version` dan `notes`, push.
Hapus app: hapus foldernya, push.

## Field app.json

| Field | Default | Keterangan |
|---|---|---|
| `name` | nama folder app | Nama tampilan |
| `category` | `Other` | Bebas, kategori baru otomatis jadi section |
| `summary` | kosong | Satu baris di card |
| `description` | `summary` | Teks halaman detail |
| `version` | kosong | Contoh `1.2.0` |
| `notes` | kosong | Tampil sebagai "What's new" |
| `platform` | `["desktop","mobile"]` | `["desktop"]` cuma laptop, `["mobile"]` cuma HP, dua-duanya = responsif |
| `icon` | `icon.svg` | File ikon di folder app |
| `entry` | `index.html` | File yang dibuka saat app dijalankan |
| `featured` | - | `true` = jadi hero di Home. Pasang di satu app saja |

Diisi otomatis: Developer (nama folder dev), ID, dan Size (total ukuran file di folder app).

## Kenapa link raw.githubusercontent.com tidak dipakai buat jalanin app

GitHub mengirim HTML dari `raw.githubusercontent.com` sebagai teks biasa, jadi tidak tampil sebagai halaman. Karena itu app dijalankan lewat GitHub Pages.
