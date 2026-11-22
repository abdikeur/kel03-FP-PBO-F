# TULIS_DI_SINI: nama aplikasi

> Baca `SOAL.md` dulu. Proposal dan Laporan Akhir dikumpulkan di Classroom (bukan di repo). Ganti semua `TULIS_DI_SINI` di berkas ini dan berkas catatan lain. Berkas yang masih memuat
> `TULIS_DI_SINI` dianggap belum diisi.

## Tautan

- Paket rilis (GitHub Releases, zip yang dapat langsung dijalankan): TULIS_DI_SINI

## Kelompok

| Nama | NRP | Akun GitHub |
|---|---|---|
| 5025251077 | Bastian Gerry Simangunsong | tianucong |
| 5025251088 | I Komang Bagus Alvero Wisnawa| M4RUT1 |
| 5025251099 | Faris Rashid Azizi | abdikeur |

## Ide dan pembeda

- Ide dalam satu kalimat: TULIS_DI_SINI
- Apa yang membuatnya berbeda dari aplikasi serupa lain: TULIS_DI_SINI

## Untuk guru

- Mata pelajaran dan jenjang: TULIS_DI_SINI
- Tujuan belajar: TULIS_DI_SINI
- Cakupan materi (dan sumber rujukannya): TULIS_DI_SINI
- Cara menjalankan dan memakai: TULIS_DI_SINI

## Rancangan

Jelaskan singkat arsitektur aplikasi dan pola desain yang dipakai (mis. Strategy untuk penilaian). Tempel diagram kelas (Mermaid)
yang cocok dengan kode.


classDiagram
direction TB


class ManajerSesi {
    -ISumberSoal _sumber
    -IPenilaiLangkah _penilai
    -IPenyimpananProgres _simpan
    +SoalPecahan SoalAktif
    +int Skor
    +event SkorBerubah
    +event LevelSelesai
    +event UmpanBalikDiberikan
    +ManajerSesi(ISumberSoal s, IPenilaiLangkah p, IPenyimpananProgres sp)
    +MulaiLevel(int nomor) void
    +IrisBatang(string batang, int faktor) void
    +GabungBatang(string batang, int faktor) void
    +JawabSoal() HasilEvaluasi
    +SoalBerikutnya() void
}

class LevelBelajar {
    +int Nomor
    +string Nama
    +IReadOnlyList~SoalPecahan~ DaftarSoal
    +int JumlahSoal
}

class SoalPecahan {
    <<abstract>>
    +string Id
    +int Level
    +string Petunjuk
    +BatangPecahan BatangA
    #SoalPecahan(string id, int level, BatangPecahan a)
    +Evaluasi(BatangPecahan jawaban) HasilEvaluasi
    +BuatPenjelasan(bool benar) string
    +Nilai(BatangPecahan jawaban, IPenilaiLangkah penilai, int langkah) HasilEvaluasi
    +ToString() string
}

class SoalPenyederhanaan {
    +Evaluasi(BatangPecahan jawaban) HasilEvaluasi
    +BuatPenjelasan(bool benar) string
}

class SoalPenjumlahan {
    +BatangPecahan BatangB
    +Evaluasi(BatangPecahan jawaban) HasilEvaluasi
    +BuatPenjelasan(bool benar) string
}

class SoalPengurangan {
    +BatangPecahan BatangB
    +Evaluasi(BatangPecahan jawaban) HasilEvaluasi
    +BuatPenjelasan(bool benar) string
}

class BatangPecahan {
    +int Pembilang
    +int Penyebut
    +string LabelWarna
    +BatangPecahan(int p, int q)
    +BatangPecahan(int p, int q, string warna)
    +Iris(string faktor) void
    +Gabung(int faktor) void
    +BisaDigabung(int faktor) bool
    +SudahSederhana() bool
    +ToString() string
}


class ISumberSoal {
    <<interface>>
    +Muat() IReadOnlyList~LevelBelajar~
}

