---
id: 1491
title: "مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus"
englishTitle: "مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus"
category: "09-url-http-provider-service-bus"
wikiUrl: "https://wiki.graphbpms.ir/1491"
breadcrumbs: ["مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus"]
priority: 5
updatedAt: "2026-10-25T21:33:55"
---

# مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1491](https://wiki.graphbpms.ir/1491)

# مستند خودکار، URL، خطاهای HTTP، مثال کامل، تست و مرز Provider/Service Bus
در این آموزش
- [100. سند خودکار هر Service Bus](#wiki-servicebus-doc-test-section-01)
- [101. الگوی URL](#wiki-servicebus-doc-test-section-02)
- [102. خطاهای HTTP](#wiki-servicebus-doc-test-section-03)
- [103. مثال کامل: Provider با پارامتر ثابت و Service Bus محدود](#wiki-servicebus-doc-test-section-04)
- [104. تست Service Bus](#wiki-servicebus-doc-test-section-07)
- [105. مرز Provider و Service Bus](#wiki-servicebus-doc-test-section-08)

## 100. سند خودکار هر Service Bus

برای هر Service Bus، GraphBPMS یک سند مصرف اختصاصی تولید می‌کند. این سند می‌تواند مستقیماً به توسعه‌دهنده، تیم Integration، مشتری یا Consumer بیرونی تحویل داده شود.

سند شامل موارد زیر است:

- نام سرویس؛

- وضعیت فعال/غیرفعال؛

- Anonymous بودن یا نبودن؛

- فهرست کاربران مجاز؛

- URL نهایی Endpoint؛

- HTTP Method؛

- Content-Type؛

- ساختار Request؛

- ساختار Response؛

- خطاهای HTTP؛

- نحوه دریافت Token در صورت نیاز؛

- نمونه کد برای `cURL`، `C#`، `fetch` و `jQuery`.

## 101. الگوی URL

سند تولیدشده URL اجرای سرویس را با ساختاری مشابه زیر نمایش می‌دهد:

```
/servicebus/run/<ServiceBusName>

```

مثال:

```
/servicebus/run/GetAllActiveUsersWithOutStaticParamtersInput

```

Domain و Port به Deployment مقصد وابسته‌اند.

## 102. خطاهای HTTP

برای سرویس‌های غیر Anonymous:

Status Code
وضعیت
معنی

`401`
`UnAuthorized`
کاربر Login نکرده یا Token معتبر ندارد

`403`
`Forbidden`
کاربر Login کرده ولی در فهرست کاربران مجاز این سرویس نیست

`500`
Internal Error
خطا هنگام اجرای Service Bus / Provider

متن خطا برای `500` در Body پاسخ ارائه می‌شود. همچنین در قرارداد فعلی، متن‌هایی که در Status Description قرار می‌گیرند باید با محدودیت‌های Encoding همان مسیر سازگار باشند؛ متن فارسی در Body پاسخ قابل اتکاتر است.

## 103. مثال کامل: Provider با پارامتر ثابت و Service Bus محدود

فرض کنید Provider زیر وجود دارد:

```
Provider:
GetAllActiveUsers

Parameters:
FromUserId
Status

Output:
UserID
FullName

```

دو Service Bus می‌توان ساخت:

### Service Bus A

```
Name: GetAllActiveUsers
Method: GET
Response: application/json
Anonymous: true
Static Parameters: ندارد

```

Consumer پارامترهای لازم را می‌فرستد و سرویس عمومی است.

### Service Bus B

```
Name: GetAllActiveUsersWithOutStaticParamtersInput
Method: POST
Response: application/json
Anonymous: false
Allowed Users: User A, User B
Static Parameter:
FromUserId = 1000

```

در سند Service Bus B، `FromUserId` در Request Parameters نمایش داده نمی‌شود چون مقدار آن از Backend تأمین می‌شود.

## 104. تست Service Bus

پس از ساخت سرویس:

- سند تولیدشده را باز کنید.

- URL، Method و Content-Type را کنترل کنید.

- اگر Anonymous نیست، Token بگیرید.

- Request نمونه را با Postman/cURL/fetch اجرا کنید.

- خروجی را با Test مستقیم Provider مقایسه کنید.

- برای Static Parameter بررسی کنید Consumer نتواند مقدار نهایی Backend را تغییر دهد.

- خطاهای 401، 403 و 500 را نیز تست کنید.

## 105. مرز Provider و Service Bus

Provider منطق Data Access/Integration را تعریف می‌کند؛ Service Bus همان Provider را به یک قرارداد HTTP بیرونی تبدیل می‌کند.

```
Provider = What to execute
Service Bus = How external consumers call it

```

این جداسازی اجازه می‌دهد یک Provider در Form/Custom JS مصرف شود و هم‌زمان چند API بیرونی با Contractهای متفاوت روی آن ساخته شود.

Provider در GraphBPMS فقط یک Query ساده نیست؛ یک لایه قرارداد داده و Integration است که می‌تواند بین Form/Custom JS و Database/REST/WSDL قرار گیرد و در صورت نیاز از طریق Service Bus به API قابل مصرف بیرونی تبدیل شود.

الگوی پیشنهادی پیاده‌سازی:

```
اول Provider استاندارد بساز
↓
پارامتر و خروجی را دقیق تعریف کن
↓
در خود Designer تست کن
↓
برای Dataset بزرگ Paging فعال کن
↓
در Custom JS از absoluteMethodHelper.runProvider استفاده کن
↓
برای File به File/Attachment Contract توجه کن
↓
خطا را با Promise/Catch مدیریت کن
↓
Secret و داده حساس را Client-Side نبر
↓
در صورت نیاز Provider را از طریق Service Bus به API مستند منتشر کن

```

برای Providerهای معمولی، `absoluteMethodHelper.runProvider` API اصلی سمت Client در مستندات و کد فعلی بررسی‌شده است. Helper جاری علاوه بر Providerهای ساده، حالت بدون Parameter، File Input، File Output و ترکیب File Input + File Output را پوشش می‌دهد.

در لایه امنیت ساخت Provider نیز سه منبع محدودیت از هم تفکیک شده‌اند: `web.config`، System Settings و Permissionهای Form/SubForm. این کنترل‌ها در Create/Edit اعمال می‌شوند و نباید با Runtime Authorization اجرای Provider اشتباه گرفته شوند.

موارد وابسته به نسخه/استقرار در متن همان بخش مشخص شده‌اند و سایر رفتارهای Back-end این سند بر اساس پیاده‌سازی جاری یا نمونه اجرای واقعی ثبت شده‌اند.

**جمع‌بندی اجرایی:**
Provider در GraphBPMS هسته اتصال و اجرای داده است؛ Database Connection و WSDL Parser زیرساخت ورودی را آماده می‌کنند، انواع Provider قرارداد داده را می‌سازند، absoluteMethodHelper مسیر مصرف در Custom JS را فراهم می‌کند و Service Bus همان Provider را به API مستند و قابل مصرف بیرونی تبدیل می‌کند.
