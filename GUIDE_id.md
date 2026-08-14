# Panduan teknis DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Panduan ini mengacu pada [analisis teknis berbahasa Tionghoa](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg), lalu diperiksa silang dengan [kode resmi](https://github.com/deepseek-ai/deepseek-harness) dan [dokumentasi arsitektur](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness masih dalam Developer Preview. Artikel menganalisis Commit tetap; nama paket, Preset, dan API internal dapat berubah.

## Model utama

DSH memelihara dua sistem yang terkoordinasi:

- **Graf plugin runtime:** kemampuan saat ini, Scope tempat kemampuan terlihat, dan Fiber yang memiliki siklus hidupnya.
- **Aliran append-only Session Event:** fakta persisten dari Agent yang diproyeksikan menjadi riwayat model, UI, Resume, dan Fork.

Agent Loop mengambil model, Prompt, alat, dan kebijakan dari graf, lalu menulis hasil ke aliran event.

## Alur komposisi

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Lapisan berikutnya mengganti seluruh baris berdasarkan ID atau menyisipkan baris baru. Diagnosis pertama:

```bash
dsh --profile web --dump-config
```

## Runtime Cordis

| Elemen | Tanggung jawab |
| --- | --- |
| Context | Visibilitas, pewarisan, dan Realm Service yang terisolasi. |
| Service | Kontrak stabil antara Definition, Provider, dan Consumer. |
| Fiber | Instans Plugin nyata dengan konfigurasi, dependensi, dan Disposer. |
| Effect | Mengaitkan resource dan Cleanup dengan Fiber. |
| Event | Memperluas alur dengan notifikasi, keputusan, atau Waterfall Middleware. |
| Loader | Mengubah konfigurasi menjadi pohon yang dapat diperbarui dan dibongkar. |

`inject` adalah kontrak dependensi Context, bukan izin sistem operasi. `ctx.effect()` mengatur Cleanup tetapi tidak membatalkan transaksi eksternal.

## Agent dan Session

Turn berisi nol atau lebih Step; Step biasanya mencakup permintaan model dan alat terkait. Session Event mencatat batas, pesan, Chunk, Tool Call, dan hasil. `deriveMessages()` memproyeksikan riwayat yang terlihat oleh model.

Pencatatan lengkap tidak berarti pengiriman ulang lengkap. Compaction dapat menyembunyikan Surface lama sambil mempertahankan event. Log yang dapat diputar ulang juga tidak membuat efek eksternal aman untuk diulang.

## Cache dan keamanan

Graf dinamis tidak otomatis membatalkan Prefix Cache. Cache berubah ketika alat, Prompt, model, atau riwayat yang terlihat berubah. Jaga urutan stabil dan pisahkan data volatil.

Plugin pihak ketiga adalah kode berhak tinggi di proses host. Tinjau skrip instalasi, Node API, jaringan, kredensial, file, subprocess, telemetri, dan Cleanup; pin sebuah Commit.

## Checklist pengembangan

- Gunakan Service atau Event Seam sebelum mengubah Loop.
- Nyatakan dependensi dengan `inject` dan validasi konfigurasi dengan Schema.
- Berikan kepemilikan dan Cleanup pada listener, timer, Service, dan handle.
- Tentukan apakah state milik Host, Agent Scope, atau Session Log.
- Uji pergantian Provider, update, Unload, Resume, Fork, dan Compaction.
- Kemas sebagai Bundle dan validasi dengan `--dump-config`.

Lihat versi [Inggris](GUIDE.md) atau [Tionghoa](GUIDE_zh.md) untuk rincian.

