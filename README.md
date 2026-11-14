# NavigationLab

Aplikasi Android sederhana untuk mata kuliah Pengembangan Aplikasi Perangkat Bergerak (TIF – B), Fakultas Ilmu Komputer, Universitas Brawijaya. Project ini adalah hasil Modul Praktikum 08: refactoring dari callback ke Kotlin Coroutines dan Flow, ditambah fitur Refresh.

## Fitur

- Daftar profil diambil dari REST API menggunakan Retrofit.
- Halaman Detail untuk melihat data satu profil berdasarkan ID.
- Pencarian reaktif: hasil muncul otomatis setelah pengguna berhenti mengetik sekitar 500 ms.
- Tombol Refresh untuk memuat ulang data, mengikuti query pencarian yang sedang aktif.
- Loading state, error state, dan tombol Coba Lagi.
- Navigasi antar halaman: Home, Detail, Profile, dan About.

## Teknologi

- Kotlin dan Jetpack Compose (Material 3)
- Navigation Compose 2.10.2
- Retrofit 3.0.0 dengan converter Gson
- Kotlinx Coroutines 1.11.0
- Lifecycle 2.9.4 (ViewModel dan `collectAsStateWithLifecycle`)

## Arsitektur

Alur data mengikuti pola berikut:

```
Compose UI -> ViewModel -> Repository -> Remote Data Source -> REST API
```

- **Remote data source dan repository** mengekspos `suspend function` untuk operasi sekali jalan.
- **ViewModel** menjalankan pekerjaan lewat `viewModelScope.launch` dan mengekspos state sebagai `StateFlow` yang read-only.
- **Compose UI** mengoleksi state dengan `collectAsStateWithLifecycle()` dan hanya mengirim event ke ViewModel.
- **Pencarian** memakai operator `debounce`, `distinctUntilChanged`, dan `collectLatest`.

## Struktur Package

```
com.example.navigationlab
├── data
│   ├── model
│   ├── remote
│   └── repository
├── ui
│   ├── detail
│   ├── home
│   └── screen
├── MainActivity.kt
├── NavigationLabApp.kt
└── Routes.kt
```

## Cara Menjalankan

1. Clone repository ini.
2. Buka project dengan Android Studio.
3. Tunggu proses Gradle sync selesai.
4. Jalankan aplikasi pada emulator atau perangkat Android (minSdk 28).
5. Pastikan perangkat terhubung ke internet karena data diambil dari REST API.

## Tugas Praktikum 08: Refresh

- `HomeViewModel.refresh()` berjalan di `viewModelScope.launch`.
- Jika query kosong, daftar awal dimuat. Jika query berisi teks, pencarian diulang dengan query aktif.
- `isLoading` dan `errorMessage` diperbarui pada setiap tahap.
- Tombol Refresh di `HomeScreen` dinonaktifkan selama data sedang dimuat.

## Penulis

Zaidan Alvi Zulfan Pramudya (245150200111056)
Program Studi Teknik Informatika, Fakultas Ilmu Komputer, Universitas Brawijaya
