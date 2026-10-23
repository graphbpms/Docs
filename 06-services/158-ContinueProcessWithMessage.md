---
id: 158
graphDocId: 1381
title: "ContinueProcessWithMessage"
subTitle: null
module: "06-services"
wikiPath: "158"
wikiUrl: "https://wiki.graphbpms.ir/158"
parentPageUrl: "https://wiki.graphbpms.ir/modules/services"
breadcrumbs: ["سرویس های عمومی گراف","ContinueProcessWithMessage"]
parentId: 5
parentTitle: "سرویس های عمومی گراف"
url: "158"
priority: 4
language: 1
showInNavigation: false
isStandalonePage: false
summary: "مسیر موضوعی: سرویس های عمومی گراف. آدرس سرویس: در صورتی که در فرآیند طراحی شده از Message Boundary Event یا Receive Task استفاده کرده باشید، در زمان طراحی فرآیند باید دو ورودی برای فرآیند اجرا شده و مرحله مورد نظر در Provider قرار داده باشید. زمانی که فرآیند در حین اجرا/ادامه به مرحله Message Boundary Event یا Receive Task برسد متوقف خواهد شد و جهت ادامه آن از مرحله مورد نظر باید از این سرویس استفاده شود. مقادیر ورودی:؛ توضیحات؛ Position؛ Allow null."
updatedAt: "2026-10-18T16:08:51"
---

# ContinueProcessWithMessage

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `سرویس های عمومی گراف` ❯ `ContinueProcessWithMessage`  
> 🔗 **صفحه مادر در دانشنامه:** [https://wiki.graphbpms.ir/modules/services](https://wiki.graphbpms.ir/modules/services) (بخش «ContinueProcessWithMessage»)

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
