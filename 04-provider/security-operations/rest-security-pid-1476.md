---
id: 1476
title: "امنیت REST — Secret، Constant/Default، Output حساس و SQL Injection"
englishTitle: "REST Security, Secrets & SQL Sanitization"
category: "04-provider/security-operations"
wikiPath: "1450/1474/1476"
wikiUrl: "https://wiki.graphbpms.ir/1450/1474/1476"
breadcrumbs: ["مدیریت منابع داده Provider","امنیت، دسترسی و عملیات","امنیت REST — Secret، Constant/Default، Output حساس و SQL Injection"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# امنیت REST — Secret، Constant/Default، Output حساس و SQL Injection

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `امنیت، دسترسی و عملیات` ❯ `امنیت REST — Secret، Constant/Default، Output حساس و SQL Injection`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1474/1476](https://wiki.graphbpms.ir/1450/1474/1476)

در این آموزش

-   [57\. Secret و Constant در REST](#wiki-rest-security-section-01)
-   [58\. Output حساس](#wiki-rest-security-section-02)
-   [59\. SQL Injection](#wiki-rest-security-section-03)

## 57\. Secret و Constant در REST

GraphBPMS Provider در وضعیت جاری، نوع جداگانه‌ای با مفهوم Secret/Vault برای Parameterهای REST ندارد. Parameterها همان Parameterهای معمولی هستند و Constant/Default Value نیز یک مقدار Static ذخیره‌شده در Provider است.

بنابراین این دو سناریو باید از هم تفکیک شوند:

```text
Client مقدار را ارسال می‌کند
→ همان مقدار برای Parameter استفاده می‌شود

Client مقدار مؤثر ارسال نمی‌کند
+ Provider برای Parameter مقدار Default دارد
→ مقدار Default ذخیره‌شده در Provider استفاده می‌شود
```

نمونه نامناسب برای Credential حساس در Client:

```js
absoluteMethodHelper.runProvider(
    PROVIDER_ID,
    [
        {
            name: 'Authorization',
            value: 'Bearer REAL_SECRET'
        }
    ]
);
```

این مقدار به‌عنوان داده Client-Side قابل مشاهده است و ممکن است در Logging پارامترهای Provider نیز ثبت شود.

قرار دادن همان مقدار به‌عنوان Constant/Default در Provider باعث می‌شود Client مجبور نباشد آن را در هر Call ارسال کند، اما **آن را به Secret محافظت‌شده تبدیل نمی‌کند**.

در این حالت مقدار Default به‌صورت Static در اطلاعات Provider در Database نگهداری می‌شود و فقط زمانی جایگزین مقدار اجرا می‌شود که برای Parameter مقدار مؤثری ارسال نشده باشد.

**\[راهکار عمومی امنیت\]** اگر Credential واقعاً حساس است، محل امن نگهداری آن باید در معماری Server-Side/Configuration/Secret Management پروژه حل شود، نه با فرض Secret بودن Constant Provider.

## 58\. Output حساس

Providerهایی که به جدول User یا Security متصل می‌شوند نباید بی‌دلیل فیلدهای حساس برگردانند.

از خروجی دادن مواردی مانند:

```text
Password
Private Key
Security Stamp
Token
Connection String
```

خودداری شود.

اصل پیشنهادی:

```text
Least Data
```

فقط ستون‌هایی را Select کنید که Consumer واقعاً نیاز دارد.

## 59\. SQL Injection

**\[راهکار عمومی امنیت SQL\]**

Query را با الحاق مستقیم مقدار کاربر نسازید.

بد:

```js
const query =
    "SELECT * FROM Users WHERE Name = '" +
    userValue +
    "'";
```

به‌جای آن در Provider از Parameter استفاده کنید:

```sql
WHERE Name = @Name
```

و در Client:

```js
{
    name: 'Name',
    value: userValue
}
```

ارسال شود.
