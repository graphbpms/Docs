---
id: 161
title: "GenerateStimulExportFile"
englishTitle: "Automated Stimulsoft Report Render API"
category: "06-services/file-and-export"
wikiPath: "161"
wikiUrl: "https://wiki.graphbpms.ir/161"
breadcrumbs: ["سرویس های عمومی گراف","GenerateStimulExportFile"]
priority: 7
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:27:11"
---

# GenerateStimulExportFile

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `سرویس های عمومی گراف` ❯ `GenerateStimulExportFile`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/161](https://wiki.graphbpms.ir/161)

# سرویس دریافت خروجی قالب چاپ (GenerateStimulExportFile)

**آدرس سرویس:** `https://UrlAddress/SharedApi/GenerateStimulExportFile`

در صورتی که نیاز به دریافت خروجی قالب چاپ طراحی شده با یکی از فرمت‌های `pdf`، `jpg`، `csv`، `html` دارید از این سرویس استفاده نمایید.

## مقادیر ورودی

| توضیحات | Position | Allow null | Parameter type | Parameter name |
| --- | --- | --- | --- | --- |
| توکن احراز هویت | Header | False | String | `privateToken` |
| چاپ | Body | False | Number | `exportId` |
| شناسه جدول ReportRecourse منبع داده به همراه آرایه ای مقادیر ورودی | Body | False | Array = \[{ResourceId: number, Parameters: \[{name: string, value: string}\]}\] | `paramList` |
| در صورتی که مقداری ارسال نشود خروجی به صورت pdf است. مقادیر ورودی می تواند یکی از فرمت های: Pdf, html, csv, jpg باشد | Body | True | String | `fileExportType` |

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| فایل تولید شده با فرمت درخواستی به صورت base64 | True | String | `Content` |
| کد خطای متناظر، در صورتی که -1 باشد عملیات به درستی انجام شده است. | True | Number | `ErrorCode` |
| در صورتی که عملیات ورود با خطا مواجه شود مقدار این خروجی true می شود. | False | Boolean | `HasError` |
| نتیجه موفقیت یا عدم موفقیت عملیات ورود در این خروجی قرار می گیرد. | True | String | `Message` |
| در صورت بروز خطا متن خطا در این بخش قرار میگیرد. | True | String | `MessageException` |

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
