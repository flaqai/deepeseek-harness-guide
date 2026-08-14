# Hướng dẫn sử dụng DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Đây là hướng dẫn nhanh bằng tiếng Việt. DeepSeek Harness vẫn ở giai đoạn Developer Preview; hãy đối chiếu lệnh với commit đang triển khai và tài liệu chính thức.

## Bắt đầu nhanh

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Mở `http://127.0.0.1:3080`, cấu hình dịch vụ mô hình và thử trước trong workspace tạm thời. Lệnh thứ hai hiển thị cây plugin được kết hợp từ Profile, Bundle và Patch.

## Nhóm mô-đun

- Kết hợp runtime: Context, Service, Fiber, Effect, Event và Loader.
- Thực thi agent: bộ điều hợp mô hình, Prompt, Agent Loop, công cụ, chính sách, phê duyệt và sandbox.
- Trạng thái: sự kiện Session, bộ nhớ, nén và phát lại.
- Giao diện: Host, Remote API, Web Client, desktop, TUI và di động.
- Hệ sinh thái: workflow, trình duyệt, thị giác, tích hợp, giao diện và công cụ phát triển.

## Cài đặt an toàn

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Ghim Git commit và kiểm tra giấy phép, script cài đặt, mạng, tệp, tiến trình con, thông tin bí mật và lưu giữ dữ liệu. Kiểm thử khởi động, từ chối, timeout, unload, khởi động lại và rollback trong Profile tạm thời.

## Skills thực dụng

[`skills/`](skills/) có bốn Agent Skill để khám phá kho mã, tạo khung plugin, phát triển công cụ và rà soát plugin. Skill hướng dẫn công việc phát triển, không phải plugin của DSH Runtime.

Xem quy trình đầy đủ, xử lý sự cố và checklist phát hành trong [sổ tay tiếng Anh](USAGE.md).
