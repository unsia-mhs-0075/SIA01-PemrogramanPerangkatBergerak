# SIA01-Pemrograman Berbasis Perangkat Bergerak

Pemrograman Berbasis Perangkat Bergerak: Android Studio, VS Code & Flutter

## Pertemuan

| Pertemuan | Materi | Status |
|---|---|---|
| [Pertemuan 1](P1) | Pengenalan Flutter & Android, Hello World | ✅ dibuat & **terbukti jalan** di emulator |
| [Pertemuan 2](P2) | Membuat Hello World pada Flutter | ✅ didokumentasikan |
| [Pertemuan 3](P3) | Membuat project & menjalankan di emulator | ✅ didokumentasikan |
| Pertemuan 4 | — | ⬜ folder materi **kosong** |
| Pertemuan 5 | — | ⬜ materi belum tersedia |
| Pertemuan 6 | — | ⬜ materi belum tersedia |
| Pertemuan 7 | — | ⬜ materi belum tersedia |
| [Pertemuan 8](P8) | Ujian Akhir Semester | ✅ dikerjakan, materi ujian disimpan di luar repo |

Hanya P1, P2, P3, dan P8 yang memiliki folder di repositori ini. Pertemuan
lain masih berupa materi di folder Dokumen, atau belum tersedia dari dosen.

## Lingkungan Pengembangan

Semua pertemuan di repo ini dikerjakan dan dijalankan di lingkungan berikut.
Path-nya di bawah adalah lokasi sebenarnya di mesin pengembang — modul asli
dosen menyebutkan contoh versi lama (JDK 15, Windows), sehingga catatan ini
dipakai sebagai acuan yang berlaku saat praktikum dikerjakan.

### Komponen yang Terpasang

| Komponen | Versi | Lokasi |
|---|---|---|
| Flutter SDK | 3.35.1 (channel stable) | `~/Appz/flutter` |
| Dart | 3.9.0 | menyertai Flutter SDK |
| Android Studio | (versi terpasang) | `~/Appz/android-studio` |
| Android SDK | platform android-29…37.2 | `~/Appz/Sdk` |
| System image | android-34, google_apis, x86_64 | `~/Appz/Sdk/system-images/` |
| Emulator (AVD) | `praktikum_p1` (Pixel 6, Android 14) | `~/Appz/Sdk/emulator/` |
| JDK untuk build | OpenJDK 21.0.12 | `/usr/lib/jvm/java-21-openjdk-amd64` |
| VS Code | (versi terpasang) | — |

### Variabel Lingkungan

Sesuatu yang perlu diekspor di setiap sesi terminal baru:

```bash
export PATH="$HOME/Appz/flutter/bin:$HOME/Appz/Sdk/platform-tools:$PATH"
export ANDROID_HOME="$HOME/Appz/Sdk"
```

Cek keterhubungan alat bantu dengan:

```bash
flutter doctor
```

Catatan: pemeriksaan toolchain pada `flutter doctor` bisa memakan waktu
lebih dari empat menit ketika SDK masih menyiapkan komponen. Waktu tunggu
ini bukan kegagalan — yang menjadi penanda lingkungan siap adalah
`[✓] Android Studio` dan `[✓] Connected device`.

### Membuat Project Baru

```bash
flutter create --org ac.id.unsia --project-name nama_aplikasi nama_folder
```

`--org ac.id.unsia` menghasilkan package name `ac.id.unsia.nama_aplikasi`,
sehingga identitas aplikasi tetap konsisten di semua pertemuan.

### Menjalankan di Emulator

Urutannya: system image → AVD → jalankan emulator → tunggu boot selesai.

```bash
# dipasang sekali saja
sdkmanager "system-images;android-34;google_apis;x86_64"
avdmanager create avd -n praktikum_p1 -k "system-images;android-34;google_apis;x86_64" \
    -d pixel_6 --force

# jalankan emulator (lihat catatan KVM di bawah)
~/Appz/Sdk/emulator/emulator -avd praktikum_p1 -no-audio -no-boot-anim \
    -gpu swiftshader_indirect -no-snapshot

# tunggu sampai boot selesai, jangan sekadar tidur
sg kvm -c "adb shell getprop sys.boot_completed"   # → 1 berarti siap
```

Bila emulator sudah menyala dan `flutter devices` mengenalinya
(`emulator-5554`), aplikasi dapat langsung dijalankan dengan:

```bash
flutter run -d emulator-5554
```

Cara yang lebih bersih untuk disertakan sebagai bukti kerja adalah
membangun APK lebih dahulu, lalu memasangnya:

```bash
flutter build apk --debug
sg kvm -c "adb install -r build/app/outputs/flutter-apk/app-debug.apk"
sg kvm -c "adb shell monkey -p ac.id.unsia.nama_aplikasi \
    -c android.intent.category.LAUNCHER 1"
sg kvm -c "adb exec-out screencap -p" > screenshot/01-hasil.png
```

### Dua Hal yang Wajib Diperhatikan

**1. Izin KVM untuk emulator.** Emulator menolak berjalan tanpa akses
`/dev/kvm` (pesan: `This user doesn't have permissions to use KVM`). User
`mhs0075` sudah ditambahkan ke grup `kvm`, tetapi perubahan grup hanya
berlaku pada login baru. Di sesi terminal yang sudah lama terbuka, bungkus
perintah emulator dan setiap pemanggilan `adb` dengan:

```bash
sg kvm -c "<perintah>"
```

Perlu diingat bahwa `sg` membuka shell baru, sehingga `PATH` dan
`ANDROID_HOME` yang diekspor di shell pemanggil **tidak** ikut terbawa —
ekspor ulang di dalam perintah yang dikutip.

**2. Versi JDK untuk Gradle.** Android Studio membawa JDK sendiri (JBR)
yang pada mesin ini berisi JDK 25, sedangkan Gradle 8.12 yang dipakai
Flutter 3.35.1 belum mendukungnya. Kegagalannya muncul tanpa pesan
penjelas — hanya tertulis nomor versi `25.0.3` sebagai seluruh isi
bagian *What went wrong*. Menyetel `JAVA_HOME` saja tidak cukup karena
daemon Gradle tetap memilih JBR milik Studio. Perbaikannya adalah
menambahkan satu baris ke `android/gradle.properties` setiap project:

```properties
org.gradle.java.home=/usr/lib/jvm/java-21-openjdk-amd64
```

Build pertama juga membutuhkan waktu lama (Gradle beserta dependensinya
berukuran ratusan MB), jadi jalankan `flutter build apk --debug` dengan
sabar atau sebagai proses latar belakang.
