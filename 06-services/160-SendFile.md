---
id: 160
graphDocId: 1393
title: "SendFile"
subTitle: null
module: "06-services"
wikiPath: "160"
wikiUrl: "https://wiki.graphbpms.ir/160"
parentPageUrl: "https://wiki.graphbpms.ir/modules/services"
breadcrumbs: ["سرویس های عمومی گراف","SendFile"]
parentId: 5
parentTitle: "سرویس های عمومی گراف"
url: "160"
priority: 6
language: 1
showInNavigation: false
isStandalonePage: false
summary: "مسیر موضوعی: سرویس های عمومی گراف. آدرس سرویس: از این سرویس جهت ارسال فایل در سیستم آرشیو سامانه گراف استفاده می شود. دقت نمایید که جزئیات فایل بررسی می گردد. فایل باید به فرمت base64 به این سرویس ارسال گردد. مقادیر ورودی:؛ توضیحات؛ Position؛ Allow null؛ Allow null؛ Parameter name؛ ؛ توکن احراز هویت؛ Header؛ False؛ String؛ privateToken؛ ؛ فایل به فرمت base64؛ Body؛ False؛ String؛ fileContent؛ ؛ نام فایل به همراه نوع فایل مانند profile.jpg؛ Body؛ False؛ String."
updatedAt: "2026-10-18T16:08:51"
---

# SendFile

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `سرویس های عمومی گراف` ❯ `SendFile`  
> 🔗 **صفحه مادر در دانشنامه:** [https://wiki.graphbpms.ir/modules/services](https://wiki.graphbpms.ir/modules/services) (بخش «SendFile»)

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
