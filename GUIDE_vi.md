# Hướng dẫn kỹ thuật DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Hướng dẫn này tham khảo một [bài phân tích kỹ thuật bằng tiếng Trung](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg), sau đó đối chiếu với [mã nguồn chính thức](https://github.com/deepseek-ai/deepseek-harness) và [tài liệu kiến trúc](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness đang ở Developer Preview. Bài viết phân tích các Commit cố định; tên gói, Preset và API nội bộ có thể thay đổi.

## Mô hình trung tâm

DSH duy trì hai hệ thống phối hợp:

- **Đồ thị plugin runtime:** năng lực hiện có, Scope nhìn thấy chúng và Fiber sở hữu vòng đời.
- **Luồng Session Event append-only:** sự kiện bền vững của Agent, được chiếu thành lịch sử mô hình, UI, Resume và Fork.

Agent Loop lấy mô hình, Prompt, công cụ và chính sách từ đồ thị, rồi ghi kết quả vào luồng Event.

## Quy trình cấu thành

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Lớp phía sau thay thế toàn bộ hàng cấu hình theo ID hoặc chèn hàng mới. Bắt đầu chẩn đoán bằng:

```bash
dsh --profile web --dump-config
```

## Cordis Runtime

| Thành phần | Vai trò |
| --- | --- |
| Context | Khả năng nhìn thấy, kế thừa và Realm cô lập của Service. |
| Service | Hợp đồng ổn định giữa Definition, Provider và Consumer. |
| Fiber | Instance Plugin thực với cấu hình, dependency và Disposer. |
| Effect | Gắn resource và Cleanup với Fiber. |
| Event | Mở rộng luồng bằng thông báo, quyết định hoặc Waterfall Middleware. |
| Loader | Biến cấu hình thành cây có thể cập nhật và tháo dỡ. |

`inject` là hợp đồng dependency của Context, không phải quyền hệ điều hành. `ctx.effect()` tổ chức Cleanup nhưng không hoàn tác giao dịch bên ngoài.

## Agent và Session

Turn chứa từ 0 Step trở lên; Step thường gồm một yêu cầu mô hình và các công cụ liên quan. Session Event ghi ranh giới, tin nhắn, Chunk, Tool Call và kết quả. `deriveMessages()` chiếu lịch sử mà mô hình nhìn thấy.

Ghi đầy đủ không có nghĩa gửi lại đầy đủ. Compaction có thể che Surface cũ nhưng vẫn giữ Event gốc. Log có thể phát lại cũng không làm cho tác động bên ngoài an toàn khi lặp lại.

## Cache và bảo mật

Đồ thị động không tự làm mất Prefix Cache. Cache thay đổi khi công cụ, Prompt, mô hình hoặc lịch sử nhìn thấy thay đổi. Giữ thứ tự ổn định và tách dữ liệu biến động.

Plugin bên thứ ba là mã có quyền cao trong tiến trình host. Kiểm tra script cài đặt, Node API, mạng, thông tin xác thực, tệp, subprocess, telemetry và Cleanup; pin một Commit.

## Checklist phát triển

- Dùng Service hoặc Event Seam trước khi sửa Loop.
- Khai báo dependency bằng `inject` và xác thực cấu hình bằng Schema.
- Gán chủ sở hữu và Cleanup cho listener, timer, Service và handle.
- Quyết định state thuộc Host, Agent Scope hay Session Log.
- Kiểm thử Provider swap, update, Unload, Resume, Fork và Compaction.
- Đóng gói thành Bundle và xác minh bằng `--dump-config`.

Xem bản [tiếng Anh](GUIDE.md) hoặc [tiếng Trung](GUIDE_zh.md) để biết đầy đủ chi tiết.

