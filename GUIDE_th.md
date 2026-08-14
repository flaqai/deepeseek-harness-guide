# คู่มือเชิงเทคนิค DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

คู่มือนี้อ้างอิง[บทวิเคราะห์ภาษาจีน](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg) และตรวจสอบกับ[ซอร์สอย่างเป็นทางการ](https://github.com/deepseek-ai/deepseek-harness)และ[เอกสารสถาปัตยกรรม](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)

> DeepSeek Harness ยังอยู่ใน Developer Preview บทความวิเคราะห์ Commit แบบตายตัว ชื่อแพ็กเกจ Preset และ API ภายในอาจเปลี่ยนได้

## แบบจำลองหลัก

DSH ดูแลสองระบบที่ทำงานร่วมกัน:

- **กราฟปลั๊กอินขณะทำงาน:** แสดงความสามารถปัจจุบัน Scope ที่มองเห็น และ Fiber ที่เป็นเจ้าของวงจรชีวิต
- **สตรีม Session Event แบบ append-only:** บันทึกข้อเท็จจริงถาวรของ Agent และฉายไปเป็นประวัติโมเดล UI, Resume และ Fork

Agent Loop รับโมเดล Prompt เครื่องมือ และนโยบายจากกราฟ แล้วเขียนผลลัพธ์กลับสู่สตรีม Event

## ลำดับการประกอบ

`Profile → Bundles → Profile Patch → Home Patch → --patch`

ชั้นหลังจะแทนที่แถวการกำหนดค่าทั้งแถวตาม ID หรือเพิ่มแถวใหม่ เริ่มวิเคราะห์ด้วย:

```bash
dsh --profile web --dump-config
```

## Cordis Runtime

| องค์ประกอบ | หน้าที่ |
| --- | --- |
| Context | กำหนดการมองเห็น การสืบทอด และ Realm ที่แยกของ Service |
| Service | สัญญาที่เสถียรระหว่าง Definition, Provider และ Consumer |
| Fiber | อินสแตนซ์ Plugin จริง พร้อมการกำหนดค่า dependency และ Disposer |
| Effect | ผูก resource และ Cleanup เข้ากับ Fiber |
| Event | ขยายลำดับงานด้วยการแจ้งเตือน การตัดสินใจ หรือ Waterfall Middleware |
| Loader | เปลี่ยนการกำหนดค่าเป็นต้นไม้ที่อัปเดตและถอดได้ |

`inject` เป็นสัญญา dependency ของ Context ไม่ใช่สิทธิ์ระบบปฏิบัติการ `ctx.effect()` จัดโครงสร้าง Cleanup แต่ไม่ย้อนธุรกรรมภายนอก

## Agent และ Session

Turn มี Step ตั้งแต่ศูนย์ขึ้นไป ส่วน Step มักรวมคำขอโมเดลและการเรียกเครื่องมือ Session Event บันทึกขอบเขต ข้อความ Chunk, Tool Call และ Result ส่วน `deriveMessages()` ฉายประวัติที่โมเดลมองเห็น

การบันทึกครบไม่เท่ากับส่งซ้ำทั้งหมด Compaction ซ่อน Surface เก่าได้โดยยังเก็บ Event ต้นฉบับ และ Log ที่เล่นซ้ำได้ไม่ได้ทำให้ผลข้างเคียงภายนอกปลอดภัยเมื่อรันซ้ำ

## Cache และความปลอดภัย

กราฟแบบไดนามิกไม่ทำให้ Prefix Cache ใช้ไม่ได้เอง Cache เปลี่ยนเมื่อเครื่องมือ Prompt โมเดล หรือประวัติที่มองเห็นเปลี่ยน รักษาลำดับให้คงที่และแยกข้อมูลที่เปลี่ยนบ่อย

Plugin ภายนอกเป็นโค้ดสิทธิ์สูงใน process หลัก ตรวจสอบสคริปต์ติดตั้ง Node API เครือข่าย ข้อมูลลับ ไฟล์ subprocess เทเลเมทรี และ Cleanup พร้อม pin Commit

## รายการตรวจสอบ

- ใช้ Service หรือ Event Seam ก่อนแก้ Loop
- ประกาศ dependency ด้วย `inject` และตรวจการกำหนดค่าด้วย Schema
- กำหนดเจ้าของและ Cleanup ให้ listener, timer, Service และ handle
- ตัดสินใจว่า state อยู่ที่ Host, Agent Scope หรือ Session Log
- ทดสอบ Provider swap, update, Unload, Resume, Fork และ Compaction
- แพ็กเป็น Bundle และตรวจด้วย `--dump-config`

อ่านรายละเอียดได้จากฉบับ[อังกฤษ](GUIDE.md)หรือ[จีน](GUIDE_zh.md)

