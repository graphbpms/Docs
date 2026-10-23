---
id: 1494
title: "نمونه Integration برای File → File"
englishTitle: "End-to-End File-to-File Integration Pattern"
category: "04-provider/faq-reference"
wikiPath: "1450/1492/1494"
wikiUrl: "https://wiki.graphbpms.ir/1450/1492/1494"
breadcrumbs: ["مدیریت منابع داده Provider","FAQ و مرجع توسعه‌دهنده","نمونه Integration برای File → File"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# نمونه Integration برای File → File

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `FAQ و مرجع توسعه‌دهنده` ❯ `نمونه Integration برای File → File`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1492/1494](https://wiki.graphbpms.ir/1450/1492/1494)

در این آموزش

-   [88\. الگوی Integration برای File → File](#wiki-file-integration-section-01)

## 88\. الگوی Integration برای File → File

الگوی File → File از نظر مسیر Input/Output قابل استفاده است. برای تست Contract یک Endpoint واقعی یا آزمایشگاهی، می‌توان Provider کم‌ریسک زیر را ساخت:

```text
Name:
EchoOrTransformFile

Type:
REST

Method:
POST

Request Content Type:
multipart/form-data

Input:
file   File   form-data

Response:
File
```

Endpoint تست:

1.  File را دریافت کند؛
2.  همان File یا نسخه Transform‌شده را برگرداند؛
3.  Header مناسب را تنظیم کند:

```text
Content-Disposition: attachment; filename="output.bin"
```

Client:

```js
global
    .getFileFromUser({
        accept: '*'
    })
    .then(file => {
        return absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'file',
                    value: file
                }
            ]
        );
    })
    .then(result => {
        saveAs(
            result.Content,
            result.Content.name
        );
    });
```

این Test Case یکجا اثبات می‌کند:

```text
File Picker
→ File Input
→ multipart/form-data
→ File Output
→ Download
```