class IPenilaiLangkah {
    <<interface>>
    +Nilai(int langkahDipakai, int langkahMinimal) int
}

class IPenyimpananProgres {
    <<interface>>
    +Simpan(ProgresPemain p) void
    +Muat() ProgresPemain
}

class IDisposable {
    <<interface>>
}


class SumberSoalJson
class SumberSoalBawaan

class PenilaiBebas
class PenilaiLangkahMinimal

class PenyimpananJson
class PenyimpananMemori


class ProgresPemain {
    +IReadOnlyDictionary~int,int~ BintangPerLevel
    +int SkorTertinggi
}

class PerekamRiwayatLatihan {
    -StreamWriter _tulis
    +Catat(string baris) void
    +Dispose() void
}


class HasilEvaluasi {
    +bool Benar
    +string Pesan
    +int Poin
}


class Exception

class PotonganBelumSamaException
class PecahanTidakValidException
class PenyederhanaanTidakMungkinException


ManajerSesi "1" *-- "0..*" LevelBelajar : memiliki
LevelBelajar "1" *-- "10" SoalPecahan : memiliki

SoalPecahan "1" *-- "1..2" BatangPecahan : memiliki

ManajerSesi ..> HasilEvaluasi : menerima
SoalPecahan ..> HasilEvaluasi : mengembalikan


SoalPenyederhanaan --|> SoalPecahan
SoalPenjumlahan --|> SoalPecahan
SoalPengurangan --|> SoalPecahan


SumberSoalJson ..|> ISumberSoal
SumberSoalBawaan ..|> ISumberSoal

PenilaiBebas ..|> IPenilaiLangkah
PenilaiLangkahMinimal ..|> IPenilaiLangkah

PenyimpananJson ..|> IPenyimpananProgres
PenyimpananMemori ..|> IPenyimpananProgres

PerekamRiwayatLatihan ..|> IDisposable

ManajerSesi ..> ISumberSoal : DI
ManajerSesi ..> IPenilaiLangkah : DI
ManajerSesi ..> IPenyimpananProgres : DI

ManajerSesi ..> ProgresPemain : simpan/muat
ManajerSesi ..> PerekamRiwayatLatihan : using


PotonganBelumSamaException --|> Exception
PecahanTidakValidException --|> Exception
PenyederhanaanTidakMungkinException --|> Exception

SoalPenjumlahan ..> PotonganBelumSamaException : throw
SoalPengurangan ..> PotonganBelumSamaException : throw

BatangPecahan ..> PecahanTidakValidException : throw
BatangPecahan ..> PenyederhanaanTidakMungkinException : throw


TULIS_DI_SINI

## Cara menjalankan (untuk pengembang)

```bash
dotnet run --project src/Aplikasi     # menjalankan aplikasi
```

## Membuat paket rilis

```bash
dotnet publish src/Aplikasi -c Release -r win-x64 --self-contained true -o terbit
```

Kompres folder `terbit` menjadi zip, lalu unggah ke GitHub Releases di repo ini (tab Releases > Draft a new release).
Salin tautannya ke bagian **Tautan** di atas. Uji paket itu di komputer lain sebelum pos berikutnya.

## Status persyaratan

Isi dengan `selesai`, `sebagian`, atau `belum`.

| ID | Persyaratan | Status | Catatan |
|---|---|---|---|
| P1-P7 | Produk (ide, pedagogi, dapat dijalankan, progres, etika, uji coba, panduan) | TULIS_DI_SINI | |
| K1-K5 | Enkapsulasi, komposisi/pewarisan, polimorfisme, abstract/interface/DI, exception/life cycle | TULIS_DI_SINI | |
| K6 | Event | TULIS_DI_SINI | |
| K7 | Logika murni | TULIS_DI_SINI | |

## Yang belum selesai

TULIS_DI_SINI (tulis "tidak ada" bila semuanya selesai)
