---
name: trio
description: "Router payung untuk HyperFrames, OneTake, dan Anti-Slop. Baca file ini pertama saat pengguna menyebut trio atau meminta video dengan workflow Trio."
---

# Trio

Panggilan tunggal untuk tiga paket skill agen AI: **video + anti-slop**. Baca file ini terlebih dahulu saat pengguna berkata "pakai trio", "mode trio", atau menyebut Trio.

## Urutan Eksekusi

1. Baca skill ini sebelum memilih workflow.
2. Anti-Slop menemani seluruh sesi, bukan hanya audit terakhir. Tanyakan mode DURING atau AFTER bila belum ditentukan. Jika template perintah cepat di bawah dipakai tanpa jawaban mode, gunakan DURING + AFTER sebagai default.
3. Pilih tepat satu engine video. HyperFrames adalah default; pilih OneTake hanya untuk permintaan one-take/continuous/anti-slideshow yang eksplisit. Jangan jalankan dua engine render untuk deliverable yang sama.
4. Pilih router HyperFrames yang cocok, lalu load hanya domain skills yang workflow itu perlukan.
5. Terapkan filter anti-slop pada keputusan visual, motion, copy, teks/caption di video, dan kode komposisi.
6. Selesaikan pemeriksaan dan Delivery Gate sebelum mengklaim video siap.

## Routing

| Permintaan | Engine dan skill |
|---|---|
| Video dari VO/topic/article tanpa capture website | HyperFrames: `hyperframes` dahulu → `faceless-explainer` → domain skills sesuai kebutuhan |
| Promo atau showcase website/produk | HyperFrames: `hyperframes` dahulu → `product-launch-video` |
| Talking-head dengan caption | HyperFrames: `hyperframes` dahulu → `embedded-captions` |
| Talking-head dengan graphic overlays | HyperFrames: `hyperframes` dahulu → `talking-head-recut` |
| Musik menjadi video yang beat-driven | HyperFrames: `hyperframes` dahulu → `music-to-video` |
| Gerak grafis singkat tanpa narasi | HyperFrames: `hyperframes` dahulu → `motion-graphics` |
| Film produk 10–60 detik yang diminta continuous/one-take/anti-slideshow | OneTake: `onetake` |
| Breakdown draft-vs-final di NullMotion | `nullmotion`; film final harus sudah dirender dahulu |
| Hanya filter UI/copy/code | Anti-Slop saja, tanpa engine video |

### Domain Skills HyperFrames

- `hyperframes-core`: struktur komposisi, timing, track, media, determinisme.
- `hyperframes-creative`: arah visual, storyboard, tipografi, palette, narasi.
- `hyperframes-animation`: choreography dan runtime motion.
- `hyperframes-keyframes`: camera move, reframe, path, mask, keyframe.
- `hyperframes-cli`: init, check, preview, render, diagnostics.
- `media-use`: transkripsi, caption, audio, gambar, logo, dan sourcing media.
- `hyperframes-audio`: mixing, volume, ducking, effects pada audio yang sudah ditempatkan.
- `hyperframes-registry`: cari/install block bila brief menyebut treatment bernama.
- `hyperframes-studio`: project Studio dan review flow.

## Template Perintah Cepat: Video dari VO

Isi empat field wajib. Isi `PATH_ASET` hanya bila `ASET_EKSTERNAL: ya`.

```text
Pakai trio. Buat video baru dari VO ini.

PATH_VO: [lokasi file audio]
STYLE: [nama style; warna aksen bila ditentukan]
RATIO: [9:16 / 16:9 / 1:1]
OUTPUT: [draft / final]
CAPTION: [off / on]
ASET_EKSTERNAL: [tidak / ya]
PATH_ASET: [wajib jika ASET_EKSTERNAL=ya; path folder gambar/screenshot]

Default:
- Gunakan HyperFrames faceless-explainer. Pertahankan VO asli dan ikuti isi serta bahasa yang benar-benar terdengar.
- Periksa durasi, bahasa, dan kondisi audio; transkripsikan VO dengan word-level timestamps sebelum menulis storyboard.
- Jika transkripsi gagal, jangan menyimpulkan isi dari nama file, jangan membuat timing/caption seolah sudah terverifikasi, dan jangan membangun visual final. Beri tahu pengguna penyebab serta opsi berikutnya.
- Default subtitle/caption adalah OFF. Buat hanya jika `CAPTION: on` diminta secara eksplisit.
- Jika caption diminta, selaraskan storyboard, reveal visual, dan caption ke timestamp VO. Caption kata demi kata hanya dibuat bila timestamp tersedia dan tervalidasi. Jika timestamp tidak tersedia, berhenti dan jelaskan kendala; jangan mengarang sinkronisasi.
- ASET_EKSTERNAL=tidak: buat visual full motion dari tipografi, diagram, grafis, dan animasi yang relevan dengan VO.
- ASET_EKSTERNAL=ya: inventaris dan cocokkan aset dari PATH_ASET dengan isi VO/storyboard; gunakan aset yang relevan dan isi beat tanpa aset dengan full motion. Jangan mewajibkan setiap gambar tampil.
- Jangan mengubah file aset sumber. Salin/adopt hanya aset yang dipilih ke project dan pertahankan nama/provenance yang jelas.
- Style membingkai aset, bukan ditimpa oleh aset. Terapkan palette, tipografi, crop, hierarchy, dan motion yang diminta secara konsisten.
- Jangan menebak kecocokan hanya dari nama file. Periksa isi visual bila kemampuan vision tersedia. Jika tidak bisa melihat gambar, katakan terus terang dan minta manifest/deskripsi atau konfirmasi pemetaan sebelum memakai aset yang ambigu.
- Tampilkan ringkasan pemetaan `file -> beat/scene` sebelum build. Tanyakan hanya untuk kecocokan ambigu, aset penting yang belum terpetakan, atau keputusan yang tidak bisa disimpulkan dengan aman. Laporkan aset yang tidak dipakai dan alasannya.
- Tanpa BGM kecuali diminta. Jangan menambahkan statistik, testimonial, klaim performa, atau fakta yang tidak didukung VO atau aset terverifikasi.
- Simpan project baru di `videos/<nama-file-audio>/`. Jika folder/project sudah ada, jangan menimpa; pilih nama folder unik dan laporkan lokasinya.
- Terapkan Anti-Slop selama pembuatan dan lakukan audit akhir.

OUTPUT=draft: jalankan check, buat draft dan preview untuk ditinjau. Jangan menyebut draft sebagai final.
OUTPUT=final: siapkan kualitas render final, tetapi tetap jalankan check, inspeksi snapshot/contact sheet, buka preview, dan minta persetujuan eksplisit untuk render final. Kata "final" tidak melewati gerbang persetujuan HyperFrames.
```

