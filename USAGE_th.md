# คู่มือการใช้งาน DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

หน้านี้เป็นคู่มือฉบับย่อภาษาไทย DeepSeek Harness ยังอยู่ในช่วง Developer Preview จึงควรตรวจสอบคำสั่งกับ commit ที่ใช้งานและเอกสารทางการ

## เริ่มต้นอย่างรวดเร็ว

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

เปิด `http://127.0.0.1:3080` ตั้งค่าบริการโมเดล และทดลองใน workspace ชั่วคราวก่อน คำสั่งที่สองจะแสดงผังปลั๊กอินที่ประกอบจาก Profile, Bundle และ Patch

## หมวดหมู่โมดูล

- การประกอบ Runtime: Context, Service, Fiber, Effect, Event และ Loader
- การทำงานของ Agent: model adapter, Prompt, Agent Loop, เครื่องมือ, policy, การอนุมัติ และ sandbox
- สถานะ: Session Event, หน่วยความจำ, การย่อ และ replay
- ส่วนติดต่อ: Host, Remote API, Web Client, desktop, TUI และ mobile
- ระบบนิเวศ: workflow, browser, vision, integration, theme และเครื่องมือพัฒนา

## การติดตั้งอย่างปลอดภัย

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

ตรึง Git commit และตรวจสอบใบอนุญาต สคริปต์ติดตั้ง เครือข่าย ไฟล์ subprocess ข้อมูลลับ และการเก็บข้อมูล ทดสอบการเริ่ม การปฏิเสธ timeout การ unload การ restart และ rollback ใน Profile ชั่วคราว

## Skills ที่ใช้งานได้

[`skills/`](skills/) มี Agent Skill สี่รายการสำหรับสำรวจ repository สร้างโครงปลั๊กอิน พัฒนาเครื่องมือ และตรวจสอบปลั๊กอิน Skill ใช้กำกับงานพัฒนา ไม่ใช่ปลั๊กอินของ DSH Runtime

ดูขั้นตอน การแก้ปัญหา และรายการตรวจสอบการเผยแพร่ทั้งหมดใน [คู่มือภาษาอังกฤษ](USAGE.md)
