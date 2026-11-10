---
id: 1494
title: "نمونه Integration برای File → File"
englishTitle: "نمونه Integration برای File → File"
category: "09-integration-file-file"
wikiUrl: "https://wiki.graphbpms.ir/1494"
breadcrumbs: ["نمونه Integration برای File → File"]
priority: 2
updatedAt: "2026-10-25T21:33:55"
---

# نمونه Integration برای File → File

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `نمونه Integration برای File → File`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1494](https://wiki.graphbpms.ir/1494)

# نمونه Integration برای File → File
در این آموزش
- [88. الگوی Integration برای File → File](#wiki-file-integration-section-01)

## 88. الگوی Integration برای File → File

الگوی File → File از نظر مسیر Input/Output قابل استفاده است. برای تست Contract یک Endpoint واقعی یا آزمایشگاهی، می‌توان Provider کم‌ریسک زیر را ساخت:

```
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

- File را دریافت کند؛

- همان File یا نسخه Transform‌شده را برگرداند؛

- Header مناسب را تنظیم کند:

```
Content-Disposition: attachment; filename="output.bin"

```

Client:

```
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

```
File Picker
→ File Input
→ multipart/form-data
→ File Output
→ Download

```