### Contoh: Full Motion

```text
Pakai trio. Buat video baru dari VO ini.

PATH_VO: M:\Videos\penjelasan-cache.mp3
STYLE: Swiss Pulse, aksen hijau #15A161
RATIO: 9:16
OUTPUT: draft
ASET_EKSTERNAL: tidak
CAPTION: off
```

### Contoh: Memakai Screenshot dan Foto

```text
Pakai trio. Buat video baru dari VO ini.

PATH_VO: M:\Videos\penjelasan-cache.mp3
STYLE: Swiss Pulse, aksen hijau #15A161
RATIO: 9:16
OUTPUT: final
ASET_EKSTERNAL: ya
PATH_ASET: M:\Videos\penjelasan-cache\assets
CAPTION: on
HINDARI: jangan pakai foto stok atau data yang tidak ada sumbernya
```

### Perilaku Aset Eksternal

1. Daftar file gambar/video di folder, catat nama, format, dimensi/durasi, lalu baca metadata yang relevan. Folder dapat berisi subfolder; inventarisasi secara rekursif.
2. Cocokkan nama dan isi visual aset dengan VO, storyboard, serta scene yang tepat. Nama file adalah petunjuk, bukan bukti isi.
3. Sebelum build, sampaikan tabel ringkas `aset | scene/beat | alasan cocok | confidence`. Pemilihan yang tidak ambigu boleh diteruskan sesuai mode workflow. Tahan dan minta konfirmasi untuk identitas, konteks, atau kecocokan yang ambigu.
4. Jangan membuat orang, lokasi, produk, data, atau peristiwa dalam gambar seolah diketahui jika tidak dapat diverifikasi. Jangan mengubah foto/screenshot dengan treatment yang mengubah makna tanpa persetujuan.
5. Aset tidak cocok, rusak, duplikat, atau tidak dipakai dilaporkan; jangan hapus atau ubah sumbernya.
6. Saat aset nyata digunakan, lakukan satu grounded media-polish scan. Jangan menerapkan grade/filter generik secara diam-diam.

## Review dan Persetujuan

- Untuk proyek baru, jalankan intent layer HyperFrames dan catat brief di project. Template menetapkan maksud rutin, tetapi tidak menghapus pertanyaan yang diperlukan untuk menyelesaikan ambiguitas atau konflik.
- Ikuti storyboard/review gates workflow yang dipilih. Bila workflow meminta storyboard atau sketch approval, jangan menganggap `OUTPUT=final` sebagai persetujuan storyboard.
- Sebelum final render, HyperFrames mewajibkan check, snapshot/preview, dan persetujuan render yang eksplisit. Jangan render final hanya karena check lolos.
- Jika snapshot tidak dapat dibaca oleh agent, laporkan batasan inspeksi; jangan mengklaim pemeriksaan visual sudah dilakukan.

## Anti-Slop Delivery Gate

- DURING: semua keputusan warna, tipografi, komposisi, motion, caption, dan copy punya tujuan yang jelas.
- AFTER: audit teks (tanpa em dash buatan agen dan tanpa buzzword/klaim palsu), kontras, keterbacaan mobile/rasio target, pacing, identitas visual, dan aset yang benar-benar cocok.
- Jalankan pemeriksaan HyperFrames yang sesuai (`npx hyperframes check`; snapshot saat dibutuhkan), perbaiki error dan temuan yang memengaruhi hasil.
- Subtitle/caption default OFF; bila `CAPTION: on`, pastikan teks bersumber dari transkrip dan sinkron dengan timestamp.
- Laporan akhir menyebut path render, durasi, dimensi/fps, hasil check, apakah preview disetujui, serta batasan seperti caption yang dilewati atau aset ambigu.

## CLI HyperFrames

Tidak perlu install global. Gunakan `npx hyperframes <command>` dan ikuti kontrak `hyperframes-cli`.

## Lisensi

OneTake dilisensikan PolyForm Noncommercial 1.0.0 dan tidak boleh digunakan untuk proyek komersial. Untuk proyek komersial, arahkan ke HyperFrames (Apache-2.0).
