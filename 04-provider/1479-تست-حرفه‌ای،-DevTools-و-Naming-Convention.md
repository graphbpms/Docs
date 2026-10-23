---
id: 1479
graphDocId: 1540
title: "تست حرفه‌ای، DevTools و Naming Convention"
subTitle: null
module: "04-provider"
wikiPath: "1450/1474/1479"
wikiUrl: "https://wiki.graphbpms.ir/1450/1474/1479"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1474/1479"
breadcrumbs: ["مدیریت منابع داده Provider","امنیت، دسترسی و عملیات","تست حرفه‌ای، DevTools و Naming Convention"]
parentId: 1474
parentTitle: "امنیت، دسترسی و عملیات"
url: "1479"
priority: 5
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۵. امنیت، دسترسی و عملیات. امنیت، دسترسی و عملیات. GraphBPMS • Provider تست حرفه‌ای، DevTools و Naming Convention در این آموزش 71. Test Caseهای حداقلی 72. تست در DevTools 73. Naming پیشنهادی 71. Test Caseهای حداقلی Test نتیجه مورد انتظار ورودی معتبر Success ورودی خالی رفتار تعریف‌شده ورودی نامعتبر خطای کنترل‌شده بدون رکورد Empty Result بدون Crash چند رکورد Schema صحیح Nullable مقدار Null صحیح خطای DB/REST Promise Reject / Error دسترسی Create/Edit کاربر غیرمجاز نتواند…. محورهای اصلی: 71. Test Caseهای حداقلی؛ 72. تست در DevTools؛ 73. Naming پیشنهادی."
updatedAt: "2026-10-18T16:08:51"
---

# تست حرفه‌ای، DevTools و Naming Convention

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `امنیت، دسترسی و عملیات` ❯ `تست حرفه‌ای، DevTools و Naming Convention`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1474/1479](https://wiki.graphbpms.ir/1450/1474/1479)

در این آموزش

-   [71\. Test Caseهای حداقلی](#wiki-test-naming-section-01)
-   [72\. تست در DevTools](#wiki-test-naming-section-02)
-   [73\. Naming پیشنهادی](#wiki-test-naming-section-03)

## 71\. Test Caseهای حداقلی

| Test | نتیجه مورد انتظار |
| --- | --- |
| ورودی معتبر | Success |
| ورودی خالی | رفتار تعریف‌شده |
| ورودی نامعتبر | خطای کنترل‌شده |
| بدون رکورد | Empty Result بدون Crash |
| چند رکورد | Schema صحیح |
| Nullable | مقدار Null صحیح |
| خطای DB/REST | Promise Reject / Error |
| دسترسی Create/Edit | کاربر غیرمجاز نتواند Query/Operation ممنوع را در زمان ایجاد یا ویرایش Provider ثبت کند |
| Paging صفحه اول | داده صحیح |
| Paging صفحه آخر | داده صحیح |
| Sort | ترتیب صحیح |
| Filter | خروجی صحیح |
| File Input | Upload صحیح |
| File Output | Download صحیح |
| File Input + Output | End-to-End صحیح |

## 72\. تست در DevTools

برای REST/File سناریوها Network Tab بسیار مفید است.

موارد مهم:

```text
Request Method
Request Content-Type
Form Data
Request Headers
Response Status
Response Content-Type
Content-Disposition
Response Size
```

از Screenshotی که Secret واقعی دارد در Wiki استفاده نکنید.

## 73\. Naming پیشنهادی

الگو:

```text
<Action><Entity><Qualifier>
```

مثال:

```text
GetActiveUsers
GetCitiesByState
UpdateRequestStatus
ArchiveCompletedTasks
DownloadContractFile
UploadAndConvertDocument
```

برای Provider دارای Side Effect، Action را در نام مشخص کنید:

```text
Update...
Delete...
Archive...
Send...
Create...
```

تا با Provider Read-Only اشتباه نشود.
