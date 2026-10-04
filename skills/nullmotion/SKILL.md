---
name: nullmotion
description: "Spesialisasi HyperFrames: pakai aplikasi lokal NullMotion (blixvip/NullMotion) untuk breakdown iklan — final film di atas, draft HyperFrames hitam-putih per section di bawah, sinkron frame, export MP4 whole preview. Aktif saat pengguna menyebut 'NullMotion', 'breakdown draft vs final', 'film + draft per section', atau ingin meng-import footage sendiri ke NullMotion. Video baru yang belum dirender → render dulu via /hyperframes."
---

# nullmotion — spesialisasi HyperFrames

> **Front door tetap `/hyperframes`.** Skill ini hanya untuk **menggunakan aplikasi NullMotion secara lokal**: melihat final film ↔ draft per section secara sinkron, lalu mengekspor breakdown-nya sebagai MP4. NullMotion bukan engine render — film yang di-breakdown harus sudah jadi (render HyperFrames atau footage yang tersedia).

## Kapan dipakai

- Ingin melihat **draft vs final**: film final di atas, draft hitam-putih per section (Hook, Reveal, App, End card, …) di bawah, playhead sinkron frame.
- Ingin **meng-import footage sendiri** (video yang sudah jadi), menyusun draft per section, lalu mengekspor breakdown-nya jadi MP4.
- **Bukan** untuk: render video baru (→ `/hyperframes`), VO/TTS/transkrip (→ `media-use`), atau editing footage mentah.

## Persiapan

| Kebutuhan | Untuk | Catatan |
|---|---|---|
| Node.js 22+ | server lokal | tanpa `npm install` — tidak ada dependensi runtime |
| Chrome atau Edge | export MP4 | WebCodecs + `requestVideoFrameCallback` |
| FFmpeg + ffprobe | import referensi, section planning | wajib mulai tahap import |
| Python 3 | `scripts/import-motionclone.py` | wajib mulai tahap import |

## Alur kerja

1. **Clone & jalankan** (sekali saja — simpan di **drive yang sama** dengan video target, karena importer memakai hard-link):

   ```
   git clone https://github.com/blixvip/NullMotion.git
   cd NullMotion && npm start        # http://127.0.0.1:4343 (ubah dengan PORT/HOST)
   ```

2. **Import referensi** (video final, mis. hasil render HyperFrames):

   ```
   python scripts/import-motionclone.py --source /path/to/video
   node scripts/plan-sections.mjs     # sections + scene cuts + contact sheets
   ```

   Media masuk ke `.local-media/` dan `public/references/` (di-ignore Git). Bila struktur input tidak cocok dengan skrip importer, taruh video manual di `public/references/` lalu tulis `drafts.json` sesuai README.

3. **Tulis draft** per section di `public/references/drafts.json` — satu section = satu draft 640×360, HTML scene di paused GSAP timeline. Beat kinds: `text`, `logo`, `phone`, `window`, `input`, `chat`, `cards`, `list`, `chart`, `cloud`. Tulis sesuai layout, teks, dan timing frame asli; bagian tanpa deskripsi otomatis dapat draft placeholder.

4. **Review** di `127.0.0.1:4343` — film final di atas, draft section aktif mendapat outline oranye dan mengikuti playhead, draft lain loop sendiri.

5. **Export**: `Download → Whole preview` — render frame-by-frame 24 Mbps + audio film (bukan rekaman real-time, tidak ada frame yang jatuh).

## Batasan (dari AGENTS.md NullMotion)

- **`showcase/` jangan ditambah reference** tanpa pilihan eksplisit owner — hanya tiga reference bawaan yang boleh terbit.
- Media impor tetap di `.local-media/` + `public/references/` (di-ignore Git) — **jangan di-commit**; source video bersifat read-only dan non-destruktif.
- Preview = komposisi browser, **bukan AI generation** — jangan mengaku sebaliknya.
- **Jangan menambahkan lisensi** ke repo NullMotion — keputusan owner.
- Browser hanya boleh memanggil origin proyek ini sendiri; jangan menyalin kredensial, database, render, atau pengaturan provider pihak lain.

## Kualitas

```
npm run check   # cek file UI
npm test        # node --test (server, media, editor)
```

## Lisensi

`blixvip/NullMotion` **tidak memiliki lisensi proyek** (all-rights-reserved):

- **Jangan menyalin atau menyisipkan kode NullMotion** ke repo lain — cukup dipakai sebagai aplikasi lokal.
- Skill ini tulisan asli + tautan; tidak ada kode NullMotion di dalamnya.
- GSAP & mp4-muxer yang dibundel punya lisensi sendiri — lihat `THIRD_PARTY_NOTICES.md` di repo NullMotion.
