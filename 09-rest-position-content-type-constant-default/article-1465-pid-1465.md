---
id: 1465
title: "پارامترهای REST — Position، Content Type و Constant/Default"
englishTitle: "پارامترهای REST — Position، Content Type و Constant/Default"
category: "09-rest-position-content-type-constant-default"
wikiUrl: "https://wiki.graphbpms.ir/1465"
breadcrumbs: ["پارامترهای REST — Position، Content Type و Constant/Default"]
priority: 3
updatedAt: "2026-10-25T21:33:55"
---

# پارامترهای REST — Position، Content Type و Constant/Default

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `پارامترهای REST — Position، Content Type و Constant/Default`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1465](https://wiki.graphbpms.ir/1465)

# پارامترهای REST — Position، Content Type و Constant/Default
در این آموزش
- [14.5. گام «لیست پارامترها» و Request Content Type](#wiki-rest-params-section-01)
- [14.6. رفتار محل قرارگیری پارامترها (Parameter Location)](#wiki-rest-params-section-02)

## 14.5. گام «لیست پارامترها» و Request Content Type

پس از تنظیم اطلاعات اصلی سرویس، وارد مرحله تعریف پارامترهای ورودی می‌شوید.
در این گام ابتدا «نوع محتوای درخواست» یا `Request Content Type` مشخص می‌شود.
این انتخاب، نحوه ساخت Payload و همچنین گزینه‌های قابل انتخاب برای محل قرارگیری پارامترها را تعیین می‌کند.

- **application/json**: برای ارسال داده به‌صورت JSON.

- **application/json; charset=utf-8**: همان JSON با تعیین صریح charset.

- **application/x-www-form-urlencoded**: برای ارسال داده‌های فرم URL-Encoded.

- **multipart/form-data**: برای ارسال فایل و فیلدهای همراه آن.

هر ردیف پارامتر ورودی معمولاً شامل این اجزاست:

- **نوع داده** مانند String، Boolean یا File

- **مقدار** یا مقدار پیش‌فرض / نمونه

- **نام پارامتر**

- **محل قرارگیری** مانند url، header، body یا form-data

فهرست نوع محتوای درخواست (Request Content Type) در گام پارامترها. مقادیر متداول شامل application/json، application/json; charset=utf-8، application/x-www-form-urlencoded و multipart/form-data هستند.

نمونه تعریف پارامترهای ورودی برای درخواست multipart/form-data؛ شامل Authorization در Header، فایل در form-data و پارامترهای model، language و response_format.

در درخواست multipart/form-data محل قرارگیری هر پارامتر می‌تواند از نوع url، header یا form-data باشد. این تصویر منوی انتخاب محل قرارگیری برای پارامتر فایل را نشان می‌دهد.

نمونه تعریف پارامترها برای application/json; charset=utf-8. در این حالت پارامترها معمولاً در body یا header قرار می‌گیرند؛ مانند Authorization در header و model/language در body.

نمونه تعریف پارامترها برای application/x-www-form-urlencoded. در این حالت نیز پارامترها در body یا header ثبت می‌شوند و سامانه payload را متناسب با این Content-Type می‌سازد.

راهنمای استفاده از Query Parameter در URL. برای قراردادن متغیر در آدرس سرویس باید نام متغیر دلخواه داخل آکلاد قرار گیرد؛ مانند rememberMe={rememberMeValue}.

نمونه کامل تعریف Query Parameter متناظر با placeholder موجود در URL. در این مثال پارامتر rememberMeValue با نوع Boolean، مقدار true و محل قرارگیری url تعریف شده است.

## 14.6. رفتار محل قرارگیری پارامترها (Parameter Location)

محل قرارگیری پارامتر به Content-Type و ماهیت پارامتر وابسته است:

سناریومحل‌های رایجتوضیح

**Query / Path like URL**urlبرای مقادیری که باید داخل آدرس یا Query String جای‌گذاری شوند.
**Header**headerبرای هدرهایی مانند Authorization.
**JSON Request**bodyبرای فیلدهایی که داخل بدنه JSON قرار می‌گیرند.
**Form URL Encoded**bodyبرای کلید/مقدارهایی که در بدنه فرم‌مانند ارسال می‌شوند.
**Multipart**form-dataبرای فایل‌ها و فیلدهای همراه آن در فرم چندبخشی.

در نمونه‌های تصویری، دیده می‌شود که:

- پارامتر `Authorization` در `header` تعریف شده است.

- پارامتر `file` در سناریوی multipart از نوع `File` و محل `form-data` دارد.

- پارامترهای `model` و `language` بسته به Content-Type در `body` یا `form-data` قرار گرفته‌اند.

- پارامتر `rememberMeValue` برای پرکردن Query String در محل `url` ثبت شده است.

**توضیح تکمیلی الزامی — Constant / Default در REST Provider**

اگر برای یک Parameter مقدار Default/Constant ذخیره شده باشد، Caller مجبور نیست آن Parameter را در هر Call دوباره ارسال کند.
اگر Caller برای همان Parameter یک مقدار مؤثر ارسال کند، مقدار ارسال‌شده استفاده می‌شود؛
و فقط در صورت نبود مقدار مؤثر، Default ذخیره‌شده جایگزین می‌شود.

مقدار ارسالی CallerDefault ذخیره‌شدهمقدار نهایی

ارسال نشده`fa``fa`
`en``fa``en`

این رفتار با **Static Backend Parameter در Service Bus** یکی نیست؛ در Service Bus مقدار Static برای اجرای Backend قفل می‌شود.
