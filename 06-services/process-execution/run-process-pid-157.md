---
id: 157
title: "RunProcess"
englishTitle: "Process Initiation API (RunProcess)"
category: "06-services/process-execution"
wikiPath: "157"
wikiUrl: "https://wiki.graphbpms.ir/157"
breadcrumbs: ["سرویس های عمومی گراف","RunProcess"]
priority: 3
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:25:32"
---

# RunProcess

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `سرویس های عمومی گراف` ❯ `RunProcess`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/157](https://wiki.graphbpms.ir/157)

# سرویس اجرای فرایند (RunProcess)

**آدرس سرویس:** `https://UrlAddress/SharedApi/RunProcess`

## مقادیر ورودی

| توضیحات | Position | Allow null | Parameter type | Parameter name |
| --- | --- | --- | --- | --- |
| توکن احراز هویت | Header | False | String | `privateToken` |
| به صورت raw ارسال شود. | Body | False | 
Object = {  
ProcessGroup?: number,  
Process?: number,  
RoleID: number,  
UserID: number,  
RunType: number,  
Variables?:  
\[{name: string, value: string}\],  
ProcessTitle?: string

 | `model` |

**?** به معنی allow null است .

> [!NOTE]
> **نکات**
> 
> 1.  به طور همزمان نباید مقدار ProcessGroup یا Process را ارسال کرد.
> 2.  در صورتی که مقدار ProcessGroup ارسال شده باشد آخرین نگارش فعال آن گروه فرآیندی مورد استفاده قرار میگیرد.
> 3.  در صورتی که مقدار Process ارسال شده باشد، فرآیند متناظر شناسه ارسالی حتما" باید فعال باشد در غیر این صورت فرآیند اجرا نمی شود.
> 4.  شناسه نقش مجاز شروع کننده فرآیند باید صحیح باشد و اجباری است.
> 5.  شناسه کاربر مجاز شروع کننده فرآیند باید صحیح باشد و اجباری است.
> 6.  مقدار پیش فرض RunType باید 500002 باشد در غیر این صورت فرآیند اجرا نمی شود.
> 7.  متغیرهای ارسالی باید از قبل در فرآیند تعریف شده باشند، در زمان ارسال متغیر نیازی به استفاده از @@ نیست و مقدار value باید به صورت یک مقدار خام عددی یا رشته ای یا Json array, Json object باشد.
> 8.  صورتی که قصد ارسال مقداری به صورت رشته ای و عددی در قالب متغیر ها دارید بایستی به صورت زیر ارسال کنید

```
[
    {
        "name": "Var1",
        "value":"StingValue"
    }
    ,
    {
        "name": "Var2",
        "value": 100
    }
]
```

**نمونه روش مقدار دهی Variables های پیچیده:**

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

> [!NOTE]
> **نکته**
> 
> در صورتی که نیاز به تغییر عنوان فرآیند در حین اجرای آن دارید از ProcessTitle استفاده کنید و در صورتی که مقداری ارسال نشود عنوان پیش فرض فرآیند در زمان طراحی مورد استفاده قرار میگیرد.

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| نتیجه عملیات و داده های خروجی در این بخش قرار میگیرد. | True | Any | `Content` |
| کد خطای متناظر، در صورتی که -1 باشد عملیات به درستی انجام شده است. | True | Number | `ErrorCode` |
| در صورتی که عملیات ورود با خطا مواجه شود مقدار این خروجی true می شود. | True | Boolean | `HasError` |
| نتیجه موفقیت یا عدم موفقیت عملیات ورود در این خروجی قرار می گیرد. | True | String | `Message` |
| در صورت بروز خطا متن خطا در این بخش قرار میگیرد. | True | String | `MessageException` |

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
