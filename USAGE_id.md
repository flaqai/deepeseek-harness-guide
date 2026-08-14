# Panduan penggunaan DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Ini adalah panduan cepat dalam Bahasa Indonesia. DeepSeek Harness masih dalam pratinjau pengembang; cocokkan perintah dengan commit yang digunakan dan dokumentasi resmi.

## Mulai cepat

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Buka `http://127.0.0.1:3080`, atur layanan model, lalu mulai di workspace sementara. Perintah kedua menampilkan pohon plugin hasil gabungan Profile, Bundle, dan Patch.

## Kategori modul

- Komposisi runtime: Context, Service, Fiber, Effect, Event, dan Loader.
- Eksekusi agent: adaptor model, Prompt, Agent Loop, alat, kebijakan, persetujuan, dan sandbox.
- Status: event Session, memori, kompaksi, dan replay.
- Antarmuka: Host, Remote API, Web Client, desktop, TUI, dan seluler.
- Ekosistem: workflow, browser, visi, integrasi, tema, dan alat pengembangan.

## Instalasi aman

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Sematkan commit Git dan tinjau lisensi, skrip instalasi, jaringan, berkas, subproses, kredensial, dan retensi data. Uji startup, penolakan, timeout, unload, restart, dan rollback pada Profile sementara.

## Skills praktis

[`skills/`](skills/) menyediakan empat Agent Skill untuk menjelajahi repositori, membuat plugin, membangun alat, dan meninjau plugin. Skill memandu pekerjaan pengembangan dan bukan plugin DSH Runtime.

Lihat [panduan lengkap berbahasa Inggris](USAGE.md) untuk prosedur, pemecahan masalah, dan checklist rilis.
