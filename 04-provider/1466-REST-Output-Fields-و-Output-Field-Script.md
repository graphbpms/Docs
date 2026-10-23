---
id: 1466
graphDocId: 1523
title: "REST Output Fields و Output Field Script"
subTitle: null
module: "04-provider"
wikiPath: "1450/1462/1466"
wikiUrl: "https://wiki.graphbpms.ir/1450/1462/1466"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1462/1466"
breadcrumbs: ["مدیریت منابع داده Provider","اتصال به Web Service","REST Output Fields و Output Field Script"]
parentId: 1462
parentTitle: "اتصال به Web Service"
url: "1466"
priority: 4
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۳. اتصال به Web Service. اتصال به Web Service. GraphBPMS • Provider REST Output Fields و Output Field Script در این آموزش 14.7. گام «لیست فیلدهای خروجی سرویس» 14.8. Output Field Script در REST مثال‌های تکمیلی برای Output Field Script 14.7. گام «لیست فیلدهای خروجی سرویس» در این مرحله ابتدا «نوع محتوای پاسخ» یا Response Content Type تعیین می‌شود. این انتخاب مشخص می‌کند که سامانه پاسخ سرویس را چگونه تفسیر کند. محورهای اصلی: 14.7. گام «لیست فیلدهای خروجی سرویس»؛ 14.8. Output Field Script در REST؛ مثال‌های تکمیلی برای Output Field Script؛ مثال ۱ — تبدیل Boolean به Status متنی؛ مثال ۲ — ترکیب دو خروجی متنی؛ مثال ۳ — محاسبه مقدار عددی؛ مثال ۴ — مدیریت مقدار Null / خالی."
updatedAt: "2026-10-18T16:08:51"
---

