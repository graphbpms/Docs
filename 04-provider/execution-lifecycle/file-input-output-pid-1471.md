---
id: 1471
title: "File Input، File Output و File Input + Output"
englishTitle: "File Streaming & Binary Payloads"
category: "04-provider/execution-lifecycle"
wikiPath: "1450/1468/1471"
wikiUrl: "https://wiki.graphbpms.ir/1450/1468/1471"
breadcrumbs: ["مدیریت منابع داده Provider","اجرای Provider در کد","File Input، File Output و File Input + Output"]
priority: 3
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# File Input، File Output و File Input + Output

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اجرای Provider در کد` ❯ `File Input، File Output و File Input + Output`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1468/1471](https://wiki.graphbpms.ir/1450/1468/1471)

در این آموزش

-   [30\. تشخیص File](#wiki-file-provider-section-01)
-   [31\. FormData خودکار](#wiki-file-provider-section-02)
-   [32\. گرفتن فایل از کاربر با global.getFileFromUser](#wiki-file-provider-section-03)
-   [33\. Provider با خروجی File](#wiki-file-provider-section-04)
-   [34\. یک Call که هم فایل می‌گیرد و هم فایل می‌دهد](#wiki-file-provider-section-07)
-   [40\. سناریو 5 — File Input](#wiki-file-provider-section-08)
-   [41\. سناریو 6 — File Output](#wiki-file-provider-section-09)
-   [42\. سناریو 7 — File Input + File Output](#wiki-file-provider-section-10)

## 30\. تشخیص File

Helper برای تشخیص فایل بررسی می‌کند:

```js
value instanceof File
```

نتیجه:

-   `File` مستقیم قابل ارسال است؛
-   `Blob` خام به‌تنهایی File محسوب نمی‌شود.

اگر Blob دارید:

```js
const file =
    new File(
        [myBlob],
        'document.bin'
    );
```

## 31\. FormData خودکار

اگر حتی یک Parameter دارای `File` باشد، `absoluteMethodHelper.runProvider` به‌صورت خودکار Request را با `FormData` می‌سازد.

بنابراین در Custom JS لازم نیست خودتان `FormData` بسازید.

```js
absoluteMethodHelper.runProvider(
    PROVIDER_ID,
    [
        {
            name: 'file',
            value: myFile
        },
        {
            name: 'description',
            value: 'Sample file'
        }
    ]
);
```

Helper:

-   ProviderId را اضافه می‌کند؛
-   File را به‌صورت multipart اضافه می‌کند؛
-   سایر Parameters را در همان FormData قرار می‌دهد.

## 32\. گرفتن فایل از کاربر با `global.getFileFromUser`

```js
global
    .getFileFromUser({
        accept: '*'
    })
    .then(myFile => {
        return absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'file',
                    value: myFile
                }
            ]
        );
    })
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

`myFile` باید یک `File` باشد.

## 33\. Provider با خروجی File

Helper فعلی Response را از روی Header بررسی می‌کند. اگر `Content-Disposition` وجود داشته باشد و شامل `attachment` باشد، Response به File تبدیل می‌شود.

ساختار Result ساخته‌شده توسط Helper:

```js
{
    HasError: false,
    Content: File,
    Message: null,
    MessageException: null
}
```

استفاده:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        const file = result.Content;

        saveAs(file, file.name);
    })
    .catch(error => {
        console.error(error);
    });
```

### 33.1. `saveAs`

`saveAs` در Sample Code فعلی محصول استفاده شده است، اما بخشی از `absoluteMethodHelper` نیست. آن را به‌عنوان Utility/Library سمت Client در نظر بگیرید.

### 33.2. نکته Header

اگر Endpoint Binary برگرداند ولی Header مناسب Attachment نداشته باشد، File Detection این Helper مطابق انتظار عمل نخواهد کرد.

Response مقصد باید در سناریوی File Output دارای قرارداد مناسب باشد، از جمله:

```text
Content-Disposition: attachment; filename="..."
```

## 34\. یک Call که هم فایل می‌گیرد و هم فایل می‌دهد

این سناریو در پیاده‌سازی جاری قابل انجام است.

Helper:

-   در Input وجود `File` را تشخیص می‌دهد و Request را `FormData` می‌کند؛
-   در Output Attachment را تشخیص می‌دهد و `File` می‌سازد.

نمونه:

```js
global
    .getFileFromUser({
        accept: '*'
    })
    .then(inputFile => {
        return absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'file',
                    value: inputFile
                },
                {
                    name: 'operation',
                    value: 'convert'
                }
            ]
        );
    })
    .then(result => {
        const outputFile = result.Content;

        saveAs(
            outputFile,
            outputFile.name
        );
    })
    .catch(error => {
        console.error(error);
    });
```

REST Provider باید `multipart/form-data` را برای Request داشته باشد و Endpoint خروجی را به‌صورت Attachment برگرداند.

این ترکیب در مسیر فنی جاری پشتیبانی می‌شود. طبیعتاً Endpoint مقصد باید Request از نوع `multipart/form-data` را بپذیرد و برای خروجی فایل، قرارداد Attachment مناسب را برگرداند؛ این شرط مربوط به Contract سرویس مقصد است، نه محدودیت `absoluteMethodHelper`.

## 40\. سناریو 5 — File Input

Provider REST:

```text
Method: POST
Request Content Type: multipart/form-data

file: File / form-data
```

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
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

## 41\. سناریو 6 — File Output

```js
absoluteMethodHelper
    .runProvider(
        PROVIDER_ID,
        [
            {
                name: 'someParam',
                value: 'sample'
            }
        ]
    )
    .then(result => {
        const file = result.Content;

        saveAs(
            file,
            file.name
        );
    })
    .catch(error => {
        console.error(error);
    });
```

## 42\. سناریو 7 — File Input + File Output

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
        const outputFile = result.Content;

        saveAs(
            outputFile,
            outputFile.name
        );
    })
    .catch(error => {
        console.error(error);
    });
```
