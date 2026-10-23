---
id: 1493
title: "FAQ"
englishTitle: "Integration FAQ & Known Patterns"
category: "04-provider/faq-reference"
wikiPath: "1450/1492/1493"
wikiUrl: "https://wiki.graphbpms.ir/1450/1492/1493"
breadcrumbs: ["مدیریت منابع داده Provider","FAQ و مرجع توسعه‌دهنده","FAQ"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# FAQ

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `FAQ و مرجع توسعه‌دهنده` ❯ `FAQ`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1492/1493](https://wiki.graphbpms.ir/1450/1492/1493)

در این آموزش

-   [79\. آیا Provider بدون Parameter قابل اجرا است؟](#wiki-faq-section-01)
-   [80\. آیا Provider بدون Output قابل اجرا است؟](#wiki-faq-section-02)
-   [81\. آیا Provider می‌تواند File دریافت کند؟](#wiki-faq-section-03)
-   [82\. آیا Provider می‌تواند File برگرداند؟](#wiki-faq-section-04)
-   [83\. آیا یک Provider می‌تواند هم File بگیرد و هم File برگرداند؟](#wiki-faq-section-05)
-   [84\. آیا Blob را مستقیم می‌توان داد؟](#wiki-faq-section-06)
-   [85\. آیا runProvider Result را مستقیم Array می‌دهد؟](#wiki-faq-section-07)
-   [86\. آیا Error را باید داخل .then بررسی کرد؟](#wiki-faq-section-08)
-   [87\. آیا Paging با absoluteMethodHelper انجام می‌شود؟](#wiki-faq-section-09)

## 79\. آیا Provider بدون Parameter قابل اجرا است؟

بله.

```js
absoluteMethodHelper.runProvider(PROVIDER_ID)
```

در Helper فعلی `parameters = []` مقدار پیش‌فرض است.

## 80\. آیا Provider بدون Output قابل اجرا است؟

بله؛ نمونه واقعی Advanced Editor بدون Output در محیط آزمایش ارائه شده است.

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

در این حالت الزاماً `providerResult` را Parse نکنید.

## 81\. آیا Provider می‌تواند File دریافت کند؟

بله.

در REST Provider:

```text
multipart/form-data
File
form-data
```

و در Client مقدار باید `File` باشد.

## 82\. آیا Provider می‌تواند File برگرداند؟

بله. REST Provider می‌تواند Response Type از نوع `File` داشته باشد. Helper فعلی Attachment را به `File` تبدیل کرده و در:

```js
result.Content
```

قرار می‌دهد.

## 83\. آیا یک Provider می‌تواند هم File بگیرد و هم File برگرداند؟

در Helper فعلی Client، هر دو مسیر هم‌زمان پشتیبانی می‌شوند:

```text
File Input → FormData
Attachment Output → File
```

بنابراین از دید Client Transport بله. Endpoint مقصد و تنظیم REST Provider باید همین Contract را پشتیبانی کنند.

## 84\. آیا Blob را مستقیم می‌توان داد؟

در `absoluteMethodHelper.runProvider` فعلی File Detection با:

```js
instanceof File
```

انجام می‌شود؛ Blob را به File تبدیل کنید:

```js
new File(
    [blob],
    'file.bin'
)
```

## 85\. آیا `runProvider` Result را مستقیم Array می‌دهد؟

در نمونه‌های فعلی خیر. Result Model دریافت می‌شود و Structured Data معمولاً در:

```js
result.Content.providerResult
```

است.

## 86\. آیا Error را باید داخل `.then` بررسی کرد؟

برای Helper فعلی بررسی‌شده، `HasError === true` باعث Reject می‌شود و `.catch` مرجع است.

در نسخه‌های قدیمی Wiki الگوهای متفاوت دیده می‌شوند؛ نسخه مقصد کنترل شود.

## 87\. آیا Paging با `absoluteMethodHelper` انجام می‌شود؟

در کد فعلی، Sample Paging با:

```js
providerDesigner.runProviderPaging(...)
```

تولید می‌شود و `runProviderPaging` در `absoluteMethodHelper.js` فعلی مشاهده نشد.
