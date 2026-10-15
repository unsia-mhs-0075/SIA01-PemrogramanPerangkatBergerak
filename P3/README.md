# Pertemuan 3

Membuat dan Menjalankan Project Flutter

## Ringkasan

Pertemuan ini membahas **pembuatan project Flutter** lewat Android Studio
maupun Visual Studio Code, penyiapan emulator, dan penjelasan struktur
folder project Flutter.

Materi berasal dari modul praktikum mata kuliah Pemrograman Berbasis
Perangkat Bergerak beserta video pembelajaran dosen.

## Membuat dan Menjalankan Project

Langkah lewat Android Studio:

1. Pilih **Create New Flutter Project**
2. Pilih **Flutter Application**, lalu **Next**
3. Tentukan:
   - Project Name — nama aplikasi
   - Flutter SDK path — folder tempat Flutter dipasang
   - Project location — folder penyimpanan
   - Description — keterangan bebas
4. Klik **Finish**, tunggu proses penyiapan selesai (perlu koneksi internet)

Langkah lewat Visual Studio Code:

1. Buka **View → Command Palette** (`Ctrl + Shift + P`)
2. Ketik `flutter`, pilih **Flutter: New Project**
3. Pilih **Application**
4. Pilih folder penyimpanan, lalu tentukan nama project
5. Tunggu proses penyiapan selesai

Perintah yang setara untuk praktikum di repositori ini:

```bash
flutter create --org ac.id.unsia --project-name nama_aplikasi nama_folder
```

Hasilnya berupa project lengkap yang siap dijalankan — sudah terbukti
dikerjakan pada P1, termasuk build APK debug yang berhasil.

## Menjalankan di Emulator atau HP Android

**Lewat emulator:** buat **AVD Manager** → **Create Virtual Device** →
pilih perangkat → pilih versi sistem operasi Android (unduh bila belum
ada) → **Finish** → jalankan dengan tombol *play*.

**Lewat HP Android:** aktifkan mode developer
(*Settings → System → About Phone*, ketuk **Build number** beberapa kali),
lalu aktifkan **USB Debugging** pada **Developer options**, hubungkan
perangkat dengan kabel data, dan pastikan nama perangkat muncul di Android
Studio atau VS Code.

Praktikum di repositori ini dijalankan di **emulator Pixel 6 (Android 14,
API 34)** dengan akselerasi KVM. Urutan lengkapnya tercatat di
[README utama](../README.md).

## Struktur Folder Project Flutter

| Berkas / folder | Isi |
|---|---|
| `android/` | source code aplikasi Android |
| `ios/` | source code aplikasi iOS |
| `lib/` | source code Dart — tempat menulis kode aplikasi |
| `test/` | source code Dart untuk pengujian |
| `.gitignore` | konfigurasi Git |
| `.metadata` | metadata project, dibuat otomatis |
| `.packages` | alamat path package, dibuat oleh pub |
| `pubspec.yaml` | informasi project dan library yang dibutuhkan |
| `pubspec.lock` | versi library yang dipakai, untuk mengunci versi |
| `README.md` | penjelasan kode aplikasi |

## Membuat Hello World

Buka `lib/main.dart`, kemudian ubah menjadi:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Aplikasi Flutter Pertama',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Belajar Flutter'),
        ),
        body: const Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

Penjelasan widget yang dipakai:

| Widget | Fungsi |
|---|---|
| `MyApp` | `StatelessWidget`, widget induk |
| `MaterialApp` | membungkus aplikasi dengan tema material design |
| `Scaffold` | struktur dasar material design |
| `AppBar` | membuat app bar |
| `Center` | widget layout untuk menaruh widget di tengah |
| `Text` | menampilkan teks |

Kode di atas sudah **dijalankan sungguhan** di emulator Android, bukan
sekadar contoh. Screenshot hasilnya ada di
[README P1](../P1/README.md).

Modul dosen menuliskan `const MyApp({Key? key}) : super(key: key);`, yang
merupakan gaya Dart versi lama. Versi di repositori ini memakai
`const MyApp({super.key});` — bentuk ringkas yang setara pada Dart 3.9.0
(versi Flutter 3.35.1).

## Catatan Materi

Modul praktikum dosen menggunakan langkah Windows dengan JDK 15 dan Flutter
SDK di `D:\Android\flutter`. Praktikum di repositori ini dikerjakan di
Linux (Flutter 3.35.1, Dart 3.9.0, OpenJDK 21). Perbedaan yang perlu
diperhatikan — izin KVM untuk emulator dan penunjukan JDK untuk Gradle —
tercatat di [README utama](../README.md).
