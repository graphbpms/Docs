# GraphBPMS Documentation (مستندات دانشنامه گراف)

[![Docs Status](https://img.shields.io/badge/Docs-Production_Ready-2ea44f.svg)](#)
[![Total Articles](https://img.shields.io/badge/Canonical_Articles-125-blue.svg)](#)
[![Modules](https://img.shields.io/badge/Modules-9-orange.svg)](#)
[![Status](https://img.shields.io/badge/Live_URLs-100%25_Active-brightgreen.svg)](#)

به مخزن رسمی و ساختاریافته مستندات جامع سامانه **GraphBPMS** خوش آمدید.
این مستندات به صورت پالایش‌شده و اصیل، با حذف کلیه رکوردهای تکراری و نودهای منسوخ، بر پایه ساختار ناوبری رسمی وب‌سایت دانشنامه (`wiki.graphbpms.ir`) سازمان‌دهی شده‌اند.

## 📦 ساختار ماژولار مستندات

مستندات سامانه در ۹ شاخه اصلی ماژولار تفکیک شده‌اند. در هر شاخه یک فایل `README.md` به عنوان پیش‌خوان و هاب ماژول قرار دارد:

| ماژول | شرح بخش | تعداد مقالات | پیش‌خوان ماژول | نسخه برخط در دانشنامه |
| :--- | :--- | :---: | :---: | :---: |
| `01-form-builder` | فرم‌ساز پیشرفته، ابزارها و فیلدها | ۲۴ | [مشاهده پیش‌خوان](01-form-builder/README.md) | [wiki.graphbpms.ir/modules/forms](https://wiki.graphbpms.ir/modules/forms) |
| `02-process` | مدیریت و مدل‌سازی فرآیندها (BPMN) | ۲۲ | [مشاهده پیش‌خوان](02-process/README.md) | [wiki.graphbpms.ir/modules/process](https://wiki.graphbpms.ir/modules/process) |
| `03-report` | گزارش‌ساز پیشرفته، نمودارها و GIS | ۹ | [مشاهده پیش‌خوان](03-report/README.md) | [wiki.graphbpms.ir/modules/reports](https://wiki.graphbpms.ir/modules/reports) |
| `04-provider` | منابع داده، سرویس‌ها و وب‌سرویس‌ها | ۳۸ | [مشاهده پیش‌خوان](04-provider/README.md) | [wiki.graphbpms.ir/modules/providers](https://wiki.graphbpms.ir/modules/providers) |
| `05-system` | مدیریت سیستم، کاربران، نقش‌ها و لایه‌ها | ۹ | [مشاهده پیش‌خوان](05-system/README.md) | [wiki.graphbpms.ir/modules/system](https://wiki.graphbpms.ir/modules/system) |
| `06-services` | سرویس‌های عمومی سامانه (Login, RunProcess...) | ۷ | [مشاهده پیش‌خوان](06-services/README.md) | [wiki.graphbpms.ir/modules/services](https://wiki.graphbpms.ir/modules/services) |
| `07-installation` | راهنمای نصب، بروزرسانی و WebConfig | ۴ | [مشاهده پیش‌خوان](07-installation/README.md) | [wiki.graphbpms.ir/modules/install](https://wiki.graphbpms.ir/modules/install) |
| `08-video-tutorials` | فیلم‌ها و دوره‌های ویدئویی آموزشی | ۱۱ | [مشاهده پیش‌خوان](08-video-tutorials/README.md) | [wiki.graphbpms.ir/modules/videos](https://wiki.graphbpms.ir/modules/videos) |
| `10-general` | فایل‌ها، ضمیمه‌ها و مستندات تکمیلی | ۱ | [مشاهده پیش‌خوان](10-general/README.md) | [wiki.graphbpms.ir/1509](https://wiki.graphbpms.ir/1509) |

## 📚 نحوه ناوبری و مطالعه
* **[فهرست جامع دانشنامه (SUMMARY.md)](SUMMARY.md)**: فهرست کامل درختی تمام ۱۲۵ مقاله به تفکیک ماژول‌ها.
* **فایل متادیتا [manifest.json](manifest.json)**: متادیتای ساختاریافته JSON شامل شناسه‌ها، بردکرامب‌ها و مسیرهای URL جهت استفاده در موتورهای هوش مصنوعی و RAG.

## ⚙️ استاندارد فایل‌های مارک‌داون
تمام صفحات شامل یک بلوک استاندارد **Frontmatter (YAML)** در ابتدای فایل هستند که متادیتای کامل سند را ذخیره کرده است:
```yaml
---
id: 1398
title: "Text - متنی"
module: "01-form-builder"
wikiPath: "Form/1394/1396/1397/1398"
wikiUrl: "https://wiki.graphbpms.ir/Form/1394/1396/1397/1398"
breadcrumbs: ["فرم‌ساز پیشرفته", "استودیوی فرم‌ساز", "فیلدهای فرم", "فیلدهای پایه", "Text - متنی"]
parentId: 1397
priority: 1
language: 1
showInNavigation: true
---
```

---
*سامانه مستندات GraphBPMS — نسخه تولید نهایی*