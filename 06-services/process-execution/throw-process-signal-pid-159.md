---
id: 159
title: "ThrowProcessSignal"
englishTitle: "Broadcast Signal Event API"
category: "06-services/process-execution"
wikiPath: "159"
wikiUrl: "https://wiki.graphbpms.ir/159"
breadcrumbs: ["سرویس های عمومی گراف","ThrowProcessSignal"]
priority: 5
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:26:38"
---

# ThrowProcessSignal

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `سرویس های عمومی گراف` ❯ `ThrowProcessSignal`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/159](https://wiki.graphbpms.ir/159)

# سرویس فراخوانی سیگنال فرایند (ThrowProcessSignal)

**آدرس سرویس:** `https://UrlAddress/SharedApi/ThrowProcessSignal`

در صورتی که نیاز دارید سیگنال مد نظر خود را فراخوانی کنید تا با توجه به کلید آن تمامی سیگنال های catch در شروع فرآیند، نمونه سازی و شروع شوند و سیگنال های catch در انتظار میانی ادامه پیدا کنند، می توانید از این سرویس استفاده کنید.

> [!NOTE]
> **نکته**
> 
> در نظر داشته باشید که فراخوانی این سرویس عینا همانند یک نود سیگنال از ��نس throw در فرآیند می باشد.

## مقادیر ورودی

| توضیحات | Position | Allow null | Parameter type | Parameter name |
| --- | --- | --- | --- | --- |
| توکن احراز هویت | Header | False | String | `privateToken` |
| کلید سیگنال | Body | False | String | `SignalKey` |
| در صورتی که نیاز به ارسال مقادیری به فرآیند دارید از این ورودی می توانید استفاده کنید | Body | True | Array=\[{name:string , value:string}\] | `Variables` |

**نمونه بدنه ارسالی :**

```
{
    "SignalKey": "SignalA",
    "Variables": [
        {
            "Name": "variableNumber1",
            "Value": "someValueForVariableNumber1"
        }
    ]
}
```

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| نتیجه عملیات و داده های خروجی در این بخش قرار می گیرد | True | 
Array of objects = \[{  
ProcessCreationID: number,  
StatusDescription: string,  
EndVariables: \[  
{  
Name: string,  
Value: string  
}  
\]  
}\]

 | `Content` |
| کد خطای متناظر، در صورتی که -1 باشد عملیات به درستی انجام شده است. | True | Number | `ErrorCode` |
| در صورتی که عملیات ورود با خطا مواجه شود مقدار این خروجی true می شود. | False | Boolean | `HasError` |
| نتیجه موفقیت یا عدم موفقیت عملیات ورود در این خروجی قرار می گیرد. | True | String | `Message` |
| در صورت بروز خطا متن خطا در این بخش قرار میگیرد. | True | String | `MessageException` |

> [!NOTE]
> **نکته**
> 
> به ازای هر نمونه فرآیندی که سیگنالی در آن فراخوانی شده است یک رکورد از نتیجه آن فراخوانی در خروجی این سرویس داده می‌شود که شامل فیلد StatusDescription، EndVariables و ProcessCreationID است. فیلد StatusDescription اعلام‌کننده وضعیت انجام عملیات است و فیلد EndVariables شامل متغیرهای خروجی فرآیند است و تنها در صورتی دارای مقداری جز آرایه خالی می‌باشد که فرآیند، در ادامه این فراخوانی سیگنال به یک نود اتمام برسد که در آن متغیری خروجی داده شده باشد. فیلد EndVariables دقیقاً همان متغیرهای خروجی فرآیند هستند. فیلد ProcessCreationID مشخص‌کننده شناسه نمونه فرآیندی است که سیگنال آن فراخوانی شده است.

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
