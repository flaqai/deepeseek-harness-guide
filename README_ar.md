# دليل DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [اقرأ دليل البنية التقني →](GUIDE_ar.md)

> دليل مجتمعي متعدد اللغات لفهم [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) وتوسيعه وبناء الإضافات له.

DeepSeek Harness (`dsh`) هو إطار تشغيل مفتوح المصدر للوكلاء طورته DeepSeek AI. فكرته الأساسية هي: **كل شيء إضافة**. يمكن تركيب أو استبدال موصلات النماذج والأدوات وحلقة الوكيل وتخزين الجلسات والصلاحيات وبيئة العزل والقياس عن بعد وواجهة المستخدم عبر الإعدادات.

> [!IMPORTANT]
> هذا دليل مجتمعي مستقل وليس مستودعًا رسميًا تابعًا لـ DeepSeek. لا يزال DeepSeek Harness في مرحلة المعاينة للمطورين وقد تحدث تغييرات غير متوافقة. راجع دائمًا [المستودع الرسمي](https://github.com/deepseek-ai/deepseek-harness) و[الوثائق الرسمية](https://deepseek-harness.github.io/deepseek-harness/).

## لماذا نحتاج إلى Harness؟

النموذج وحده لا يقرأ مستودعًا ولا ينفذ الأوامر ولا يستدعي الأدوات ولا يطلب الموافقة ولا يحفظ الجلسات. يوفر Harness بيئة التشغيل هذه وينسق بين المستخدم والنموذج والأدوات وحالة التطبيق.

يعتمد DeepSeek Harness على [Cordis](https://github.com/cordiverse/cordis). تضيف الإضافات خدمات وأحداثًا محددة النوع وتأثيرات قابلة للعكس إلى Context مشترك. وبذلك يمكن استبدال النموذج أو الأدوات أو بيئة العزل أو التخزين أو الوكلاء الفرعيين دون إنشاء نسخة متفرعة من التطبيق كاملًا.

## المفاهيم الأساسية

| المفهوم | المعنى |
| --- | --- |
| Plugin | وحدة TypeScript أو كائن أو فئة خدمة يتم تركيبها داخل Cordis Context. |
| Bundle | حزمة npm توزع طبقة إعدادات من خلال `dsh.bundle`. |
| Profile | تركيبة قابلة للتشغيل من Bundles واعتماديات محلية. |
| Patch | طبقة YAML تضيف صفوف الإعدادات أو تستبدلها. |
| Service / Event | قدرة قابلة للاستبدال ونقطة توسعة في تدفق الوكيل. |

حلقة الوكيل نفسها قابلة للاستبدال. تجمع الحلقة الافتراضية التعليمات ومخططات الأدوات، وتبث استجابة النموذج، وتنفذ الأدوات، وتسجل أحداث الجلسة الدائمة.

## بدء سريع

```bash
npx @deepseek-ai/dsh web
```

تعمل واجهة الويب افتراضيًا على `http://127.0.0.1:3080`. أضف بيانات اعتماد النموذج من **Settings → Models** ثم اختر مساحة عمل.

## ما الذي يغطيه الدليل؟

- Cordis ودورة حياة الإضافة وحقن الاعتماديات والتأثيرات القابلة للعكس.
- إضافات الأدوات والنماذج والعزل والتخزين والوكلاء الفرعيين وواجهة الويب.
- Bundles وProfiles و`cordis.patch.yml` والاختبار والنشر والأمان.
- مهارات Agent مخطط لها: `dsh-repository-explorer` و`dsh-plugin-scaffold` و`dsh-tool-builder` و`dsh-plugin-review`.

تعني **Skill** هنا سير عمل تعليميًا قابلًا لإعادة الاستخدام لوكلاء البرمجة، وليست **Plugin** تعمل داخل DeepSeek Harness. هذه المهارات لم تُنشر بعد.

## الموارد الرسمية

- [الشيفرة المصدرية](https://github.com/deepseek-ai/deepseek-harness)
- [البنية](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [أول إضافة](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [حزم الإضافات وتثبيتها](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## الترخيص

[MIT](LICENSE)
