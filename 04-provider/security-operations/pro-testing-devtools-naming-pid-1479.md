---
id: 1479
title: "تست حرفه‌ای، DevTools و Naming Convention"
englishTitle: "Enterprise Testing & DevTools Inspection"
category: "04-provider/security-operations"
wikiPath: "1450/1474/1479"
wikiUrl: "https://wiki.graphbpms.ir/1450/1474/1479"
breadcrumbs: ["مدیریت منابع داده Provider","امنیت، دسترسی و عملیات","تست حرفه‌ای، DevTools و Naming Convention"]
priority: 5
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# تست حرفه‌ای، DevTools و Naming Convention

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `امنیت، دسترسی و عملیات` ❯ `تست حرفه‌ای، DevTools و Naming Convention`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1474/1479](https://wiki.graphbpms.ir/1450/1474/1479)

# تست حرف��‌ای، DevTools و Naming Convention

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
