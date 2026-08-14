# Panduan DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

> Panduan komunitas multibahasa untuk memahami, memperluas, dan membangun plugin bagi [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) adalah agent harness sumber terbuka yang dikembangkan oleh DeepSeek AI. Gagasan utamanya: **semuanya adalah plugin**. Adaptor model, alat, agent loop, penyimpanan sesi, izin, sandbox, telemetri, dan antarmuka dapat disusun atau diganti melalui konfigurasi.

> [!IMPORTANT]
> Proyek ini adalah panduan komunitas independen, bukan repositori resmi DeepSeek. DeepSeek Harness masih dalam pratinjau pengembang dan dapat mengalami perubahan yang tidak kompatibel. Selalu periksa [repositori resmi](https://github.com/deepseek-ai/deepseek-harness) dan [dokumentasi resmi](https://deepseek-harness.github.io/deepseek-harness/).

## Mengapa harness dibutuhkan?

Model saja tidak membaca repositori, menjalankan perintah, memanggil alat, meminta persetujuan, menyimpan sesi, atau memulihkan kegagalan. Harness menyediakan lingkungan kerja tersebut dan mengoordinasikan pengguna, model, alat, serta status aplikasi.

DeepSeek Harness ditenagai oleh [Cordis](https://github.com/cordiverse/cordis). Plugin menambahkan service, event bertipe, dan effect yang dapat dibalik ke Context bersama. Dengan demikian, model, alat, sandbox, penyimpanan, atau subagen dapat diganti tanpa melakukan fork seluruh aplikasi.

## Konsep utama

| Konsep | Arti |
| --- | --- |
| Plugin | Modul TypeScript, objek, atau kelas service yang dipasang ke Cordis Context. |
| Bundle | Paket npm yang mengirimkan lapisan konfigurasi melalui `dsh.bundle`. |
| Profile | Komposisi Bundles dan dependensi lokal yang dapat dijalankan. |
| Patch | Lapisan YAML untuk menyisipkan atau mengganti baris konfigurasi. |
| Service / Event | Kemampuan yang dapat diganti dan titik ekstensi pada alur agen. |

Agent loop juga dapat diganti. Loop bawaan menyusun prompt dan skema alat, melakukan streaming respons model, menjalankan alat, dan mencatat event sesi yang persisten.

## Mulai cepat

```bash
npx @deepseek-ai/dsh web
```

Web UI tersedia di `http://127.0.0.1:3080` secara default. Tambahkan kredensial di **Settings → Models**, lalu pilih workspace.

## Isi panduan

- Cordis, siklus hidup plugin, dependency injection, dan effect yang dapat dibalik.
- Plugin alat, model, sandbox, penyimpanan, subagen, dan Web UI.
- Bundles, Profiles, `cordis.patch.yml`, pengujian, publikasi, dan keamanan.
- Agent Skills yang direncanakan: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder`, dan `dsh-plugin-review`.

Di sini, **Skill** adalah alur instruksi yang dapat digunakan kembali oleh agen coding, bukan **Plugin** runtime DeepSeek Harness. Skills tersebut belum dirilis.

## Sumber resmi

- [Kode sumber](https://github.com/deepseek-ai/deepseek-harness)
- [Arsitektur](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Plugin pertama](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Pemaketan dan instalasi](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Lisensi

[MIT](LICENSE)
