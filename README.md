# TERMULSCAN Build

Repository production build untuk TERMULScan.

## Pipeline otomatis

TERMULSCAN menjalankan CI pada push ke `main` dan pull request:
1. `flutter pub get`
2. `flutter analyze`
3. `flutter test`

TERMULSCAN-BUILD memeriksa commit `main` terbaru setiap 15 menit dan memverifikasi bahwa commit tersebut sama dengan commit pada push workflow CI terbaru yang sukses. Jika CI belum selesai, gagal, atau commit telah dibangun sebelumnya, pipeline tidak membuat APK duplikat. Pada pemeriksaan berikutnya, proses akan dicoba lagi.

Jika commit valid, repository ini:
1. Checkout SHA TERMULSCAN yang tepat.
2. Menjalankan ulang analyze dan test.
3. Menggunakan keystore production dari GitHub Secrets.
4. Membuat `TERMULScan-production.apk`.
5. Mengunggah APK sebagai Actions artifact.
6. Menerbitkan GitHub Release untuk commit sumber tersebut.

Tidak dibutuhkan `TERMULSCAN_BUILD_TOKEN`, dan tidak perlu memindahkan APK secara manual. Pemicu terjadwal berjalan paling sering setiap 15 menit; eksekusi dapat sedikit terlambat sesuai antrean GitHub Actions.

## Required secrets di TERMULSCAN-BUILD

- `TERMULSCAN_KEYSTORE_BASE64` — base64 keystore production `.keystore` / `.jks`
- `TERMULSCAN_KEYSTORE_PASSWORD`
- `TERMULSCAN_KEY_ALIAS`
- `TERMULSCAN_KEY_PASSWORD`

Keystore production tidak pernah dikomit ke repository.

## Build manual

Buka **Actions → Build TERMULScan Production APK → Run workflow**. Biarkan `source_sha` kosong untuk memilih commit `main` terakhir yang sudah tervalidasi, atau isi SHA lengkap 40 karakter untuk membangun commit tertentu. Commit yang sudah dirilis akan dilewati untuk mencegah rilis duplikat.

## Output

- Actions artifact: `TERMULScan-production-<source-sha>`
- GitHub Release: `source-<source-sha>` dengan APK `TERMULScan-production.apk`
