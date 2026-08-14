# คู่มือ DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [อ่านคู่มือสถาปัตยกรรมเชิงเทคนิค →](GUIDE_th.md)

> คู่มือหลายภาษาที่ดูแลโดยชุมชนสำหรับทำความเข้าใจ ขยายความสามารถ และสร้างปลั๊กอินให้ [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)

DeepSeek Harness (`dsh`) คือ agent harness แบบโอเพนซอร์สที่พัฒนาโดย DeepSeek AI แนวคิดหลักคือ **ทุกอย่างเป็นปลั๊กอิน** ทั้งตัวเชื่อมต่อโมเดล เครื่องมือ วงจรของเอเจนต์ การเก็บเซสชัน สิทธิ์ แซนด์บ็อกซ์ เทเลเมทรี และส่วนติดต่อผู้ใช้ สามารถประกอบหรือแทนที่ผ่านการกำหนดค่าได้

> [!IMPORTANT]
> โปรเจกต์นี้เป็นคู่มือชุมชนอิสระ ไม่ใช่คลังอย่างเป็นทางการของ DeepSeek ขณะนี้ DeepSeek Harness ยังอยู่ในช่วง Developer Preview และอาจมีการเปลี่ยนแปลงที่ไม่เข้ากัน โปรดตรวจสอบกับ[คลังอย่างเป็นทางการ](https://github.com/deepseek-ai/deepseek-harness)และ[เอกสารอย่างเป็นทางการ](https://deepseek-harness.github.io/deepseek-harness/)เสมอ

## ทำไมต้องมี Harness

โมเดลเพียงอย่างเดียวไม่สามารถอ่านคลังโค้ด รันคำสั่ง เรียกเครื่องมือ ขออนุมัติ เก็บเซสชัน หรือกู้คืนจากข้อผิดพลาดได้ Harness จัดเตรียมสภาพแวดล้อมนี้และประสานผู้ใช้ โมเดล เครื่องมือ และสถานะของแอปพลิเคชัน

DeepSeek Harness สร้างบน [Cordis](https://github.com/cordiverse/cordis) ปลั๊กอินเพิ่ม Service, Event ที่มีชนิดข้อมูล และ Effect ที่ย้อนกลับได้ลงใน Context ร่วม จึงเปลี่ยนโมเดล เครื่องมือ แซนด์บ็อกซ์ พื้นที่จัดเก็บ หรือเอเจนต์ย่อยได้โดยไม่ต้อง fork ทั้งแอปพลิเคชัน

## แนวคิดหลัก

| แนวคิด | ความหมาย |
| --- | --- |
| Plugin | โมดูล TypeScript อ็อบเจ็กต์ หรือคลาส Service ที่ติดตั้งใน Cordis Context |
| Bundle | แพ็กเกจ npm ที่เผยแพร่ชั้นการกำหนดค่าผ่าน `dsh.bundle` |
| Profile | ชุดประกอบที่รันได้จาก Bundles และ dependency ภายในเครื่อง |
| Patch | ชั้น YAML ที่เพิ่มหรือแทนที่แถวการกำหนดค่า |
| Service / Event | ความสามารถที่สับเปลี่ยนได้และจุดขยายในลำดับงานของเอเจนต์ |

แม้แต่ agent loop ก็แทนที่ได้ ลูปมาตรฐานจะประกอบ prompt และ schema ของเครื่องมือ สตรีมคำตอบจากโมเดล รันเครื่องมือ และบันทึก event ของเซสชัน

## เริ่มต้นอย่างรวดเร็ว

```bash
npx @deepseek-ai/dsh web
```

Web UI เปิดที่ `http://127.0.0.1:3080` โดยค่าเริ่มต้น เพิ่มข้อมูลรับรองใน **Settings → Models** แล้วเลือก workspace

## เนื้อหาของคู่มือ

- Cordis, วงจรชีวิตปลั๊กอิน, dependency injection และ effect ที่ย้อนกลับได้
- ปลั๊กอินสำหรับเครื่องมือ โมเดล แซนด์บ็อกซ์ พื้นที่จัดเก็บ เอเจนต์ย่อย และ Web UI
- Bundles, Profiles, `cordis.patch.yml`, การทดสอบ การเผยแพร่ และความปลอดภัย
- Agent Skills ที่วางแผนไว้: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` และ `dsh-plugin-review`

**Skill** ในที่นี้หมายถึงขั้นตอนการทำงานที่ใช้ซ้ำได้สำหรับเอเจนต์เขียนโค้ด ไม่ใช่ **Plugin** ที่ทำงานใน DeepSeek Harness โดย Skills ข้างต้นยังไม่เผยแพร่

## แหล่งข้อมูลอย่างเป็นทางการ

- [ซอร์สโค้ด](https://github.com/deepseek-ai/deepseek-harness)
- [สถาปัตยกรรม](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [ปลั๊กอินแรก](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [การแพ็กและติดตั้ง](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## สัญญาอนุญาต

[MIT](LICENSE)
