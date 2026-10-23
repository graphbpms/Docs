---
id: 160
title: "SendFile"
englishTitle: "Binary Document Upload API (SendFile)"
category: "06-services/file-and-export"
wikiPath: "160"
wikiUrl: "https://wiki.graphbpms.ir/160"
breadcrumbs: ["سرویس های عمومی گراف","SendFile"]
priority: 6
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:26:56"
---

# SendFile

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `سرویس های عمومی گراف` ❯ `SendFile`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/160](https://wiki.graphbpms.ir/160)

# سرویس ارسال فایل (SendFile)

**آدرس سرویس:** `https://UrlAddress/SharedApi/SendFile`

از این سرویس جهت ارسال فایل در سیستم آرشیو سامانه گراف استفاده می شود. دقت نمایید که جزئیات فایل بررسی می گردد. فایل باید به فرمت base64 به این سرویس ارسال گردد.

## مقادیر ورودی

| توضیحات | Position | Allow null | Parameter type | Parameter name |
| --- | --- | --- | --- | --- |
| توکن احراز هویت | Header | False | String | `privateToken` |
| فایل به فرمت base64 | Body | False | String | `fileContent` |
| نام فایل به همراه نوع فایل مانند profile.jpg | Body | False | String | `filename` |

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| شناسه فایل در آرشیو | True | String | `Result` |

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