# REST Output Fields و Output Field Script

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `اتصال به Web Service` ❯ `REST Output Fields و Output Field Script`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1462/1466](https://wiki.graphbpms.ir/1450/1462/1466)

در این آموزش

-   [14.7. گام «لیست فیلدهای خروجی سرویس»](#wiki-rest-output-script-section-01)
-   [14.8. Output Field Script در REST](#wiki-rest-output-script-section-02)
-   [مثال‌های تکمیلی برای Output Field Script](#wiki-rest-output-script-section-03)

## 14.7. گام «لیست فیلدهای خروجی سرویس»

در این مرحله ابتدا «نوع محتوای پاسخ» یا `Response Content Type` تعیین می‌شود. این انتخاب مشخص می‌کند که سامانه پاسخ سرویس را چگونه تفسیر کند. نمونه‌های متداول شامل `text/plain`، `application/json` و در برخی سناریوها محتوای فایل است.

اگر پاسخ JSON باشد، امکان تعریف فیلدهای ساختاریافته خروجی وجود دارد. همچنین اگر بدنه پاسخ یک آرایه باشد، گزینه «محتوی بدنه خروجی آرایه است؟» باید متناسب با قرارداد سرویس تنظیم شود.

هر ردیف فیلد خروجی معمولاً می‌تواند این ویژگی‌ها را داشته باشد:

-   **script**: آیا این فیلد از طریق اسکریپت محاسبه می‌شود؟
-   **nullable**: آیا مقدار این فیلد می‌تواند تهی باشد؟
-   **نوع داده**: مثل String یا Boolean
-   **نام فیلد**: مانند `IsOk` یا `A_ResponseHeader`
-   **منبع مقدار**: از `body` یا `header`

![نمونه خروجی با Response Content Type برابر text/plain. در این حالت خروجی ساختاریافته‌ای از نوع JSON تعریف نشده و معمولاً پاسخ متنی خام مبنا قرار می‌گیرد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=970F7028-CA7C-4008-8E82-3B24B4B4888B)

نمونه خروجی با Response Content Type برابر text/plain. در این حالت خروجی ساختاریافته‌ای از نوع JSON تعریف نشده و معمولاً پاسخ متنی خام مبنا قرار می‌گیرد.

![نمونه دیگری از گام «لیست فیلدهای خروجی سرویس» که نوع محتوای پاسخ در آن File تنظیم شده است. این تصویر برای تکمیل مستندات تصویری این مرحله افزوده شده است.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=542073F2-969A-4B54-A450-FA093B11232A)

نمونه دیگری از گام «لیست فیلدهای خروجی سرویس» که نوع محتوای پاسخ در آن File تنظیم شده است. این تصویر برای تکمیل مستندات تصویری این مرحله افزوده شده است.

![نمونه تعریف فیلدهای خروجی برای پاسخ application/json. در این مثال فیلد IsOk از body و فیلد A\_ResponseHeader از header استخراج می‌شود و گزینه «محتوی بدنه خروجی آرایه است؟» نیز قابل تنظیم است.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=2525E957-97DA-4054-9762-6D7422440E7A)

نمونه تعریف فیلدهای خروجی برای پاسخ application/json. در این مثال فیلد IsOk از body و فیلد A\_ResponseHeader از header استخراج می‌شود و گزینه «محتوی بدنه خروجی آرایه است؟» نیز قابل تنظیم است.

![منوی انتخاب محل خواندن فیلد خروجی. در پاسخ JSON می‌توان فیلد خروجی را از body یا header دریافت کرد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=2130917F-6EC2-49AE-A646-C1DA8AB8EE8C)

منوی انتخاب محل خواندن فیلد خروجی. در پاسخ JSON می‌توان فیلد خروجی را از body یا header دریافت کرد.

![نمونه ساخت Output Field محاسباتی در REST. با فعال‌سازی Script و استفاده از دکمه IDE می‌توان فیلدی سفارشی مثل IsNotOk را بر اساس سایر خروجی‌ها تعریف کرد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=05AF0C60-D4D4-4228-B11C-8B3A97A97E3B)

نمونه ساخت Output Field محاسباتی در REST. با فعال‌سازی Script و استفاده از دکمه IDE می‌توان فیلدی سفارشی مثل IsNotOk را بر اساس سایر خروجی‌ها تعریف کرد.

![محیط Script IDE برای Output Field Script. در نمونه حاضر مقدار فیلد IsNotOk از روی args.IsOk محاسبه شده و در متغیر result قرار می‌گیرد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=022CFA41-DE72-41E1-BE57-B9B6B5B9159A)

محیط Script IDE برای Output Field Script. در نمونه حاضر مقدار فیلد IsNotOk از روی args.IsOk محاسبه شده و در متغیر result قرار می‌گیرد.

## 14.8. Output Field Script در REST

یکی از قابلیت‌های مهم REST Provider تعریف فیلد خروجی محاسباتی است. این همان بخشی است که قبلاً تصویر کافی نداشت و اکنون با تصاویر کامل مستند شده است. برای استفاده از این قابلیت:

1.  یک ردیف خروجی جدید اضافه کنید.
2.  گزینه `script` را فعال کنید.
3.  نوع داده و نام فیلد محاسباتی را مشخص کنید؛ مثلاً `IsNotOk`.
4.  از دکمه **IDE** وارد محیط Script Editor شوید.
5.  منطق محاسبه را با استفاده از خروجی‌های دیگر بنویسید و نتیجه نهایی را در متغیر `result` قرار دهید.

در نمونه موجود، فیلد محاسباتی `IsNotOk` بر اساس فیلد `IsOk` ساخته شده است:

result = !(args.IsOk);

در این الگو، `args` شامل سایر فیلدهای خروجی همان رکورد است و سامانه انتظار دارد مقدار نهایی فیلد محاسباتی در متغیر `result` ذخیره شود. بنابراین این قابلیت برای ساخت فیلدهای مشتق‌شده، پرچم‌های کمکی و نرمال‌سازی خروجی بسیار کاربردی است.

**توجه:** Output Field Script برای تعریف ستون‌های سفارشی (Custom / Derived Fields) به‌کار می‌رود؛ یعنی فیلدی که الزاماً مستقیماً از پاسخ خام سرویس نیامده، بلکه از روی سایر خروجی‌ها محاسبه شده است.

**★ تکمیل لازم قبل از انتشار نهایی**

متن کامل راهنمایی که داخل Script IDE / UI همین بخش نمایش داده می‌شود باید از Build مقصد استخراج و **کامل و بدون خلاصه‌سازی ناقص** در همین محل قرار گیرد. تصویر واقعی IDE که در سند اولیه وجود دارد در بالا حفظ شده است؛ اما متن راهنما نیز باید به‌صورت قابل جست‌وجو در Wiki درج شود.

## مثال‌های تکمیلی برای Output Field Script

مثال‌های زیر **\[راهکار عمومی JavaScript\]** هستند و فقط از قرارداد مستند همین بخش (`args` برای خروجی‌های همان رکورد و `result` برای مقدار نهایی) استفاده می‌کنند. نام فیلدها نمونه است و باید با Schema واقعی Provider جایگزین شود.

### مثال ۱ — تبدیل Boolean به Status متنی

```js
result = args.IsOk ? 'OK' : 'NOT_OK';
```

### مثال ۲ — ترکیب دو خروجی متنی

```js
result = [args.FirstName, args.LastName]
    .filter(Boolean)
    .join(' ');
```

### مثال ۳ — محاسبه مقدار عددی

```js
result =
    Number(args.Price || 0) *
    Number(args.Quantity || 0);
```

### مثال ۴ — مدیریت مقدار Null / خالی

```js
result =
    args.Value == null
        ? ''
        : String(args.Value).trim();
```

**تست الزامی:** هر مثال قبل از انتشار باید در Script IDE همان Build مقصد اجرا شود. Syntax یا Helper آزمایش‌نشده نباید به‌عنوان API اختصاصی GraphBPMS معرفی شود.
