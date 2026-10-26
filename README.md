# TERMULSCAN Build

Repository production build untuk TERMULScan.

## Pipeline otomatis

TERMULSCAN menjalankan CI pada push ke `main` dan pull request:
1. `flutter pub get`
2. `flutter analyze`
3. `flutter test`

TERMULSCAN-BUILD memeriksa commit `main` terbaru setiap 5 menit dan memverifikasi bahwa commit tersebut sama dengan commit pada push workflow CI terbaru yang sukses. Jika CI belum selesai, gagal, atau commit telah dirilis sebelumnya, pipeline tidak membuat APK duplikat. Pada pemeriksaan berikutnya, proses akan dicoba lagi.

Jika commit valid, repository ini:
1. Checkout SHA TERMULSCAN yang tepat.
2. Menjalankan ulang analyze dan test.
3. Menggunakan keystore production dari GitHub Secrets.
4. Membuat dan memverifikasi `TERMULScan-production.apk`.
5. Menerbitkan APK sebagai aset GitHub Release untuk commit sumber tersebut.

**Distribusi hanya melalui GitHub Releases.** Workflow tidak mengunggah Actions artifact dan tidak memerlukan unduhan artifact terpisah.

Tidak dibutuhkan `TERMULSCAN_BUILD_TOKEN`, dan tidak perlu memindahkan APK secara manual. Eksekusi terjadwal dapat sedikit terlambat sesuai antrean GitHub Actions.

## Required secrets di TERMULSCAN-BUILD

- `TERMULSCAN_KEYSTORE_BASE64` — base64 keystore production `.keystore` / `.jks`
- `TERMULSCAN_KEYSTORE_PASSWORD`
- `TERMULSCAN_KEY_ALIAS`
- `TERMULSCAN_KEY_PASSWORD`

Keystore production tidak pernah dikomit ke repository.

## Build manual

Buka **Actions → Build TERMULScan Production APK → Run workflow**. Biarkan `source_sha` kosong untuk memilih commit `main` terakhir yang sudah tervalidasi, atau isi SHA lengkap 40 karakter untuk membangun commit tertentu. Commit yang sudah dirilis akan dilewati untuk mencegah rilis duplikat.

## Output

- **GitHub Release:** tag `source-<source-sha>`
- **APK Release asset:** `TERMULScan-production.apk`

Unduh APK dari halaman Releases: https://github.com/jntcargo053b-wq/TERMULSCAN-BUILD/releases
