# Hướng dẫn DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [Đọc hướng dẫn kiến trúc kỹ thuật →](GUIDE_vi.md)

> Hướng dẫn cộng đồng đa ngôn ngữ để tìm hiểu, mở rộng và xây dựng plugin cho [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) là agent harness mã nguồn mở do DeepSeek AI phát triển. Ý tưởng cốt lõi là: **mọi thứ đều là plugin**. Bộ điều hợp mô hình, công cụ, vòng lặp agent, lưu trữ phiên, quyền hạn, sandbox, telemetry và giao diện đều có thể được kết hợp hoặc thay thế bằng cấu hình.

> [!IMPORTANT]
> Đây là hướng dẫn cộng đồng độc lập, không phải kho chính thức của DeepSeek. DeepSeek Harness hiện ở giai đoạn Developer Preview và có thể có thay đổi không tương thích. Luôn kiểm tra [kho chính thức](https://github.com/deepseek-ai/deepseek-harness) và [tài liệu chính thức](https://deepseek-harness.github.io/deepseek-harness/).

## Vì sao cần Harness?

Một mô hình riêng lẻ không tự đọc kho mã, chạy lệnh, gọi công cụ, xin phê duyệt, lưu phiên hay phục hồi sau lỗi. Harness cung cấp môi trường vận hành đó và điều phối người dùng, mô hình, công cụ cùng trạng thái ứng dụng.

DeepSeek Harness sử dụng [Cordis](https://github.com/cordiverse/cordis). Plugin đóng góp Service, Event có kiểu và Effect có thể đảo ngược vào Context dùng chung. Nhờ vậy, nhóm có thể thay mô hình, công cụ, sandbox, lưu trữ hoặc subagent mà không cần fork toàn bộ ứng dụng.

## Khái niệm chính

| Khái niệm | Ý nghĩa |
| --- | --- |
| Plugin | Mô-đun TypeScript, đối tượng hoặc lớp Service được gắn vào Cordis Context. |
| Bundle | Gói npm cung cấp một lớp cấu hình qua `dsh.bundle`. |
| Profile | Cấu hình có thể chạy gồm Bundles và dependency cục bộ. |
| Patch | Lớp YAML chèn hoặc thay thế các hàng cấu hình. |
| Service / Event | Năng lực có thể thay thế và điểm mở rộng trong luồng agent. |

Bản thân agent loop cũng có thể thay thế. Vòng lặp mặc định lắp ráp prompt và schema công cụ, stream phản hồi mô hình, chạy công cụ và ghi lại các event phiên bền vững.

## Bắt đầu nhanh

```bash
npx @deepseek-ai/dsh web
```

Web UI mặc định chạy tại `http://127.0.0.1:3080`. Thêm thông tin xác thực trong **Settings → Models**, sau đó chọn workspace.

## Nội dung hướng dẫn

- Cordis, vòng đời plugin, dependency injection và effect có thể đảo ngược.
- Plugin cho công cụ, mô hình, sandbox, lưu trữ, subagent và Web UI.
- Bundles, Profiles, `cordis.patch.yml`, kiểm thử, phát hành và bảo mật.
- Agent Skills dự kiến: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` và `dsh-plugin-review`.

Ở đây, **Skill** là quy trình hướng dẫn có thể tái sử dụng cho agent lập trình, không phải **Plugin** runtime của DeepSeek Harness. Các Skills trên chưa được phát hành.

## Tài nguyên chính thức

- [Mã nguồn](https://github.com/deepseek-ai/deepseek-harness)
- [Kiến trúc](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Plugin đầu tiên](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Đóng gói và cài đặt](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Giấy phép

[MIT](LICENSE)
