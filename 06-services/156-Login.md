---
id: 156
graphDocId: 1365
title: "Login"
subTitle: null
module: "06-services"
wikiPath: "156"
wikiUrl: "https://wiki.graphbpms.ir/156"
parentPageUrl: "https://wiki.graphbpms.ir/modules/services"
breadcrumbs: ["سرویس های عمومی گراف","Login"]
parentId: 5
parentTitle: "سرویس های عمومی گراف"
url: "156"
priority: 2
language: 1
showInNavigation: false
isStandalonePage: false
summary: "مسیر موضوعی: سرویس های عمومی گراف. آدرس سرویس: قبل از ارسال هرگونه درخواستی به سرویس های داخلی گراف ابتدا باید وارد سیستم شوید. این سرویس امکان لاگین و دریافت Token را فراهم می آورد. مقادیر ورودی:؛ توضیحات؛ Position؛ Allow Null؛ Parameter Type؛ Parameter Name؛ ؛ نام کاربری که در سامانه فعال است.؛ Body؛ False؛ String؛ username؛ ؛ کلمه عبور؛ Body؛ False؛ String؛ password."
updatedAt: "2026-10-18T16:08:51"
---

# Login

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `سرویس های عمومی گراف` ❯ `Login`  
> 🔗 **صفحه مادر در دانشنامه:** [https://wiki.graphbpms.ir/modules/services](https://wiki.graphbpms.ir/modules/services) (بخش «Login»)

# سرویس ورود به سیستم (Login)

**آدرس سرویس:** `https://UrlAddress/SharedApi/login`

قبل از ارسال هرگونه درخواستی به سرویس های داخلی گراف ابتدا باید وارد سیستم شوید. این سرویس امکان لاگین و دریافت Token را فراهم می آورد.

## مقادیر ورودی

| توضیحات | Position | Allow Null | Parameter Type | Parameter Name |
| --- | --- | --- | --- | --- |
| نام کاربری که در سامانه فعال است. | Body | False | String | `username` |
| کلمه عبور | Body | False | String | `password` |
| مقدار پیش فرض این ورودی false است، در صورتی که کاربران سامانه از طریق ldap به سیستم وارد می شوند این مقدار باید true باشد. | Body | False | Boolean | `networkUser` |

## مقادیر خروجی

| توضیحات | Nullable | Result property type | Result property name |
| --- | --- | --- | --- |
| Token اصلی که باید در ارسال درخواست های بعد از ورود به سیستم ارسال شود. | True | String | `PrivateToken` |
| می تواند ارسال نشود. | True | String | `PublicToken` |
| در صورتی که عملیات ورود با خطا مواجه شود مقدار این خروجی true می شود. | True | String | `PublicToken` |
| نتیجه موفقیت یا عدم موفقیت عملیات ورود در این خروجی قرار می گیرد. | False | String | `Message` |

> [!NOTE]
> **نکات**
> 
> 1.  مقادیر خروجی فوق در قالب یک Json Object استاندارد بازگشت داده می شود.
> 2.  جهت ادامه سایر سرویس ها فقط از Private token استفاده نمایید
> 3.  براساس تنظیمات سیستم Private token دارای اعتبار زمانی است.

جهت مشاهده مستندات استفاده از سرویس های عمومی گراف بر روی لینک زیر کلیک کنید

[مستندات سرویس های عمومی گراف](https://documenter.getpostman.com/view/29502528/2sAXqv3fFN#493e0e02-cda4-4093-90d9-8c6956fe4c04)
