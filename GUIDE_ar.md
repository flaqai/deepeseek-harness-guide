# الدليل التقني لـ DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

يستند هذا الدليل إلى [تحليل تقني باللغة الصينية](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg)، وتمت مقارنته مع [الشيفرة الرسمية](https://github.com/deepseek-ai/deepseek-harness) و[وثائق البنية](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> ما زال DeepSeek Harness في مرحلة Developer Preview. يحلل المقال Commits ثابتة، وقد تتغير الحزم وPresets وواجهات API الداخلية.

## النموذج الأساسي

يدير DSH نظامين مترابطين:

- **رسم إضافات وقت التشغيل:** يحدد القدرات الحالية وScope ظهورها وFiber المسؤول عن دورة حياتها.
- **تدفق append-only لأحداث Session:** يسجل حقائق Agent الدائمة ويعرضها كتاريخ للنموذج وUI وResume وFork.

تحصل Agent Loop على النماذج وPrompts والأدوات والسياسات من الرسم، ثم تكتب النتائج في تدفق الأحداث.

## مسار التركيب

`Profile → Bundles → Profile Patch → Home Patch → --patch`

تستبدل الطبقات اللاحقة صف إعداد كاملًا حسب ID أو تضيف صفًا. ابدأ التشخيص بالأمر:

```bash
dsh --profile web --dump-config
```

## وقت تشغيل Cordis

| العنصر | المسؤولية |
| --- | --- |
| Context | رؤية Service والوراثة وRealms المعزولة. |
| Service | عقد ثابت بين Definition وProvider وConsumer. |
| Fiber | نسخة Plugin فعلية مع الإعداد والاعتماديات وDisposers. |
| Effect | يربط الموارد وCleanup بالـ Fiber. |
| Event | يوسع التدفق بالإشعار أو القرار أو Waterfall Middleware. |
| Loader | يحول الإعداد إلى شجرة قابلة للتحديث والإزالة. |

`inject` عقد اعتماد داخل Context وليس صلاحية لنظام التشغيل. يوفر `ctx.effect()` تنظيفًا منظمًا لكنه لا يلغي المعاملات الخارجية.

## Agent وSession

تحتوي Turn على صفر أو أكثر من Steps؛ وعادة تشمل Step طلبًا للنموذج وأدواته. تسجل Session Events الحدود والرسائل وChunks وTool Calls والنتائج. تعرض `deriveMessages()` التاريخ المرئي للنموذج.

التسجيل الكامل لا يعني إعادة الإرسال الكامل. يمكن لـ Compaction إخفاء Surface قديم مع إبقاء الأحداث. كما أن السجل القابل لإعادة العرض لا يجعل الآثار الخارجية آمنة للتكرار.

## الذاكرة المؤقتة والأمان

الرسم الديناميكي لا يبطل Prefix Cache وحده؛ يحدث ذلك عند تغير الأدوات أو Prompt أو النموذج أو التاريخ المرئي. حافظ على ترتيب ثابت واعزل البيانات المتغيرة.

إضافات الطرف الثالث شيفرة ذات صلاحيات عالية داخل عملية المضيف. راجع سكربتات التثبيت وNode API والشبكة والبيانات السرية والملفات والعمليات الفرعية والقياس عن بعد وCleanup، وثبّت Commit محددة.

## قائمة تطوير

- استخدم Service أو Event Seam قبل تعديل Loop.
- أعلن الاعتماديات بـ `inject` وتحقق من الإعداد عبر Schema.
- امنح listeners وtimers وServices وhandles مالكًا وCleanup.
- حدد ما إذا كانت الحالة تخص Host أو Agent Scope أو Session Log.
- اختبر تبديل Provider والتحديث وUnload وResume وFork وCompaction.
- وزع الإضافة كـ Bundle وتحقق باستخدام `--dump-config`.

للتفاصيل راجع النسخة [الإنجليزية](GUIDE.md) أو [الصينية](GUIDE_zh.md).

