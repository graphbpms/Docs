---
id: 1472
title: "Error Handling، Transport داخلی، Paging و Lifecycle فرم"
englishTitle: "Form Lifecycle & Event Transport"
category: "04-provider/execution-lifecycle"
wikiPath: "1450/1468/1472"
wikiUrl: "https://wiki.graphbpms.ir/1450/1468/1472"
breadcrumbs: ["مدیریت منابع داده Provider","اجرای Provider در کد","Error Handling، Transport داخلی، Paging و Lifecycle فرم"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Error Handling، Transport داخلی، Paging و Lifecycle فرم

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اجرای Provider در کد` ❯ `Error Handling، Transport داخلی، Paging و Lifecycle فرم`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1468/1472](https://wiki.graphbpms.ir/1450/1468/1472)

در این آموزش

-   [44\. رفتار خطا در Helper جاری](#wiki-runtime-advanced-section-01)
-   [45\. تفاوت با نمونه‌های قدیمی Wiki](#wiki-runtime-advanced-section-02)
-   [46\. چه چیزهایی را runProvider خودش انجام می‌دهد؟](#wiki-runtime-advanced-section-03)
-   [47\. providerDesigner.runProviderPaging](#wiki-runtime-advanced-section-05)
-   [48\. Mapping پارامترهای Paging](#wiki-runtime-advanced-section-07)
-   [48\. Custom JS در سطح فرم و محل استفاده از absoluteMethodHelper](#wiki-runtime-advanced-section-08)
-   [49\. اجرای Provider در onLoad](#wiki-runtime-advanced-section-09)
-   [50\. اجرای Provider قبل از Submit](#wiki-runtime-advanced-section-10)
-   [51\. اجرای Provider بعد از Submit موفق](#wiki-runtime-advanced-section-11)

## 44\. رفتار خطا در Helper جاری

در نسخه فعلی بررسی‌شده اگر Response JSON دارای:

```js
HasError === true
```

باشد، Helper:

1.  MessageException را برای نمایش آماده می‌کند؛
2.  `toast.showResult(result)` را اجرا می‌کند؛
3.  Promise را Reject می‌کند.

الگوی پیشنهادی برای همین Build:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        // success
    })
    .catch(error => {
        // GraphBPMS error or network error
        console.error(error);
    });
```

## 45\. تفاوت با نمونه‌های قدیمی Wiki

**\[وابسته به نسخه/استقرار\]**

در بعضی نمونه‌های Wiki الگوی زیر دیده می‌شود:

```js
absoluteMethodHelper
    .runProvider(...)
    .then(result => {
        if (result.HasError) {
            toast.showResult(result);
            return;
        }
    });
```

اما Helper فعلی ارسالی `HasError` را Reject می‌کند.

برای پروژه Legacy رفتار Helper همان نسخه کنترل شود؛ برای Build جاری ارسالی، `.catch` جزو الگوی استاندارد است.

## 46\. چه چیزهایی را `runProvider` خودش انجام می‌دهد؟

Helper خودش:

-   درخواست را به `/Provider/RunProvider` می‌فرستد؛
-   Anti-forgery Token را از صفحه دریافت و در Header قرار می‌دهد؛
-   برای Request معمولی JSON می‌فرستد؛
-   برای Request دارای File از `FormData` استفاده می‌کند؛
-   Attachment Response را به `File` تبدیل می‌کند؛
-   خطای GraphBPMS را Reject می‌کند.

### نتیجه

در Custom JS مستقیماً Endpoint داخلی را با `fetch` یا `$.ajax` فراخوانی نکنید، مگر API رسمی دیگری برای همان Build مستند شده باشد. API استانداردتر:

```js
absoluteMethodHelper.runProvider(...)
```

است.

## 47\. `providerDesigner.runProviderPaging`

نمونه JavaScript تولیدشده توسط Provider Details برای Providerهای Paging از:

```js
providerDesigner.runProviderPaging(...)
```

استفاده می‌کند.

امضای مشاهده‌شده:

```js
providerDesigner.runProviderPaging(
    providerId,
    parameters,
    pageIndex,
    pageSize,
    pagingMainFilter,
    pagingFilters,
    pagingOrders
)
```

نمونه:

```js
providerDesigner
    .runProviderPaging(
        PROVIDER_ID,
        null,
        0,
        10,
        '',
        [
            {
                name: 'UserName',
                value: ''
            }
        ],
        [
            {
                name: 'UserName',
                value: 'asc'
            }
        ]
    )
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.warn(error);
    });
```

### نکته مهم

`runProviderPaging` در فایل `absoluteMethodHelper.js` فعلی مشاهده نشد.

```text
Non-Paging Provider
    → absoluteMethodHelper.runProvider

Paging Provider
    → providerDesigner.runProviderPaging
       در Sample Generator فعلی محصول
```

این دو API را یکی نکنید.

## 48\. Mapping پارامترهای Paging

اگر Provider با `PagingUseParameters` تنظیم شده باشد، UI فعلی پارامترهای Paging را از لیست پارامترهای عادی جدا می‌کند و از آرگومان‌های Paging مقداردهی می‌کند تا Parameter دوبار ارسال نشود.

## 48\. Custom JS در سطح فرم و محل استفاده از absoluteMethodHelper

در Form Builder سامانه GraphBPMS برای هر فرم می‌توان فایل `Custom JS` تعریف کرد. این فایل محل نوشتن منطق‌های JavaScript اختصاصی همان فرم است و از طریق مجموعه‌ای از Hookهای Lifecycle به چرخه اجرای فرم متصل می‌شود. این Hookها در زمان‌های مشخصی از عمر فرم فراخوانی می‌شوند؛ بنابراین می‌توان منطق مورد نیاز را دقیقاً در زمان Load، قبل از Submit، بعد از Submit موفق، هنگام خطای Submit و سایر نقاط مستندشده اجرا کرد.

**رابطه با Provider:** از آنجا که `absoluteMethodHelper` در Custom JS فرم قابل استفاده است، متد `absoluteMethodHelper.runProvider(...)` نیز می‌تواند داخل همین Lifecycle Hookها فراخوانی شود. در نتیجه اجرای Provider محدود به یک دکمه یا رویداد خاص نیست و می‌توان آن را در نقطه مناسب از چرخه فرم قرار داد.

| Hook | زمان اجرا | نمونه کاربرد Provider |
| --- | --- | --- |
| `onLoad` | پس از Load شدن فرم | دریافت داده اولیه، وضعیت‌ها، تنظیمات یا پرکردن/کنترل فیلدها بر اساس خروجی Provider. |
| `beforeSubmit` | بعد از اقدام کاربر برای ارسال فرم و قبل از ارسال داده به Server | اعتبارسنجی سمت Client با Provider و تصمیم درباره ادامه یا توقف Submit. این Hook می‌تواند `false` یا Promise با مقدار `false` برگرداند. |
| `onSubmitSuccess` | بعد از Submit موفق فرم | اجرای عملیات تکمیلی وابسته به ثبت موفق فرم؛ برای مثال اجرای Provider دیگری که فقط پس از ذخیره موفق باید انجام شود. |
| `onSubmitFailed` | در صورت شکست Submit | اجرای منطق تکمیلی یا ثبت/نمایش اطلاعات مرتبط با خطا، در صورت نیاز. |

**نکته نام‌گذاری:** برای سناریوی «بعد از ارسال موفق»، Hook مستند GraphBPMS با نام `onSubmitSuccess` استفاده می‌شود.

Form Custom JS ← Lifecycle Hook ← absoluteMethodHelper ← runProvider(...) ← Provider Result

## 49\. اجرای Provider در `onLoad`

```js
export const onLoad = function (viewModel) {
    absoluteMethodHelper
        .runProvider(PROVIDER_ID)
        .then(result => {
            const rows =
                JSON.parse(
                    result.Content.providerResult || '[]'
                );

            // Use rows
        })
        .catch(error => {
            console.error(error);
        });
};
```

کاربرد:

-   دریافت داده اولیه؛
-   بررسی وضعیت؛
-   دریافت تنظیمات؛
-   Load یک منبع خارجی.

Provider سنگین را بی‌دلیل در `onLoad` اجرا نکنید؛ این کار زمان باز شدن فرم را افزایش می‌دهد.

## 50\. اجرای Provider قبل از Submit

اگر نتیجه Provider باید تعیین کند فرم ارسال شود یا نه:

```js
export const beforeSubmit = function (viewModel) {
    return absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'Code',
                    value: viewModel.data().Code()
                }
            ]
        )
        .then(result => {
            const rows =
                JSON.parse(
                    result.Content.providerResult || '[]'
                );

            if (
                rows.length > 0 &&
                rows[0].IsExists
            ) {
                toast.show(
                    'error',
                    'این مقدار قبلاً ثبت شده است'
                );

                return false;
            }

            return true;
        })
        .catch(error => {
            console.error(error);
            return false;
        });
};
```

این الگو با Lifecycle Promise-based فرم سازگار است.

## 51\. اجرای Provider بعد از Submit موفق

اگر اجرای Provider باید فقط بعد از ثبت موفق فرم انجام شود، محل مناسب Hook `onSubmitSuccess` است. در این حالت ابتدا Submit اصلی فرم با موفقیت انجام می‌شود و سپس منطق تکمیلی شما اجرا خواهد شد.

```js
export const onSubmitSuccess = function (viewModel) {
    absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'RecordId',
                    value: viewModel.data().Id()
                }
            ]
        )
        .then(result => {
            console.log(result);
        })
        .catch(error => {
            console.error(error);
        });
};
```

**انتخاب Hook مناسب مهم است:** اگر نتیجه Provider باید تعیین کند که فرم اصلاً ارسال شود یا نه، از `beforeSubmit` استفاده کنید. اگر Provider فقط پس از Submit موفق باید اجرا شود، از `onSubmitSuccess` استفاده کنید. اجرای Provider سنگین در `onLoad` نیز باید فقط زمانی انجام شود که واقعاً برای شروع کار فرم لازم است.
