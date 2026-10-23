---
id: 158
title: "ContinueProcessWithMessage"
englishTitle: "Message Intermediate Event API"
category: "06-services/process-execution"
wikiPath: "158"
wikiUrl: "https://wiki.graphbpms.ir/158"
breadcrumbs: ["سرویس های عمومی گراف","ContinueProcessWithMessage"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:25:48"
---

# ContinueProcessWithMessage

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `سرویس های عمومی گراف` ❯ `ContinueProcessWithMessage`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/158](https://wiki.graphbpms.ir/158)

# سرویس ادامه فرایند با Message (ContinueProcessWithMessage)

**آدرس سرویس:** `https://UrlAddress/SharedApi/ContinueProcessWithMessage`

در صورتی که در فرآیند طراحی شده از Message Boundary Event یا Receive Task استفاده کرده باشید، در زمان طراحی فرآیند باید دو ورودی برای فرآیند اجرا شده و مرحله مورد نظر در Provider قرار داده باشید. زمانی که فرآیند در حین اجرا/ادامه به مرحله Message Boundary Event یا Receive Task برسد متوقف خواهد شد و جهت ادامه آن از مرحله مورد نظر باید از این سرویس استفاده شود.

## مقادیر ورودی

| توضیحات | Position | Allow null | Parameter type | Parameter name |
| --- | --- | --- | --- | --- |
| توکن احراز هویت | Header | False | String | `privateToken` |
| شناسه اجرایی فرآیند که متوقف شده است. | Body | False | Number | `processCreationID` |
| شناسه مرحله ای که فرآیند در آن مرحله متوقف شده است. | Body | False | Number | `stepID` |
| در صورتی که نیاز به ارسال مقادیری به فرآیند دارید از این ورودی می توانید استفاده کنید. | Body | true | Array = \[{name: string, value: string}\] | `Variables` |

**نمونه روش مقدار دهی Variables:**

```
[
    {
        "name": "Var1",
        "value": "[
            { \"firstname\": \"john\", \"lastname\": \"doe\"}
        ]"
    }
]
```

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| نتیجه عملیات و داده های خروجی در این بخش قرار میگیرد. | True | 
Object = {  
ReferenceState: number,  
ReferenceStateMessage: string  
}

 | `Content` |
| کد خطای متناظر، در صورتی که -1 باشد عملیات به درستی انجام شده است. | True | Number | `ErrorCode` |
| در صورتی که عملیات ورود با خطا مواجه شود مقدار این خروجی true می شود. | False | Boolean | `HasError` |
| نتیجه موفقیت یا عدم موفقیت عملیات ورود در این خروجی قرار می گیرد. | True | String | `Message` |
| در صورت بروز خطا متن خطا در این بخش قرار میگیرد. | True | String | `MessageException` |

> [!NOTE]
> **نکته**
> 
> خروجی ReferenceState یکی از گزینه های زیر است:
> 
> 1.  `IsFired = 500090` فرآیند قبلا ادامه یافته است و امکان ادامه آن براساس اطلاعات ارسالی وجود ندارد.
> 2.  `IsNotExist = 500091` فرآیندی جهت ادامه براساس اطلاعات ارسالی وجود ندارد.
> 3.  `IsNotFired = 500092` (مخفی شده است، قابل استفاده نیست)
> 4.  `IsExceptionMode = 500093` خطایی در ادامه فرآیند رخ داده است. متن خطا در بخش Message Exception درج شده است.
> 5.  `IsFiredSuccessfully = 500094` فرآیند براساس اطلاعات ارسالی با موفقیت ادامه یافت.

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
