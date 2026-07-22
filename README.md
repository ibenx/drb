# Download Manager untuk Godot 4.x

File yang disiapkan:

- `download_manager/DownloadManager.tscn`
- `download_manager/download_manager.gd`
- `download_manager/DownloadItem.tscn`
- `download_manager/download_item.gd`

## Cara pasang

1. Salin folder `download_manager` ke project Godot Anda.
2. Buka `Project > Project Settings > Autoload`.
3. Tambahkan scene `res://download_manager/DownloadManager.tscn`.
4. Beri nama autoload `DownloadManager`.
5. Pastikan autoload aktif.

Setelah itu `DownloadManager` bisa dipanggil dari scene mana pun.

## API utama

Tambah download baru:

```gdscript
DownloadManager.add_download("https://example.com/file.zip")
```

Alias yang lebih singkat:

```gdscript
DownloadManager.enqueue("https://example.com/file.zip")
```

Kontrol jendela:

```gdscript
DownloadManager.show_window()
DownloadManager.hide_window()
DownloadManager.toggle_window()
```

Kontrol item download:

```gdscript
DownloadManager.pause_download(download_id)
DownloadManager.resume_download(download_id)
DownloadManager.cancel_download(download_id)
DownloadManager.delete_download(download_id)
```

## Signal yang tersedia

```gdscript
DownloadManager.download_finished.connect(_on_download_finished)
DownloadManager.download_failed.connect(_on_download_failed)
```

Contoh:

```gdscript
func _on_download_finished(download_id: String, save_path: String, source_url: String) -> void:
	print("Selesai:", download_id, " -> ", save_path)
```

## Tampilan UI

- Posisi di bawah layar
- Cocok untuk layout portrait `1080 x 1920`
- Ukuran teks, ikon, tombol, dan panel diperbesar lagi agar nyaman dibaca di layar portrait
- Bisa disembunyikan dan ditampilkan lagi
- Setiap item punya:
  - nama file
  - URL sumber
  - ukuran file
  - progress bar
  - label ukuran yang sudah terunduh
  - tombol ikon `▶ / ⏸ / ↻`
  - tombol ikon `✕`
  - tombol ikon `🗑`
  - link `Buka file` saat selesai

## Catatan penting

- Cukup kirim URL download, path tujuan akan otomatis dibuat ke `user://downloads/<nama_file_dari_url>`.
- Download berjalan satu per satu sesuai antrian.
- Download sekarang dijalankan lewat worker thread dan memiliki logika finalisasi tambahan agar file `.part` otomatis selesai dan di-rename.
- Fitur `pause/lanjutkan` mencoba melanjutkan dari file `.part`.
- Resume akan bekerja penuh bila server mendukung header `Range`.
- Jika server tidak mendukung resume, download akan dimulai lagi dari awal.
- Ikon tombol memakai simbol Unicode agar langsung bisa dipakai tanpa asset font eksternal tambahan.
- Control pembungkus di luar panel manager disetel agar tidak memblokir input ke tombol lain pada layar.
