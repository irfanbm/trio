# TRIO

Panggilan tunggal untuk tiga paket skill agen AI: **video + anti-slop**.

Cukup bilang **"pakai trio"** / **"mode trio"** ke agen, dan dia akan me-routing ke skill yang tepat:

| Paket | Skill | Dipakai untuk |
|---|---|---|
| **HyperFrames** | `hyperframes` (router, baca pertama), `hyperframes-core`, `hyperframes-animation`, `hyperframes-cli`, `hyperframes-creative`, `hyperframes-keyframes`, `hyperframes-audio`, `hyperframes-registry`, `hyperframes-studio`, `media-use`, `general-video`, `product-launch-video`, `faceless-explainer`, `pr-to-video`, `embedded-captions`, `talking-head-recut`, `motion-graphics`, `music-to-video`, `remotion-to-hyperframes`, `slideshow`, `figma` | Render video dari HTML, promo/produk, explainer, caption, slideshow, port Remotion, Figma → video |
| **OneTake** | `onetake` (+ `lib/`, `scripts/`, `references/`, `templates/`, `looks/`, `cases/`) | Film motion produk 10–60 detik yang *continuous* / one-take, anti-slideshow |
| **Anti-Slop** | `antislop` (core, selalu aktif) + `antislop-ui`, `antislop-copywriting`, `antislop-human`, `antislop-layoutmobile`, `antislop-code` | Filter UI / copy / code generik khas AI — termasuk teks & tampilan di dalam video |

Skill payung `skills/trio/SKILL.md` berisi aturan routing, template perintah VO, serta alur pencocokan aset eksternal. Salin folder `skills/trio/` ke direktori skills agen agar perintah "pakai trio" tersedia.

## Cara install

Salin semua folder di repo ini ke folder skills agen AI kamu, misalnya:

```bash
git clone https://github.com/<username>/trio.git
cp -r trio/* ~/workspace/skills/     # atau ~/.muse/skills/, ~/.codex/skills/, dst.
```

HyperFrames CLI tidak perlu diinstall global — dipanggil on-demand via `npx hyperframes ...` sesuai `hyperframes-cli/SKILL.md`.

## Cara update

Tarik ulang dari repo sumbernya dan timpa folder terkait:

- HyperFrames → `heygen-com/hyperframes`, folder `skills/`
- OneTake → `feitangyuan/onetake`, root repo
- Anti-Slop → `miqdadbadjuber/anti-slop`, folder `skills/`

Skill `skills/trio/` dikelola di repo ini — perubahan pada file tersebut ikut tersedia saat repo diperbarui.

## Sumber & lisensi

| Paket | Sumber | Lisensi |
|---|---|---|
| HyperFrames | [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) | Apache-2.0 (`LICENSES/hyperframes-Apache-2.0.txt`) |
| OneTake | [feitangyuan/onetake](https://github.com/feitangyuan/onetake) | **PolyForm Noncommercial 1.0.0** — gratis hanya untuk non-komersial (`onetake/LICENSE`) |
| Anti-Slop | [miqdadbadjuber/anti-slop](https://github.com/miqdadbadjuber/anti-slop) | MIT (`LICENSES/anti-slop-MIT.txt`) |

Atribusi lengkap: [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

> ⚠️ OneTake **tidak boleh dipakai untuk proyek komersial**. Ingatkan agen bila proyekmu komersial.
