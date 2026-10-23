---
id: 1498
title: "Custom JavaScript و تنظیمات سفارشی"
englishTitle: "Custom JavaScript & Extension"
category: "01-form-builder/form-studio/custom-development"
wikiPath: "1423"
wikiUrl: "https://wiki.graphbpms.ir/1423"
breadcrumbs: ["فرم‌ساز پیشرفته","استودیوی فرم‌ساز","توسعه و سفارشی‌سازی","Custom JavaScript و تنظیمات سفارشی"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:27:09"
---

# Custom JavaScript و تنظیمات سفارشی

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `فرم‌ساز پیشرفته` ❯ `استودیوی فرم‌ساز` ❯ `توسعه و سفارشی‌سازی` ❯ `Custom JavaScript و تنظیمات سفارشی`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1423](https://wiki.graphbpms.ir/1423)

# تنظیمات سفارشی فرم در GraphBPMS

این مقاله مرجع توسعه‌دهندگان برای سفارشی‌سازی فرم‌های ساخته‌شده با **فرم ساز** در GraphBPMS است. محور اصلی آن **Custom JS**، `viewModel`، Instance فیلدها، رویدادها، `absoluteMethodHelper`، کار با Promise و `async/await` و همچنین **اسکریپت سمت سرور C#** است.

برای استفاده از Custom JS باید با JavaScript آشنا باشید و آشنایی با Knockout.js نیز توصیه می‌شود. این راهنما برای توسعه‌دهندگانی است که Custom JavaScript و تنظیمات پیشرفته فرم را پیاده‌سازی می‌کنند؛ بنابراین نام API، Property، Event و مسیرهای فنی در مثال‌ها به همان شکل فنی حفظ شده‌اند.

## در این آموزش

در این آموزش

-   [آشنایی با تب تنظیمات سفارشی در استدیو فرم ساز](#custom-settings-tab)
-   [مدل ذهنی توسعه Custom JS در GraphBPMS](#mental-model)
-   [چرخه اجرای فرم و Hookهای Custom JS](#lifecycle-hooks)
-   [معماری viewModel و مسیرهای دسترسی](#viewmodel)
-   [رویدادهای Component از مسیر \_Instance](#component-events)
-   [الگوهای پایه کار با داده و وضعیت فیلدها](#viewmodel-patterns)
-   [کار مستقیم با Instance و Componentهای UI](#component-instances)
-   [الگوهای اختصاصی فیلدهای پیچیده در فرم ساز GraphBPMS](#complex-fields)
-   [absoluteMethodHelper و Integrationهای سمت Client](#absolute-method-helper)
-   [کار با Toast، Alert و Confirm](#toast-dialog)
-   [برنامه‌نویسی ناهمزمان در JavaScript](#async-js)
-   [اسکریپت سمت سرور C#](#server-csharp)
-   [پیوست — فایل نمونه کامل Custom JS در استدیو فرم ساز](#studio-custom-js-sample)

## آشنایی با تب تنظیمات سفارشی در استدیو فرم ساز

تب **تنظیمات سفارشی** در استدیو فرم ساز سه مسیر اصلی برای توسعه رفتار و ظاهر فرم در اختیار شما قرار می‌دهد: اسکریپت سمت سرور C#، اسکریپت سمت کلاینت JavaScript و استایل سفارشی CSS. هر بخش با Switch مربوط فعال می‌شود و از طریق دکمه **IDE** به محیط ویرایش کد همان بخش دسترسی دارید.

![سه بخش تنظیمات سفارشی شامل C#، JavaScript و CSS در استدیو فرم ساز](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=BF4EECFE-B884-469E-8B11-1AF97055D55C)

سه بخش اصلی تب تنظیمات سفارشی: اسکریپت سمت سرور، Custom JS و Custom CSS.

### 1) اسکریپت سمت سرور C#

با فعال کردن گزینه **استفاده از کد C#**، IDE مربوط به اسکریپت سمت سرور در دسترس قرار می‌گیرد. توسعه‌دهنده می‌تواند برای رخدادهای Server-side فرم، بر اساس ساختار C# Script فرم، کد مورد نیاز را بنویسد.

ساختار کلاس، متدهای چرخه اجرا و نمونه‌های این بخش در فصل [اسکریپت سمت سرور C#](#server-csharp) همین مقاله توضیح داده شده است.

![محیط IDE اسکریپت سمت سرور C# در استدیو فرم ساز](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D026DE13-031A-4F2D-9174-3EEE710AB54A)

محیط ویرایش C# Script در استدیو فرم ساز.

### 2) اسکریپت سمت کلاینت یا Custom JS

بخش دوم برای **سفارشی‌سازی JavaScript سمت کلاینت** است و بخش اصلی این مقاله روی همین محیط تمرکز دارد. با فعال کردن گزینه **استفاده از کد JavaScript** و انتخاب **IDE**، محیط کدنویسی Custom JS باز می‌شود.

در ادامه، ساختار `viewModel`، داده و وضعیت فیلدها، `_Instance`، Eventها، Hookها، Providerها و Helperهای مورد استفاده در این محیط به‌صورت کامل بررسی می‌شوند.

![محیط IDE مربوط به Custom JavaScript در استدیو فرم ساز](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=100C7D10-640B-4402-B4F1-18B02917D485)

محیط ویرایش Custom JS در استدیو فرم ساز.

### 3) تنظیمات نمایشی یا Custom CSS

بخش **تنظیمات نمایشی** برای تعریف CSS سفارشی فرم استفاده می‌شود. با فعال کردن گزینه **استفاده از کد CSS** و انتخاب **IDE**، می‌توانید استایل‌های سفارشی مورد نیاز فرم را تعریف کنید.

با توجه به قابلیت درج Attribute و Class برای المان‌ها و کنترل‌های فرم‌ساز، می‌توانید همان Classها را در Custom CSS هدف قرار دهید و ظاهر کنترل‌ها یا بخش‌های مختلف فرم را تغییر دهید.

![محیط IDE مربوط به Custom CSS در استدیو فرم ساز](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4EC0F2D4-A2D8-4B57-A57F-D7E80186EA4B)

محیط ویرایش Custom CSS در استدیو فرم ساز.

**نکته:** اگر در فرم از **HTML Block** استفاده کنید، Classها و Styleهای آن نیز از طریق Custom CSS قابل هدف‌گیری هستند. در نتیجه می‌توانید استایل بخش‌های HTML و در صورت نیاز ظاهر کلی فرم را نیز سفارشی کنید.

## مدل ذهنی توسعه Custom JS در GraphBPMS

برای اینکه کدهای سفارشی قابل نگهداری بمانند، بهتر است مسئله را از چهار لایه ببینید: **زمان اجرا**، **داده و وضعیت فرم**، **Component رابط کاربری** و **Integration**. این تفکیک کمک می‌کند برای هر کار از مسیر مناسب استفاده شود و کد به DOM یا Selectorهای شکننده وابسته نشود.

**1\. Lifecycle Hook**`onLoad`، `beforeSubmit`، Hookهای Tab/SubForm و...

←

**2\. viewModel**`data()` برای داده، `fields` برای وضعیت، `events` برای رویدادهای GraphBPMS

←

**3\. \_Instance**فقط وقتی به Option / Method / Event خود Component نیاز دارید

←

**4\. Integration**`absoluteMethodHelper` برای Provider و Helperهای مستندشده

**قاعده پیشنهادی:** برای تغییر مقدار داده ابتدا `viewModel.data()` را ترجیح دهید؛ برای Visibility/ReadOnly از مسیرهای GraphBPMS استفاده کنید؛ و فقط برای قابلیت‌های خود Component سراغ `_Instance` بروید.

## چرخه اجرای فرم و Hookهای Custom JS

**Custom JS** یک فایل سفارشی‌سازی برای فرم‌های ساخته‌شده با ماژول فرم ساز است. با این فایل می‌توانید قابلیت‌هایی را که در فرم‌ساز به‌صورت Visual ارائه نشده‌اند، با JavaScript به فرم اضافه کنید.

وقتی برای اولین بار فایل Custom JS را در فرم می‌سازید، تعدادی Function به‌صورت پیش‌فرض در آن وجود دارد. این Functionها مانند قلاب، کدهای Custom JS را به بخش‌های مختلف فرم‌ساز متصل می‌کنند. لازم نیست همه این Functionها در فایل شما باقی بمانند؛ می‌توانید Functionهایی را که استفاده نمی‌کنید حذف کنید تا فایل شلوغ نشود.

### `onLoad`

```
export const onLoad = function (viewModel) {
    // ...
}
```

این Function به‌محض Load شدن فرم اجرا می‌شود. برای مثال اگر بخواهید با Load شدن فرم یک Provider اجرا شود و با توجه به خروجی Provider پیامی به کاربر نمایش داده شود، از این Function استفاده می‌کنید. نحوه اجرای Provider در بخش‌های بعدی آمده است.

**نکته:** این Function پارامتری به نام `viewModel` دریافت می‌کند که در بخش `viewModel` به‌صورت کامل بررسی می‌شود.

### `onReady`

```
export const onReady = function (viewModel) {
    // ...
}
```

این Hook در زمان آماده‌شدن فرم فراخوانی می‌شود. زمانی از آن استفاده کنید که منطق شما باید بعد از آماده‌شدن فرم اجرا شود. مانند سایر Hookهای Custom JS، پارامتر `viewModel` همان View Model فرم جاری است.

### `beforeSubmit`

```
export const beforeSubmit = function (viewModel) {
    // ...
    return true; // یا false
}
```

این تابع زمانی اجرا می‌شود که کاربر روی دکمه ارسال فرم کلیک کرده، اما داده‌های فرم هنوز به سرور ارسال نشده‌اند.

> **نکته مهم:** اگر این تابع مقدار `false` یا Promiseای با مقدار `false` برگرداند، فرم ارسال نمی‌شود.

در مثال زیر، قبل از ارسال فرم از کاربر تأیید گرفته می‌شود:

```
export const beforeSubmit = function(viewModel) {
    return new Promise(resolve => {
        DevExpress.ui.dialog.confirm('هشدار', 'از ارسال فرم اطمینان دارید؟').done(isOK => {
            if (isOK) {
                resolve(true);// فرم ارسال شود
            } else {
                resolve(false);// فرم ارسال نشود
            }
        });
    });
}
```

![نتیجه تأیید قبل از ارسال فرم](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=6215CF83-7DE2-4127-9639-1C8CF35D852B&download=true)

نتیجه تأیید قبل از ارسال فرم

### `onSubmitSuccess`

```
export const onSubmitSuccess = function (viewModel) {
    console.log('Submiting done from custom js');
}
```

این تابع زمانی اجرا می‌شود که فرم با موفقیت ارسال شده و عملیات اصلی فرم، یعنی ثبت داده در پایگاه داده، انجام شده است. از آن می‌توان برای اجرای رخداد دیگری بعد از Submit، مانند نمایش یک فرم دیگر یا شروع یک فرایند خارجی، استفاده کرد.

> **نکته مهم:** اگر این تابع مقدار `false` یا Promiseای با مقدار `false` برگرداند، صفحه Reload نخواهد شد.

برای هدایت کاربر به Dashboard یک ثانیه بعد از ثبت موفق فرم:

```
export const onSubmitSuccess = function(viewModel) {
    return new Promise(resolve => {
        resolve(false);// جلوگیری از رفرش شدن صفحه
        
        setTimeout(() => {
            location.href = '/Dashboard';// هدایت کاربر به صفحه داشبورد بعد از 1 ثانیه
        }, 1000);
    });
}
```

![نتیجه هدایت کاربر بعد از ثبت موفق فرم](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=430B734A-3782-4AB7-A519-CE6C0DF7F246&download=true)

نتیجه هدایت کاربر بعد از ثبت موفق فرم

### `onSubmitFailed`

```
export const onSubmitFailed = function (viewModel, error) {
    console.error('Submiting failed from custom js', error);
}
```

پارامتر **`error`** خطایی است که رخ داده است. این تابع زمانی اجرا می‌شود که فرایند ارسال فرم به هر دلیلی با خطا مواجه شود و می‌توانید از آن برای مدیریت خطای پیش‌آمده استفاده کنید.

### رویدادهای SubForm

#### `beforeSubSubmit`

```
export const beforeSubSubmit = function (name, record, viewModel) {
    // ...
}
```

-   **`name`**: نام فیلد تودرتو (SubForm).
-   **`record`**: رکوردی که کاربر قصد ارسال آن را دارد.

این تابع زمانی اجرا می‌شود که کاربر قصد ارسال رکورد در سطح SubForm را داشته باشد. اگر مقدار `false` یا Promiseای با مقدار `false` برگردانده شود، فرم ارسال نمی‌شود.

#### `onSubSubmit`

```
export const onSubSubmit = function (name, record, viewModel) {
    // ...
}
```

-   **`name`**: نام فیلد تودرتو (SubForm).
-   **`record`**: رکوردی که کاربر قصد ارسال آن را دارد.

این تابع زمانی اجرا می‌شود که فرم با موفقیت ارسال شده است؛ یعنی زمانی که عملیات اصلی فرم، ثبت داده در پایگاه داده، انجام شده است. برای مثال می‌توانید بعد از Submit رخداد دیگری اجرا کنید؛ مانند نمایش فرم دیگر یا شروع فرایند خارجی.

#### `beforeSubDelete`

```
export const beforeSubDelete = function (name, record, viewModel) {
    // ...
}
```

این تابع زمانی اجرا می‌شود که کاربر قصد حذف یک رکورد از SubForm را داشته باشد. اگر مقدار `false` یا Promiseای با مقدار `false` برگرداند، رکورد حذف نمی‌شود.

#### `onSubDelete`

```
export const onSubDelete = function (name, record, viewModel) {
    // ...
}
```

-   **`name`**: نام فیلد تودرتو (SubForm).
-   **`record`**: رکوردی که کاربر قصد ارسال آن را دارد.

این تابع زمانی اجرا می‌شود که کاربر رکوردی از SubForm را با موفقیت حذف کند.

#### `onLoadSubForm`

```
export const onLoadSubForm = function (name, record, viewModel) {
    // ...
}
```

-   **`name`**: نام فیلد تودرتو (SubForm).
-   **`record`**: رکوردی که کاربر قصد ارسال آن را دارد.

این تابع زمانی اجرا می‌شود که کاربر قصد افزودن رکوردی را در قالب Modal دارد؛ یعنی با باز شدن Modal افزودن رکورد.

#### `onLoadEditSubForm`

```
export const onLoadEditSubForm = function (name, record, viewModel) {
    // ...
}
```

-   **`name`**: نام فیلد تودرتو (SubForm).
-   **`record`**: رکوردی که کاربر قصد ارسال آن را دارد.

این تابع زمانی اجرا می‌شود که کاربر قصد ویرایش رکوردی را در قالب Modal دارد؛ یعنی با باز شدن Modal ویرایش.

### رویدادهای Tab

#### `onTabLoad`

```
export const onTabLoad = function (tabPanelName, tabPanelItemName, viewModel) {
    if (tabPanelName === 'MyTabPanel') {
        if (tabPanelItemName === 'MyTabPanelItem') {
            // ...
        }
    }
}
```

این Function به‌محض Load شدن یک Tab اجرا می‌شود. اگر فیلدی داخل Tab باشد، تا زمانی که کاربر وارد زبانه مربوطه نشود فیلدهای آن زبانه Load نمی‌شوند. به همین دلیل کدی که برای Instance چنین فیلدی در `onLoad` نوشته شود، پیش از ساخته شدن فیلد می‌تواند با خطا مواجه شود.

> **نکته بسیار مهم:** این تابع برای هر زبانه از هر Tab فقط یک بار اجرا می‌شود.

پارامترها:

-   **`tabPanelName`**: نام فیلد Tab. اگر چند فیلد Tab در فرم وجود داشته باشد، با این پارامتر می‌توانید کدهای هر Tab را تفکیک کنید.
-   **`tabPanelItemName`**: نام زبانه Tab که در فرم‌ساز مشخص شده است.

مثال: با Load شدن Tab Item دوم، روی فیلد داخلی آن Focus شود:

```
export const onTabLoad = function (tabPanelName, tabPanelItemName, viewModel) {
    if (tabPanelName === 'TabPanel') {
        if (tabPanelItemName === 'TabPanelItem2') {
            viewModel.fields.NumberField_Instance.focus();
        }
    }
}
```

![نتیجه اجرای onTabLoad و Focus روی فیلد داخل تب](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D5638AB6-B312-4560-9FD4-0082656F971D&download=true)

نتیجه اجرای onTabLoad و Focus روی فیلد داخل تب

#### `onTabNext`

```
export const onTabNext = function (tabPanelName, currentTabPanelItemName, targetTabPanelItemName, viewModel) {
    if (tabPanelName === 'MyTabPanel') {
        if (targetTabPanelItemName === 'MyTabPanelItem') {
            // ...
        }
    }
}
```

این تابع زمانی اجرا می‌شود که کاربر در فیلد Tab به زبانه بعدی برود.

-   **`tabPanelName`**: نام فیلد Tab.
-   **`currentTabPanelItemName`**: نام زبانه‌ای که کاربر هم‌اکنون در آن است.
-   **`targetTabPanelItemName`**: نام زبانه‌ای که کاربر قصد رفتن به آن را دارد.

اگر مقدار `false` یا Promiseای با مقدار `false` برگردانده شود، کاربر به Tab بعدی نمی‌رود.

مثال: کاربر باید قبل از رفتن به Tab بعدی Checkbox پذیرش قوانین را تأیید کند:

```
export const onTabNext = function (tabPanelName, currentTabPanelItemName, targetTabPanelItemName, viewModel) {
    if (tabPanelName === 'TabPanel') {
        if (currentTabPanelItemName === 'TabPanelItem1') {
            
            if(viewModel.data().AcceptRules() !== true) {
                toast.show('error', 'پذیرش قوانین سامانه اجباری است');
                return false; // جلوگیری از رفتن به تب بعدی
            }
            
        }
    }
}
```

![نتیجه جلوگیری از رفتن به تب بعدی تا زمان پذیرش قوانین](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=7C09E5A9-0248-462E-B189-344751A070F6&download=true)

نتیجه جلوگیری از رفتن به تب بعدی تا زمان پذیرش قوانین

#### `onTabPrevious`

```
export const onTabPrevious = function (tabPanelName, currentTabPanelItemName, targetTabPanelItemName, viewModel) {
    if (tabPanelName === 'MyTabPanel') {
        if (targetTabPanelItemName === 'MyTabPanelItem') {
            // ...
        }
    }
}
```

این تابع زمانی اجرا می‌شود که کاربر در فیلد Tab به زبانه قبلی برود.

-   **`tabPanelName`**: نام فیلد Tab.
-   **`currentTabPanelItemName`**: نام زبانه‌ای که کاربر هم‌اکنون در آن است.
-   **`targetTabPanelItemName`**: نام زبانه‌ای که کاربر قصد رفتن به آن را دارد.

اگر مقدار `false` یا Promiseای با مقدار `false` برگردانده شود، کاربر به Tab قبلی نمی‌رود.

### رویداد فیلد دکمه: `onButtonFieldClick`

```
export const onButtonFieldClick = function (buttonName, viewModel, record) {
    if (buttonName === 'MyButtonName') {
        return true;
    }
}
```

این تابع زمانی اجرا می‌شود که کاربر روی فیلد دکمه کلیک کند. اگر فیلد دکمه عملیات زیرساختی داشته باشد، مانند باز کردن فرم، ابتدا این Function اجرا می‌شود و در صورتی که مقدار `false` یا Promiseای با مقدار `false` برنگرداند، عملیات زیرساختی دکمه اجرا می‌شود.

مثال: پیش از اجرای Provider ساخت کاربر، بررسی شود که نام کاربری از قبل وجود نداشته باشد:

```
export const onButtonFieldClick = function (buttonName, viewModel, record) {
    if (buttonName !== 'Insert') {
        return true;
    }

    return absoluteMethodHelper
        .runProvider(
            101014,
            [
                {
                    name: 'username',
                    value: viewModel.data().UserName()
                }
            ]
        )
        .then(result => {
            const rows = JSON.parse(
                result.Content.providerResult || '[]'
            );

            if (rows[0]?.IsExists) {
                toast.show('error', 'نام کاربری قبلاً وجود دارد');
                return false;
            }

            return true;
        })
        .catch(error => {
            console.error(error);
            return false;
        });
}
```

![نتیجه کنترل نام کاربری پیش از اجرای عملیات دکمه](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8C34EDA8-3AF8-4E7B-938D-B7B95CC85A91&download=true)

نتیجه کنترل نام کاربری پیش از اجرای عملیات دکمه

پارامترها:

-   **`buttonName`**: نام فیلد دکمه‌ای که کاربر روی آن کلیک کرده است.
-   **`record`**: اگر فیلد دکمه داخل SubForm باشد، با این پارامتر می‌توانید به رکورد مربوطه دسترسی داشته باشید.

### Hookهای اکشن لیست رکوردها

پیش از بررسی Hookهای اکشن، محل استفاده آن را در تجربه کاربر نهایی در نظر بگیرید. زمانی که یک فرم برای کاربر نهایی به‌صورت **رونوشت** در دسترس قرار می‌گیرد، کاربر آن فرم را از منوی **لیست فرم‌ها (فرم‌های من)** مشاهده می‌کند.

![منوی لیست فرم‌ها و فرم‌های در دسترس کاربر نهایی](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=43323DED-E7AB-4928-A5DA-BEB4F4F2170C)

فرم‌های رونوشت‌شده در بخش «لیست فرم‌ها» در دسترس کاربر نهایی قرار می‌گیرند.

اگر فرم دارای جدول باشد، با باز کردن فرم، لیست داده‌های ثبت‌شده آن نمایش داده می‌شود.

![نمای لیست داده‌های یک فرم دارای جدول در GraphBPMS](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=876227C1-76D8-4636-8430-4EB9DD2FBEC9)

لیست داده‌های ثبت‌شده برای یک فرم دارای جدول.

در حالت پیش‌فرض، اکشن‌های لیست رکوردها شامل **ویرایش** و **حذف** هستند. با Hookهای این بخش می‌توانید برای هر رکورد اکشن‌های جدید بسازید و اکشن‌های موجود مانند ویرایش یا حذف را نیز تغییر دهید.

#### `onBuildActions`

```
export const onBuildActions = function (container, options) {
    const gridInstance = options.component;
    const record = options.data;
    const actionEdit = container.find('.gi-edit');
    const actionDelete = container.find('.gi-delete');

    devHelper.appendGridButton(
        container,
        'عنوان دکمه',
        'fa fa-puzzle-piece',
        '#E84033',
        null,
        null,
        null,
        function () {
            // عملیات اکشن سفارشی
        },
        false
    );
}
```

این Hook هنگام ساخت Actionهای لیست رکوردها فراخوانی می‌شود. `options.component` به Grid جاری و `options.data` به داده رکوردی که Action برای آن ساخته می‌شود دسترسی می‌دهد. در Sample Studio، همچنین می‌توان به Actionهای زیرساختی ویرایش و حذف از طریق `.gi-edit` و `.gi-delete` دسترسی داشت.

#### `onBeforeDelete`

```
export const onBeforeDelete = function (container, options) {
    const record = options.data;

    return true;
}
```

این Hook زمانی فراخوانی می‌شود که Action حذف در لیست داده‌های فرم اجرا شده باشد. رکورد جاری از مسیر `options.data` در دسترس است و مقدار بازگشتی برای کنترل ادامه عملیات حذف استفاده می‌شود.

### نمونه پایه فایل Custom JS در استدیو فرم ساز

فایل پایه Studio مجموعه Hookهای اصلی را از ابتدا در اختیار توسعه‌دهنده قرار می‌دهد. برای حفظ جریان آموزشی، در اینجا فقط اسکلت حداقلی نمایش داده می‌شود و نسخه کامل Sample در پیوست انتهای مقاله و در فایل `samples/custom-js-studio-sample.js` قرار دارد.

```
export let redirectUrl = '';

export const onLoad = function (viewModel) {
}

export const onReady = function (viewModel) {
}

export const beforeSubmit = function (viewModel) {
    return true;
}

export const onSubmitSuccess = function (viewModel) {
    console.log('Submiting done from custom js');
}

export const onSubmitFailed = function (viewModel, error) {
    console.error('Submiting failed from custom js', error);
}
```

[مشاهده Sample کامل در پیوست](#studio-custom-js-sample)

## معماری viewModel و مسیرهای دسترسی

در Custom JS، `viewModel` نقطه ورود اصلی به فرم است. پیش از رفتن سراغ Instance یا DOM، مشخص کنید به **داده**، **وضعیت فیلد**، **رویداد GraphBPMS** یا **UI Component** نیاز دارید.

| نیاز | مسیر پیشنهادی | نمونه |
| --- | --- | --- |
| خواندن/نوشتن داده فرم | `viewModel.data()` | `viewModel.data().UserName()` |
| Config و وضعیت فیلد | `viewModel.fields` | `viewModel.fields.UserNameConfig` |
| API خود Component | `..._Instance` | `viewModel.fields.UserName_Instance.option(...)` |
| اجرای Button Event | `viewModel.events` | `viewModel.events.MyButton()` |
| داده Modal یک SubForm | `SubFormName_Record()` | `viewModel.MySubForm_Record()` |
| پارامتر ورودی فرم | `getParam(...)` | `viewModel.getParam('RecordId')` |

برای استفاده مطمئن‌تر از Custom JS، شناخت `viewModel` مهم است. فرم‌ساز در هسته خود از Knockout.js استفاده می‌کند و مفهوم `viewModel` در اینجا به مفهوم View Model در Knockout.js اشاره دارد.

View Model در Knockout.js یک پل ارتباطی بین بخش نمایش (**View**) و داده‌های اصلی (**Model**) است. به زبان ساده، نسخه‌ای قابل مشاهده و قابل تغییر از داده‌هاست که به عناصر HTML متصل می‌شود.

### View Model چگونه کار می‌کند؟

-   **ایجاد View Model:** یک Object جاوااسکریپت شامل Propertyها و Methodهایی که داده‌ها را نمایش می‌دهند.
-   **اتصال به HTML:** از طریق ویژگی‌های `data-bind` در عناصر HTML، Propertyها و Methodها به عناصر HTML مرتبط می‌شوند.
-   **به‌روزرسانی خودکار:** تغییرات View Model به‌صورت خودکار در عناصر HTML مرتبط منعکس می‌شود. این ویژگی **Two-way data binding** است.

در Custom JS نیازی نیست View Model را خودتان بسازید یا آن را به HTML Bind کنید؛ این اتصال از قبل انجام شده است. Functionهای Custom JS پارامتری به نام `viewModel` دریافت می‌کنند و این همان View Modelای است که روی HTML فرم Bind شده است.

در هسته فرم‌ساز، `viewModel` یک Instance ساخته‌شده از کلاس `DynamicFormVM` است که روی HTML فرم Bind شده است.

### تابع عمومی `getViewModel`

تابع `getViewModel`، View Model آخرین فرم نمایش‌داده‌شده در صفحه را برمی‌گرداند.

> **هشدار:** از `getViewModel` داخل Custom JS استفاده نکنید. هدف این تابع فقط تست فرم در Console مرورگر است. ممکن است استفاده از آن در شرایط اولیه مشکلی نشان ندهد، اما اگر فرم در شرایط خاصی باز شود با مشکل مواجه شود.

### Property `data`

`data` یکی از مهم‌ترین Propertyهای `viewModel` است. این Property یک Observable شامل Object همه داده‌های فرم است. داده همه فیلدها، از فیلد متنی تا SubForm، داخل این Object قرار دارند و مقادیری که در این Property وجود دارند در نهایت با ذخیره فرم در پایگاه داده ثبت می‌شوند.

ساختار این Object به این صورت است که Key، نام فیلد و Value یک Observable است که داده فیلد را نگه می‌دارد.

برای مثال، اگر یک فیلد متنی با نام `UserFullName` داشته باشید:

```
viewModel.data().UserFullName()
```

#### Observable چیست؟

Observable در Knockout.js به داده قابلیت اطلاع‌رسانی تغییرات می‌دهد. وقتی یک داده Observable باشد، تغییر آن به بخش‌های مرتبط برنامه اطلاع داده می‌شود؛ همان ویژگی Two-way data binding.

برای تغییر مقدار فیلد:

```
viewModel.data().UserFullName('Mahdi Hamedi')
```

اگر به Observable مقدار پاس دهید، مقدار آن تغییر می‌کند و هم‌زمان در فیلد متنی نمایش داده می‌شود. اگر هیچ مقداری پاس ندهید، مقدار Observable برگردانده می‌شود. همچنین وقتی کاربر مقدار فیلد را تغییر دهد، Observable مرتبط نیز تغییر می‌کند و مقدار جدید قابل دریافت است.

این الگو فقط مخصوص فیلد متنی نیست و برای فیلدهای دیگر مانند SubForm و فیلد عددی نیز به همین صورت استفاده می‌شود.

![کار با data فرم از طریق getViewModel در Console](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=2AD1D232-D88F-44F4-AE8F-171E122D5563&download=true)

کار با data فرم از طریق getViewModel در Console

### Property `fields`

برای هر فیلد فرم، مقادیری برای دسترسی به تنظیمات و Instance آن ساخته می‌شود که از طریق `fields` قابل دسترسی هستند:

-   **`Config`**: پیکربندی فیلد. فرم‌ساز علاوه بر Knockout.js برای کتابخانه‌های UI از DevExtreme استفاده می‌کند و این مقدار Config فیلد را نگه می‌دارد.
-   **`_Instance`**: فیلدهایی که با DevExtreme ساخته می‌شوند یک Object از کلاس Component مربوطه دارند. با `_Instance` می‌توانید مستقیماً به Component دسترسی داشته باشید و مطابق مستندات DevExtreme آن را تغییر دهید.
-   **`_Visibility`**: یک Observable با مقدار `true` یا `false` برای کنترل دستی نمایش یا عدم نمایش فیلد. اگر این مقدار را در Custom JS به‌صورت دستی کنترل کنید، قواعد نمایشی فرم‌ساز روی فیلد اثر نخواهند گذاشت.
-   **`_ReadOnly`**: یک Observable با مقدار `true` یا `false` برای کنترل دستی فقط‌خواندنی یا قابل‌ویرایش بودن فیلد. اگر این مقدار را در Custom JS به‌صورت دستی کنترل کنید، قواعد نمایشی فرم‌ساز روی فیلد اثر نخواهند گذاشت.

برای دسترسی، نام فیلد در ابتدای این نام‌ها قرار می‌گیرد.

![نمونه مقادیر ساخته‌شده برای فیلد NumberField](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=FDDD1FFC-41EF-43A9-B576-682DFE755CBB&download=true)

نمونه مقادیر ساخته‌شده برای فیلد NumberField

مثال پنهان کردن `NumberField`:

```
viewModel.fields.NumberField_Visibility(false);
```

اگر فیلد داخل SubForm باشد:

```
viewModel.fields.MySubForm.MyNumberField_Visibility(false);
```

![نمونه دسترسی به فیلد داخل SubForm](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C475597A-8F12-448A-96C4-D2B9D57D7750&download=true)

نمونه دسترسی به فیلد داخل SubForm

### Property `events`

هر فیلد دکمه داخل فرم یک رویداد تلقی می‌شود. برای فراخوانی دستی رویداد بدون کلیک کاربر می‌توانید از `events` استفاده کنید.

اگر فیلد دکمه‌ای با نام `MyButtonField` دارید:

```
viewModel.events.MyButtonField();
```

![اجرای دستی رویداد فیلد دکمه](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AB388604-452F-4BE8-B8DB-D6211EC88A11&download=true)

اجرای دستی رویداد فیلد دکمه

اگر دکمه داخل SubForm باشد، باید رکورد SubForm را نیز پاس دهید. برای مثال:

```
viewModel.events.MySubform.MyButtonField({
    record: {
        data: viewModel.data().MySubFormData()[7]
    }
});
```

در مثال بالا دکمه با رکورد شماره 7 SubForm اجرا می‌شود.

اگر بخواهید داده‌های Modal SubForm را به دکمه پاس دهید:

```
viewModel.events.MySubform.MyButtonField({
      record: {
      data: viewModel.MySubFormData_Record()
      }
});
```

`MySubFormData_Record` داده‌های Modal SubForm است که کاربر هنگام کار با Modal با آن‌ها کار می‌کند.

### Property با الگوی `SubFormName_Record()`

این Property داده‌هایی را نگه می‌دارد که کاربر هنگام باز کردن Modal SubForm با آن‌ها کار می‌کند؛ چه برای افزودن رکورد و چه برای ویرایش رکورد.

هنگام درج، فرم‌ساز رکورد خامی بر اساس فیلدهای SubForm می‌سازد و در این Property قرار می‌دهد. هنگام ثبت، داده‌های آن به `viewModel.data()` منتقل می‌شوند. در حالت ویرایش نیز رکورد موردنظر در همین Property قرار می‌گیرد و پس از ثبت به `viewModel.data()` منتقل می‌شود.

اگر SubFormای به نام `MySubForm` و فیلدی به نام `MyTextBox` داشته باشید:

```
viewModel.MySubForm_Record().MyTextBox();// دریافت مقدار
```

```
viewModel.MySubForm_Record().MyTextBox('Mahdi Hamedi');// تنظیم مقدار
```

### متد `getFormPosition()`

نوع ماژولی که فرم در آن باز شده است را برمی‌گرداند:

-   `anonymous`: فرم در حالت ناشناس باز شده است.
-   `process`: فرم در حالت فرآیند باز شده است.
-   `task`: فرم در حالت وظیفه موبایلی باز شده است.
-   `independent`: فرم در حالت معمولی باز شده است.

### Property `formElement`

این Property به Element فرم اشاره دارد که `viewModel` روی آن Bind شده است:

```
viewModel.formElement
```

### متد `getFormState()`

حالت فرم را برمی‌گرداند:

-   `new`: فرم در حالت جدید باز شده است.
-   `edit`: فرم در حالت ویرایش باز شده است.
-   `view`: فرم در حالت مشاهده، فقط‌خواندنی، باز شده است.

### Property `isAnonymous`

اگر فرم در حالت Anonymous Load شده باشد `true` و در غیر این صورت `false` است:

```
viewModel.isAnonymous
```

### Property `formIsReadonly`

اگر فرم در حالت فقط‌خواندنی باز شده باشد `true` و در غیر این صورت `false` است:

```
viewModel.formIsReadonly
```

### متد `getParam(ParamName)`

با ارسال نام پارامتر، مقدار پارامتر ارسال‌شده به فرم را دریافت می‌کنید:

```
viewModel.getParam('ParamName')
```

نحوه ارسال پارامتر به فرم در بخش `absoluteMethodHelper` آمده است.

### متد `resetForm()`

برای پاک کردن داده‌های فرم:

```
viewModel.resetForm()
```

## رویدادهای Component از مسیر `_Instance`

وقتی فیلد های فرم ساز با یک Component خارجی مانند DevExtreme پیاده‌سازی شده باشد، `_Instance` مسیر دسترسی به Optionها، Methodها و Eventهای همان Component است. برای Eventها دو الگوی رایج دارید: تنظیم Handler از طریق `option('onEventName', handler)` و Subscribe کردن با `on('eventName', handler)`.

**قاعده مهم:** این Eventها را به همه فیلدها تعمیم ندهید. ابتدا نوع فیلد و Component واقعی آن را مشخص کنید. Date/Time و بعضی فیلدهای اختصاصی فرم ساز GraphBPMS ساختار فراخوانی متفاوت دارند.

### دو روش ثبت Event Handler

```
const instance = viewModel.fields.MyTextField_Instance;

// روش 1: تنظیم Option مربوط به Event
instance.option('onValueChanged', e => {
    console.log('New value:', e.value);
});

// روش 2: Subscribe به Event
instance.on('focusIn', e => {
    console.log('Focused', e.element);
});
```

در DevExtreme، نام Option معمولاً با `on` شروع می‌شود؛ مانند `onValueChanged`. در متد `on(...)` نام Event بدون پیشوند `on` استفاده می‌شود؛ مانند `valueChanged`.

### رویدادهای پراستفاده در Editorها

| Event / Option | کاربرد معمول | داده مهم |
| --- | --- | --- |
| `onValueChanged` / `valueChanged` | بعد از تغییر مقدار Component | `e.value` و در بسیاری از Editorها `e.previousValue` |
| `onFocusIn` / `focusIn` | وقتی Component فوکوس می‌گیرد | `e.component`، `e.element`، `e.event` |
| `onFocusOut` / `focusOut` | وقتی Component فوکوس را از دست می‌دهد | `e.component`، `e.element`، `e.event` |
| `onInput` / `input` | هنگام تغییر ورودی در حالی که Editor در Focus است | `e.event` |
| `onKeyUp` / `keyUp` | بعد از رها شدن کلید | `e.event` |
| `onPaste` / `paste` | هنگام Paste در Editorهای پشتیبانی‌شده | `e.event` |
| `onCopy` / `copy` | هنگام Copy در Editorهای پشتیبانی‌شده | `e.event` |
| `onCut` / `cut` | هنگام Cut در Editorهای پشتیبانی‌شده | `e.event` |
| `onOptionChanged` / `optionChanged` | بعد از تغییر یکی از Optionهای Component | `e.name`، `e.fullName`، `e.value` |
| `onChange` / `change` | در TextBox وقتی مقدار با Keyboard تغییر کرده و کنترل Focus را از دست می‌دهد | `e.event` |
| `onEnterKey` / `enterKey` | فشردن Enter در Editorهای پشتیبانی‌شده | `e.event` |
| `onUploadStarted` / `uploadStarted` | اختصاصی FileUploader؛ شروع Upload | `e.file`، `e.request` |

### مثال: واکنش به تغییر مقدار فیلد متنی

```
export const onLoad = function (viewModel) {
    const instance = viewModel.fields.CustomerName_Instance;

    instance.on('valueChanged', e => {
        console.log('CustomerName:', e.value);
    });
}
```

### مثال: تشخیص ورودی Keyboard بدون اتکا به `e.value`

```
export const onLoad = function (viewModel) {
    const instance = viewModel.fields.SearchText_Instance;

    instance.option('onInput', e => {
        console.log('Native event:', e.event);
        console.log('Current value:', instance.option('value'));
    });
}
```

### مثال: FileUploader و فایل انتخاب‌شده

```
export const onLoad = function (viewModel) {
    const instance = viewModel.fields.Attachments_Instance;

    instance.option('onValueChanged', e => {
        const files = e.value || [];
        console.log('Selected files:', files.length);
    });

    instance.option('onUploadStarted', e => {
        console.log('Upload started:', e.file?.name);
    });
}
```

> **مرجع رسمی DevExtreme v22.2:** ساختار Eventها به Component وابسته است. برای TextBox، Eventهایی مانند `onValueChanged`، `onInput`، `onFocusIn` و `onKeyUp` مستند شده‌اند؛ FileUploader نیز Eventهای اختصاصی Upload دارد. [dxTextBox](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxTextBox/) · [dxFileUploader](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxFileUploader/)

## الگوهای پایه کار با داده و وضعیت فیلدها

برای کار با یک فیلد در Custom JS سه مسیر اصلی وجود دارد و بهتر است قبل از نوشتن کد مشخص کنید دقیقاً کدام لایه را می‌خواهید تغییر دهید:

| مسیر | برای چه کاری مناسب است؟ | نمونه |
| --- | --- | --- |
| `viewModel.data()` | خواندن و تغییر **داده‌ای که قرار است همراه فرم ذخیره شود** | `viewModel.data().CustomerName()` |
| `viewModel.fields...Config / _Visibility / _ReadOnly` | کنترل State و Bindingهای GraphBPMS مانند نمایش، فقط‌خواندنی و Required | `viewModel.fields.CustomerName_Visibility(false)` |
| `viewModel.fields..._Instance` | کار مستقیم با Component زنده؛ Option، Method و Event | `viewModel.fields.CustomerName_Instance.option('hint', '...')` |

**راهنمای انتخاب:** برای مقدار Business Data معمولاً از `data()` شروع کنید. وقتی نیاز به رفتار UI یا قابلیت اختصاصی Component دارید سراغ `_Instance` بروید. برای Visibility/ReadOnly/Required نیز مسیرهای GraphBPMS را به تغییر مستقیم DOM ترجیح دهید.

### خواندن و تغییر مقدار فیلد

```
// خواندن مقدار
const currentValue = viewModel.data().CustomerName();

// تغییر مقدار
viewModel.data().CustomerName('GraphBPMS');

// دسترسی سطح Component در صورت نیاز
const componentValue =
    viewModel.fields.CustomerName_Instance.option('value');
```

Observableهای Knockout با فراخوانی بدون آرگومان مقدار را برمی‌گردانند و با ارسال آرگومان مقدار جدید را ثبت می‌کنند. در فیلدهایی که Data Binding استاندارد GraphBPMS برقرار است، تغییر `data()` در UI نیز منعکس می‌شود.

### نمایش، فقط‌خواندنی و غیرفعال بودن

```
// کنترل Visibility در GraphBPMS
viewModel.fields.CustomerName_Visibility(false);
viewModel.fields.CustomerName_Visibility(true);

// کنترل ReadOnly در GraphBPMS
viewModel.fields.CustomerName_ReadOnly(true);
viewModel.fields.CustomerName_ReadOnly(false);

// سطح Component — فقط برای Componentهایی که این Option را دارند
viewModel.fields.CustomerName_Instance.option('disabled', true);
viewModel.fields.CustomerName_Instance.option('disabled', false);
```

> **تفاوت مهم:** `readOnly` و `disabled` یک مفهوم واحد نیستند. در Componentهای DevExtreme، `disabled` تعامل کاربر با Component را غیرفعال می‌کند؛ `readOnly` Editor را فقط‌خواندنی می‌کند. برای منطق فرم ابتدا Stateهای GraphBPMS را در نظر بگیرید و فقط در صورت نیاز سطح Component را تغییر دهید.

### Required و Validation

```
// روش پیشنهادی در Config فیلد
viewModel.fields.CustomerNameConfig.isRequired(true);
viewModel.fields.CustomerNameConfig.isRequired(false);
```

نمونه تغییر مستقیم `validationRules` نیز دیده می‌شود. برای Required ساده، استفاده از `isRequired(...)` خواناتر و کم‌ریسک‌تر است؛ تغییر دستی آرایه Ruleها را فقط زمانی انجام دهید که واقعاً Validation سفارشی نیاز دارید.

### Placeholder، Hint، جهت و وضعیت اعتبار Component

```
const instance = viewModel.fields.CustomerName_Instance;

instance.option('placeholder', 'نام مشتری را وارد کنید');
instance.option('hint', 'این متن هنگام Hover نمایش داده می‌شود');
instance.option('rtlEnabled', true);

// این Option وضعیت اعتبار UI Component را تغییر می‌دهد
instance.option('isValid', true);
```

### کمینه، بیشینه و Step در فیلد Number

```
const number = viewModel.fields.Amount_Instance;

number.option('min', 10);
number.option('max', 100);
number.option('step', 5);
```

`min` و `max` محدوده مقدار NumberBox را مشخص می‌کنند و `step` میزان تغییر با Spin Button، کلیدهای بالا/پایین یا Mouse Wheel را تعیین می‌کند.

### Mask در فیلد متنی

```
const mobile = viewModel.fields.Mobile_Instance;

mobile.option('mask', '\\0\\9000000000');
mobile.option('maskInvalidMessage', 'شماره همراه واردشده صحیح نیست');
mobile.option('inputAttr', { style: 'direction:ltr;' });
mobile.option('useMaskedValue', true);
```

در `dxTextBox` نسخه 22.2، `mask` الگوی ورودی را کنترل می‌کند و `useMaskedValue` تعیین می‌کند کاراکترهای Mask نیز در Value قرار بگیرند یا فقط داده خام نگهداری شود.

### دریافت پارامتر ارسال‌شده به فرم

```
const customerId = viewModel.getParam('CustomerId');
```

نام پارامتر باید دقیقاً با نامی که هنگام باز کردن فرم ارسال شده تطبیق داشته باشد.

## کار مستقیم با Instance و Componentهای UI

`_Instance` زمانی به کار می‌آید که نیاز شما از خواندن/نوشتن داده فراتر است و باید Option، Method یا Event خود Component را کنترل کنید. مسیر Instance را از GraphBPMS می‌گیریم؛ سپس فقط API همان Component را از مستند رسمی Library استفاده می‌کنیم.

**\[وابسته به نسخه/استقرار\]** لینک‌های DevExtreme در این مقاله بر مبنای نسخه `22.2` هستند. وجود یک Option در DevExtreme به‌تنهایی به معنی قابل‌استفاده بودن آن در همه فیلدهای GraphBPMS نیست. ابتدا Mapping فیلد به Component را از همین راهنما بررسی کنید.

### الگوی پایه Instance

```
const instance = viewModel.fields.MyTextField_Instance;

// خواندن Option
const currentValue = instance.option('value');

// تغییر Option
instance.option('hint', 'راهنمای سفارشی');

// اجرای Method
instance.focus();

// Subscribe به Event
instance.on('valueChanged', e => {
    console.log(e.value);
});
```

### فیلد متنی — `dxTextBox`

فیلد متنی در این ساختار به `dxTextBox` متصل است. Optionهای مهم برای Custom JS شامل `value`، `placeholder`، `hint`، `disabled`، `readOnly`، `visible`، `mask` و `useMaskedValue` هستند. Eventهایی مانند `onValueChanged`، `onInput`، `onFocusIn` و `onKeyUp` نیز در API نسخه 22.2 وجود دارند.

```
export const onLoad = function (viewModel) {
    const text = viewModel.fields.Mobile_Instance;

    text.option({
        placeholder: '09xxxxxxxxx',
        mask: '\\0\\9000000000',
        useMaskedValue: true
    });

    text.on('valueChanged', e => {
        console.log('Mobile:', e.value);
    });
}
```

[مرجع رسمی dxTextBox v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxTextBox/)

#### کنترل طول، نوع ورودی و Mask

برای ورودی‌هایی مانند شماره همراه، کد یا داده‌های ساختاریافته، بهتر است Optionهای Component را روی همان `_Instance` تنظیم کنید. در `dxTextBox` v22.2، `maxLength` سقف کاراکتر، `mode` نوع ورودی و Keyboard موبایل، و `mask` الگوی ورود را کنترل می‌کنند.

```
export const onLoad = function (viewModel) {
    const mobile = viewModel.fields.Mobile_Instance;

    mobile.option({
        mode: 'tel',
        maxLength: 11,
        mask: '\\0\\9000000000',
        maskInvalidMessage: 'شماره همراه واردشده معتبر نیست',
        useMaskedValue: true,
        showClearButton: true
    });
}
```

**نکته موبایل:** `mode` در DevExtreme روی نوع ورودی HTML و مجموعه کلیدهای Keyboard موبایل اثر می‌گذارد؛ برای نمونه `tel` برای ورودی تلفن مناسب‌تر است.

### فیلد عددی — `dxNumberBox`

در `dxNumberBox` می‌توانید محدوده، Step، Format و رفتار تغییر مقدار را کنترل کنید. `step` مقدار تغییر با Spin Button، کلیدهای بالا/پایین یا Mouse Wheel است.

```
export const onLoad = function (viewModel) {
    const amount = viewModel.fields.Amount_Instance;

    amount.option({
        min: 0,
        max: 1000000000,
        step: 1000,
        format: '#,##0.##',
        showSpinButtons: true
    });

    amount.on('valueChanged', e => {
        console.log('Amount:', e.value);
    });
}
```

[مرجع رسمی dxNumberBox v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxNumberBox/)

#### مثال: مبلغ با جداکننده ارقام و کنترل بازه

```
export const onLoad = function (viewModel) {
    const amount = viewModel.fields.Amount_Instance;

    amount.option({
        min: 0,
        max: 500000000,
        step: 1000,
        format: '#,##0'
    });

    amount.on('valueChanged', e => {
        viewModel.data().Amount(e.value);
    });
}
```

در این الگو، `format` فقط نحوه نمایش عدد را کنترل می‌کند؛ مقدار اصلی فیلد همچنان از مسیر `viewModel.data().Amount()` قابل خواندن است.

### فیلد تاریخ — Component اختصاصی GraphBPMS

فیلد تاریخ DevExtreme DateBox نیست و طبق مستند فعلی GraphBPMS با `gbDatePicker` کار می‌کند. Instance اختصاصی را از Wrapper فیلد دریافت کنید:

```
const instance =
    viewModel.fields.MyDatePicker_Instance
        .element()
        .parent()
        .parent()
        .gbDatePicker('instance');
```

| Option | کاربرد | نمونه |
| --- | --- | --- |
| `disabledDates` | روزهای غیرفعال | `['1379/06/13', '1400/07/01']` |
| `disabled` | غیرفعال بودن کنترل | `true` |
| `readOnly` | فقط‌خواندنی بودن | `true` |
| `showClearButton` | نمایش دکمه پاک‌کردن مقدار | `false` |
| `startFrom` | شروع بازه انتخاب | `'1403/01/01'` |
| `endsWith` | پایان بازه انتخاب | `'1404/01/01'` |
| `placeholder` | متن راهنما | `'تاریخ را انتخاب کنید'` |
| `includeTime` | انتخاب زمان همراه تاریخ | `false` |

Eventهای مستندشده برای این Component عبارت‌اند از `changeDay`، `changeMonth`، `changeYear` و `input`.

### فیلد زمان — Component اختصاصی GraphBPMS

```
const instance =
    viewModel.fields.MyTimePicker_Instance
        .element()
        .parent()
        .parent()
        .gbTimePicker('instance');
```

| Option | کاربرد |
| --- | --- |
| `disabled` | غیرفعال بودن کنترل |
| `readOnly` | فقط‌خواندنی بودن |
| `showClearButton` | نمایش دکمه پاک‌کردن |
| `placeholder` | متن راهنما |
| `showSecound` | نمایش ثانیه؛ نام Option در مستند فعلی با همین املاء آمده است |
| `showMillisecond` | نمایش میلی‌ثانیه |

Event مورد استفاده برای این Component `input` است.

### فیلد بلی/خیر — `dxCheckBox` یا `dxSwitch`

اگر حالت Switch فعال نباشد، Component پایه `dxCheckBox` است؛ با فعال بودن Switch، Component `dxSwitch` استفاده می‌شود.

```
// حالت CheckBox
const accept = viewModel.fields.AcceptRules_Instance;
accept.option('value', true);
accept.option('text', 'قوانین را می‌پذیرم');

accept.on('valueChanged', e => {
    console.log('Accepted:', e.value);
});
```

در `dxCheckBox` گزینه `enableThreeStateBehavior` برای حالت سه‌وضعیتی وجود دارد. در `dxSwitch` نیز `switchedOnText` و `switchedOffText` متن دو وضعیت را کنترل می‌کنند.

[dxCheckBox v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxCheckBox/) · [dxSwitch v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxSwitch/)

#### مثال: CheckBox سه‌وضعیتی

```
const checkbox = viewModel.fields.Approval_Instance;

checkbox.option({
    enableThreeStateBehavior: true,
    text: 'وضعیت تأیید'
});

checkbox.on('valueChanged', e => {
    // true = checked | false = unchecked | null/undefined = indeterminate
    console.log(e.value);
});
```

اگر فیلد در حالت Switch باشد، از Optionهای همان `dxSwitch` استفاده کنید؛ برای مثال `switchedOnText` و `switchedOffText` متن دو وضعیت را تغییر می‌دهند.

**\[وابسته به نسخه/استقرار\]** خود `dxCheckBox` حالت سوم را پشتیبانی می‌کند؛ اما اگر قصد ذخیره `null` در فیلد Bit GraphBPMS را دارید، سازگاری Data Model و Validation همان Build را جداگانه تست کنید.

### فیلد انتخابی — `dxSelectBox`

در `dxSelectBox`، `value` مقدار انتخاب‌شده است. برای Data Objectها، `valueExpr` کلید مقدار و `displayExpr` فیلد نمایشی را مشخص می‌کند. برای داده‌های Remote یا DataSource، متد `getDataSource()` مسیر مناسبی برای Reload است.

```
export const onLoad = function (viewModel) {
    const select = viewModel.fields.City_Instance;

    select.option('searchEnabled', true);

    select.on('valueChanged', e => {
        console.log('Selected city:', e.value);
    });

    // Reload منبع داده فعلی
    // select.getDataSource().reload();
}
```

**غیرفعال‌کردن یک Item:** اگر Component از `items` استفاده می‌کند، Property صحیح آیتم `disabled` است و پس از تغییر، آرایه باید دوباره به `items` انتساب داده شود:

```
const select = viewModel.fields.Status_Instance;
const items = select.option('items');

if (Array.isArray(items)) {
    const nextItems = items.map((item, index) =>
        index === 1 ? { ...item, disabled: true } : item
    );

    select.option('items', nextItems);
}
```

**نکته:** اگر فیلد از `dataSource` استفاده می‌کند، `items` را هم‌زمان تنظیم نکنید. DevExtreme استفاده هم‌زمان از `items` و `dataSource` را توصیه نمی‌کند.

[مرجع رسمی dxSelectBox v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxSelectBox/)

#### مثال: جستجو و نمایش مقدار خوانا

```
const select = viewModel.fields.City_Instance;

select.option({
    searchEnabled: true,
    searchExpr: ['Title', 'Code'],
    showClearButton: true
});

// در DataSourceهای Object-based فقط وقتی ��ام فیلدهای واقعی منبع داده را می‌دانید:
select.option('valueExpr', 'Id');
select.option('displayExpr', 'Title');
```

**نکته:** نام‌های `Id`، `Title` و `Code` در مثال بالا نمونه‌اند و باید با Contract واقعی Data Source همان فیلد جایگزین شوند.

#### مثال: بازکردن SelectBox و Reload کردن DataSource

```
const select = viewModel.fields.City_Instance;

select.open();

const dataSource = select.getDataSource();
if (dataSource) {
    dataSource.reload();
}
```

اگر Component با `items` کار می‌کند، هم‌زمان `dataSource` را ست نکنید. برای تغییر یک آیتم در آرایه `items` نیز آرایه جدید را دوباره به Component بدهید.

### فیلد فایل — `dxFileUploader`

برای FileUploader، API پایه DevExtreme Optionهایی مانند `accept`، `allowedFileExtensions`، `minFileSize`، `maxFileSize` و `multiple` دارد. Eventهای `onValueChanged` و `onUploadStarted` نیز برای فایل انتخاب‌شده و شروع Upload قابل استفاده‌اند.

```
export const onLoad = function (viewModel) {
    const uploader = viewModel.fields.Attachments_Instance;

    uploader.option({
        allowedFileExtensions: ['.pdf', '.jpg', '.png'],
        maxFileSize: 5 * 1024 * 1024,
        multiple: true
    });

    uploader.option('onValueChanged', e => {
        console.log('Files:', e.value);
    });
}
```

اندازه‌های `minFileSize` و `maxFileSize` در DevExtreme بر حسب Byte هستند. رفتار Upload نهایی در GraphBPMS می‌تواند Wrapper و Contract اختصاصی خودش را داشته باشد؛ بنابراین تنظیمات Component پایه را با Contract ذخیره‌سازی فایل در GraphBPMS یکی ندانید.

[مرجع رسمی dxFileUploader v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxFileUploader/)

### فیلد نقشه — Leaflet

GraphBPMS برای نمایش Map از Leaflet استفاده می‌کند و Instance مستندشده از مسیر زیر در دسترس است:

```
const map =
    viewModel.fields.MyMapField_Instance.mapInstance;

map.setZoom(12);
map.setView([35.69871, 51.351471], 12);
```

[مرجع رسمی Leaflet](https://leafletjs.com/reference.html)

### فیلد لیست درختی — `dxTreeList`

برای TreeList، Methodهای کاربردی شامل `columnOption(...)` برای خواندن/تغییر Column، `refresh()` برای Reload و Repaint و `getDataSource().reload()` برای Reload منبع داده هستند.

```
const tree = viewModel.fields.OrganizationTree_Instance;

// مخفی‌کردن یک ستون با dataField یا name مستندشده
tree.columnOption('InternalCode', 'visible', false);

// تازه‌سازی داده
tree.refresh();
```

[مرجع رسمی dxTreeList v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxTreeList/)

### فیلد دکمه — `dxButton` + Hook اختصاصی GraphBPMS

در سطح UI، `dxButton` Optionهایی مانند `text`، `icon`، `disabled`، `visible`، `hint`، `type` و `stylingMode` دارد. برای کنترل اجرای عملیات GraphBPMS دکمه، Hook اختصاصی `onButtonFieldClick` را به تغییر مستقیم `onClick` ترجیح دهید.

```
export const onLoad = function (viewModel) {
    const button = viewModel.fields.SubmitRequest_Instance;

    button.option({
        text: 'ثبت درخواست',
        icon: 'check',
        hint: 'ثبت نهایی درخواست'
    });
}

export const onButtonFieldClick = function (buttonName, viewModel, record) {
    if (buttonName === 'SubmitRequest') {
        return true;
    }
}
```

[مرجع رسمی dxButton v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxButton/)

### فیلد تودرتو — Modal و Grid

SubForm در GraphBPMS دو Surface مهم برای سفارشی‌سازی دارد: Popup و DataGrid.

```
const popup =
    $('#modal_MySubForm').dxPopup('instance');

const grid =
    $('#grid_MySubForm').dxDataGrid('instance');

// Popup
popup.option({
    width: '70vw',
    height: '80vh',
    fullScreen: false
});

// Grid
grid.columnOption('InternalCode', 'visible', false);
grid.refresh();
```

در DevExtreme v22.2، Popup از Optionهایی مثل `width`، `height`، `fullScreen` و Methodهایی مثل `show()`/`toggle()` پشتیبانی می‌کند. DataGrid نیز Methodهایی مانند `addColumn()`، `columnOption()`، `refresh()` و `getDataSource()` دارد.

[dxPopup v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxPopup/) · [dxDataGrid v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxDataGrid/)

#### مثال: کنترل Popup و ستون‌های Grid

```
const popup =
    $('#modal_MySubForm').dxPopup('instance');

const grid =
    $('#grid_MySubForm').dxDataGrid('instance');

popup.option({
    width: '80vw',
    height: '80vh',
    fullScreen: false
});

// استفاده از شناسه ستون به‌جای وابستگی به index، در صورت وجود dataField/name پایدار
grid.columnOption('Amount', 'visible', true);
grid.columnOption('InternalCode', 'visible', false);

// تازه‌سازی نمایش/داده
grid.refresh();
```

برای Grid، `columnOption(...)` معمولاً خواناتر و پایدارتر از دستکاری رشته‌ای مسیرهایی مانند `columns[0].visible` است، به‌ویژه وقتی ترتیب ستون‌ها تغییر می‌کند.

### فیلد Tab — GraphBPMS Lifecycle در کنار `dxTabPanel`

خود Component پایه TabPanel دارای Optionهایی مانند `selectedIndex` و Eventهایی مانند `onSelectionChanged` است؛ اما Lifecycle اختصاصی GraphBPMS برای زبانه‌ها با Hookهایی مثل `onTabLoad`، `onTabNext` و `onTabPrevious` تعریف شده است. این دو لایه را با هم یکی نکنید.

```
export const onTabLoad = function (
    tabPanelName,
    tabPanelItemName,
    viewModel
) {
    if (
        tabPanelName === 'WizardTabs' &&
        tabPanelItemName === 'ContactStep'
    ) {
        viewModel.fields.Email_Instance.focus();
    }
}
```

[مرجع رسمی dxTabPanel v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxTabPanel/)

#### انتخاب زبانه: GraphBPMS Config یا Component

اگر هدف شما کنترل Flow خود GraphBPMS است، APIهای مستندشده Config مانند `next()`، `prev()` و `selectedIndex(...)` را ترجیح دهید. اگر صرفاً به Option خود Component نیاز دارید و Instance ساخته شده است، `dxTabPanel` نیز `selectedIndex` و `onSelectionChanged` دارد.

```
// مسیر GraphBPMS
viewModel.fields.MyTabConfig.selectedIndex(1);

// فقط پس از ساخته‌شدن Instance و در صورت نیاز به API Component
const tabPanel = viewModel.fields.MyTab_Instance;
console.log(tabPanel.option('selectedIndex'));
```

## الگوهای اختصاصی فیلدهای پیچیده در فرم ساز GraphBPMS

### Map: مدیریت داده‌های GeoJSON، تغییر مقدار و تعامل با Leaflet

در این بخش به طور اختصاصی فیلد نقشه را بررسی خواهیم کرد و یک سری مثال برای این فیلد خواهیم زد.

**ساختار داده فیلد نقشه:**

در فیلد نقشه، ساختار داده به صورت JSON رشته شده و از نوع GeoJSON است و همچنین در صورتی که نقشه هیچ مقداری نداشته باشد مقدار آن Null خواهد بود.

در اینجا یک مثال از فیلد نقشه با یک point که موقعیت آن میدان آزادی تهران است را می‌بینیم:

```
{"type":"FeatureCollection","features":[{"type":"Feature","properties":{},"geometry":{"type":"Point","coordinates":[51.337931,35.699747]}}]}
```

دوباره تأکید می‌شود که این داده از نوع رشته است.

بنابراین وقتی شما بخواهید مقدار فیلد نقشه را با استفاده از Custom JS تغییر دهید و هر مقدار دیگری به آن انتصاب دهید کافیست با این ساختار JSON بصورت رشته ای مقدار مورد نظر خود را انتصاب دهید.

جهت آشنایی بیشتر با نوع داده GeoJSON به این صفحه مراجعه کنید:

[](https://en.wikipedia.org/wiki/GeoJSON)[https://en.wikipedia.org/wiki/GeoJSON](https://en.wikipedia.org/wiki/GeoJSON)

* * *

**تشخیص تغییر مقدار نقشه:**

برای بسیاری از فیلدهای مبتنی بر DevExtreme می‌توان تغییر مقدار را از Eventهایی مانند `onValueChanged` تشخیص داد، اما فیلد نقشه از مسیر Observable داده کنترل می‌شود.

برای تشخیص تغییر مقدار فیلد نقشه بدین صورت عمل کنید:

```
viewModel.data().MyMapField.subscribe(newValue => {
      console.log('Map value has changed', newValue);
});
```

البته این روش برای فیلد های دیگر هم جوابگو است، اما پیشنهاد می‌شود برای فیلد های دیگر از همان روش ذکر شده استفاده کنید.

در این مثال، به محض تغییر مقدار فیلد نقشه (اضافه کردن point، rectangle، حذف آنها و...) تابع ما اجرا می‌شود که مقدار جدید فیلد نقشه را در قالب GeoJSON به صورت رشته میگیرد.

* * *

**تشخیص ساخت لایه جدید:**

در مثال بالا به طور کلی هرموقع مقدار فیلد نقشه تغییر کند تابع ما اجرا می‌شود اما شاید در مواقعی نیاز داشته باشیم که ساخت لایه جدید را تشخیص دهیم.

با استفاده از این کد می‌توانید ساخت لایه جدید در نقشه را تشخیص دهید:

```
const oldLayers = [];
viewModel.data().MyMapField.subscribe(newValue => {
    try {
        newValue = JSON.parse(newValue);
    } catch(e) {}
    if(!newValue) return;

    const createdLayer = newValue?.features.find(layer => !oldLayers.find(i => _.isEqual(i, layer)));
    if(createdLayer) {
        oldLayers.push(createdLayer);
        console.log('New layer created', createdLayer);
    }
});
```

در کد بالا یک آرایه به اسم oldLayers تعریف شده است که لایه هایی که کاربر از قبل ساخته را در خود نگه میدارد و طبیعتا این آرایه به طور پیش فرض خالی است.

به محض اینکه مقدار فیلد نقشه تغییر کرد در ابتدا مقدار رشته ای به مقدار JSON تغییر میکند و سپس روی لایه های GeoJSON بررسی می‌شود که هر لایه ای که در آرایه oldLayers وجود نداشت پس قطعا به تازگی ساخته شده، در ابتدا آن لایه را داخل oldLayer میریزیم و سپس عملیات مورد نظر را روی آن انجام میدهیم.

* * *

**نمایش point بر اساس Lat و Lng روی نقشه:**

همانطور که در بالا گفته شد، نوع داده فیلد نقشه بصورت GeoJSON است و هر مقداری را که بصورت GeoJSON به مقدار فیلد نقشه پاس دهیم آن مقدار روی نقشه نمایش داده می‌شود.

بنابراین برای نمایش یک Point با یک مختصات خاص کافیست به این شکل عمل کنیم:

```
const lng = 51.351471;
const lat = 35.69871;

viewModel.data().MyMapField(JSON.stringify({
    type: 'FeatureCollection',
    features: [
        {
            type: 'Feature',
            properties: {},
            geometry: {
                type: 'Point',
                coordinates: [lng , lat]
            }
        }
    ]
}));
```

در این مثال ما فقط یک point روی نقشه ایجاد کردیم، شما می‌توانید به تعداد دلخواه روی نقشه لایه ایجاد کنید.

* * *

**تغییر زوم نقشه:**

برای تغییر زوم نقشه باید Instance مربوط به leaflet را داشته باشیم.

با استفاده از این کد می‌توانید Instance کتابخانه Leaflet را دریافت کنید:

```
const myMapField__leafletInstance = viewModel.fields.MyMapField_Instance.mapInstance;
```

پس از دریافت instance کتابخانه leaflet، با استفاده از این کد میتوانیم زوم نقشه را به مقدار دلخواه تغییر دهیم:

```
myMapField__leafletInstance.setZoom(10);
```

* * *

**تغییر موقعیت نمایشی نقشه:**

بعد از دریافت Instance کتابخانه Leaflet می‌توانید مرکز نمای نقشه را تغییر دهید. متد `setView` علاوه بر مرکز، Zoom را نیز دریافت می‌کند؛ در مثال زیر Zoom فعلی با `getZoom()` حفظ می‌شود:

```
const currentZoom = myMapField__leafletInstance.getZoom();

myMapField__leafletInstance.setView({
    lng: 51.351471,
    lat: 35.69871
}, currentZoom);
```

توجه کنید که متد های Leaflet فقط به همین موارد محدود نمی‌شود، جهت آشنایی بیشتر با این کتابخانه به صفحه زیر مراجعه کنید:

[](https://leafletjs.com/reference.html)[https://leafletjs.com/reference.html](https://leafletjs.com/reference.html)

* * *

### Tab: چرخه عمر فیلدهای داخل زبانه (Lifecycle)

در این بخش به بررسی فیلد تب میپردازیم.

هر فیلد تب شامل چندین زبانه است که هرکدام از این زبانه ها شامل تعدادی فیلد دیگر هستند.

هر زبانه تب قابلیت مخفی شدن، فقط خواندنی شدن و غیر فعال شدن را دارد.

فیلد تب هیچ دخالتی در ساختار داده فرم نمیکند و صرفا جنبه نمایشی دارد، بطور مثال اگر شما فیلد متنی خارج از تب داشته باشد و همان فیلد متنی را داخل تب ببرید هیچ تفاوتی در ساختار داده چه در سطح viewModel چه در سطح پایگاه داده نمیکند.

**سفارشی سازی فیلد های داخل تب:**

به طور پیش فرض، موقع لود فرم تمامی فیلد های فرم لود میشوند، شامل data، \_Config و... . اما در صورتی که فیلد مورد نظر شما داخل تب باشد تا زمانی که کاربر وارد آن تب نشود \_Instance آن فیلد لود نخواهد شد.

به عبارتی دیگر، فیلد هایی که داخل تب هستند و تب آنها لود نشده است تمامی Config و data های آن فیلد داخل viewModel هستند به غیر از \_Instance که زمانی لود می‌شود که کاربر وارد آن تب مربوطه شود.

بنابراین به این نکته دقت کنید که در صورتی که در Custom JS خود می‌خواهید از Instance یک فیلد استفاده کنید حواستان باشد که آن فیلد خارج از تب باشد.

اما اگر فیلد داخل تب بود باید چه کرد؟ در این صورت باید از تابع onTabLoad مربوط به Custom JS استفاده کنید:

```
export const onTabLoad = function (tabPanelName, tabPanelItemName, viewModel) {
    if (tabPanelName === 'MyTabField') {
        if (tabPanelItemName === 'MyTabFieldItem') {
            viewModel.fields.MyTextField_Instance.option('hint', '...');
        }
    }
}
```

در مثال بالا، فرم ما یک فیلد متنی با نام MyTextField دارد که داخل تب است و ما میخواهیم hint آن را تغییر دهیم.

* * *

### Tab: کنترل زبانه‌ها از Config

**نحوه دسترسی به زبانه های تب:**

با استفاده از کد زیر می‌توانید به زبانه های تب به صورت آرایه دسترسی داشته باشید:

```
const myTabItems = viewModel.fields.MyTabFieldConfig.dataSource;
```

* * *

**نحوه بررسی اینکه آیا یک زبانه خاص قابل پیمایش است یا خیر:**

```
viewModel.fields.MyTabConfig.dataSource[0].navigable();
```

دقت کنید که پراپرتی navigable داده نمیگیرد و فقط صرفا اینکه تب قابل پیمایش است یا خبر را برمیگرداند.

* * *

**تنظیم اجباری بودن یا نبودن فیلد های داخل زبانه تب:**

```
viewModel.fields.MyTabConfig.dataSource[0].isRequired(true/*or false*/);
```

دقت کنید که در صورتی که به صورت دستی اجباری بودن یا نبودن زبانه تب را مشخص کنید دیگر قوائد نمایشی و... هیچ تاثیری روی آن نخواهند گذاشت.

* * *

**تنظیم فقط خواندنی بودن فیلد های داخل زبانه تب:**

```
viewModel.fields.MyTabConfig.dataSource[0].readOnly(true /* or false */);
```

دقت کنید که در صورتی که به صورت دستی فقط خواندنی بودن یا نبودن زبانه تب را مشخص کنید دیگر قوائد نمایشی و... هیچ تاثیری روی آن نخواهند گذاشت.

* * *

**تنظیم نمایش یا عدم نمایش بودن زبانه تب:**

```
viewModel.fields.MyTabConfig.dataSource[0].visible(false/*or true*/);
```

دقت کنید که در صورتی که به صورت دستی نمایش یا عدم نمایش زبانه تب را مشخص کنید دیگر قوائد نمایشی و... هیچ تاثیری روی آن نخواهند گذاشت.

* * *

**رفتن به مرحله بعدی:**

```
viewModel.fields.MyTabConfig.next();
```

* * *

**رفتن به مرحله قبلی:**

```
viewModel.fields.MyTabConfig.prev();
```

* * *

**تنظیم ایندکس تب انتخاب شده:**

```
viewModel.fields.MyTabConfig.selectedIndex(3);
```

* * *

**بررسی اینکه آیا رفتن به مرحله قبلی قابل انجام است یا خیر:**

```
viewModel.fields.MyTabConfig.prevIsAvailable();// true or false
```

* * *

**بررسی اینکه رفتن به مرحله بعدی قابل انجام است یا خیر:**

```
viewModel.fields.MyTabConfig.nextIsAvailable();//true or false
```

* * *

### SubForm: داده رکورد، RecordState، Modal و Grid

همانطور که در جریان هستید، SubForm یک فیلد شامل چندین فیلد دیگر به صورت آرایه ای از Object است.

برای مثال، اگر یک SubFormی به نام MySubForm شامل 2 فیلد به نام های MyTextBox و MyNumberBox داشته باشیم ساختار داده SubForm به این صورت است:

```
[
    {
        Id: 1,
        gb_state: 1,
        MyTextField: 'Text field value 1',
        MyNumberField: 456
    },
    {
        Id: 2,
        gb_state: 1,
        MyTextField: 'Text field value 2',
        MyNumberField: 13
    },
    {
        Id: 3,
        gb_state: 1,
        MyTextField: 'Text field value 2',
        MyNumberField: null
    }
]
```

که البته تمامی مقادیر object ها بصورت observable هستند.

در اینجا علاوه بر MyNumberField و MyTextField دو مقدار دیگر هم می‌بینیم: Id و gb\_state.

**فیلد Id چیست؟** وقتی شما یک فرمی بسازید معادل آن فرم در پایگاه داده یک جدول ذخیره خواهد شد. در صورتی که فرم شما شامل یک SubForm باشد معادل آن SubForm هم یک جدول در پایگاه داده ایجاد می‌شود که با استفاده از کلید خارجی با هم مرتبط میشوند.

Id در واقع شناسه رکورد SubForm شماست که بعد از ذخیره فرم در جدول SubForm ذخیره خواهد شد.

**فیلد gb\_state چیست؟** این فیلد در واقع وضعیت این رکورد را مشخص میکند:

1.  **مقدار 1 یا RecordStateType.new:** این مقدار به این معنی است که این رکورد به تازگی ایجاد شده است و به بک اند میگوید که این رکورد بایستی در پایگاه داده ایجاد شود. بنابراین اگر با استفاده از CustomJS رکورد جدیدی بصورت دستی برای SubForm بسازید و این فیلد را به درستی مقدار دهی نکنید موقع ذخیره نهایی فرم رکورد شما در پایگاه داده ایجاد نخواهد شد. **پس به طور کلی هرموقع رکورد جدیدی برای SubForm ساختید حتما باید مقدار gb\_state برابر با 1 باشد.**
    
2.  اگر مقدار یک رکورد را در SubForm به‌صورت دستی تغییر دادید اما مقدار gb\_state که قبلاً برابر با 0 بوده را به مقدار 2 تغییر ندهید، تغییر جدید در پایگاه داده ثبت نخواهد شد.
    
3.  **مقدار 2 یا RecordStateType.edit:** این مقدار فقط در حالت ویرایش بکار میرود و به این معنی است که این رکورد تغییراتی داشته و بک اند باید آن تغییرات را در پایگاه داده لحاظ کند.
    
4.  **مقدار 3 یا RecordStateType.del:** این مقدار فقط در حالت ویرایش به کار میرود و به این معنی است که این رکورد باید از پایگاه داده حذف شود، بنابراین اگر رکوردی در حالت ویرایش داشتید و صرفا فقط از آرایه SubForm آن را پاک کردید بعد از ذخیره فرم این رکورد همچنان در پایگاه داده وجود خواهد داشت.
    

**دسترسی به داده های مدال فیلد تودرتو:**

پاپ آپ مربوط به SubForm چه در حالت ویرایش رکورد چه در حالت درج رکورد جدید در پس زمینه به داده های اصلی (viewModel.data()) متصل نیست، به این دلیل که شاید کاربر اقدام به ویرایش رکوردی کند اما وسط ویرایش داده منصرف شود و اقدام به بستن مدال کند.تا زمانی که کاربر روی دکمه ثبت مدال SubForm کلیک نکرده نباید داده های آن مدال در بخش داده های اصلی فرم ریخته شود. بنابراین داده های این مدال به یک بخش دیگری از viewModel متصل است:

```
viewModel.MySubForm_Record()
```

وقتی شما روی دکمه جدید SubForm کلیک کنید، فرم‌ساز با توجه به فیلد های SubForm یک سری داده خام را میسازد و داخل این پراپرتی میریزد، سپس مدال باز شده و فیلد های آن به این داده ها متصل میشوند و بعد از اینکه کاربر روی دکمه ثبت کلیک شد فرم‌ساز داده هارا از \_Record میخواند و داخل دیتا های اصلی (viewModel.data()) میریزد.

و برای ویرایش هم همینطور، فرم‌ساز در پس زمینه از رکوردی که قصد ویرایش آن را دارید یک کپی میگیرد و داخل این پراپرتی میریزد و ادامه ماجرا.

-   نکته: در حالت ویرایش یا درج رکورد به صورت اینلاین فرم‌ساز به \_Record هیچ کاری ندارد و مستقیما به viewModel.data() وصل می‌شود.

**سفارشی سازی مدال فیلد تودرتو:**

برای تغییر آپشن های مدال (مثلا عرض، ارتفاع، حالت تمام صفحه و...) در ابتدا باید instance مربوط به مدال را داشته باشید.

با استفاده از این کد می‌توانید instance مدال را دریافت کنید:

```
const mySubForm__modalInstance = $('#modal_MySubForm').dxPopup('instance');
```

برای مثال، جهت تغییر عرض مدال می‌توانید از کد زیر استفاده کنید:

```
mySubForm__modalInstance.option('width', '500px');
```

همانطور که قبلا ذکر شد، مدال فیلد تودرتو در واقع مدال کتابخانه devextreme است.

جهت مشاهده لیست متد ها، آپشن ها و رویداد های Popup به این صفحه مراجعه نمایید:

[](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxPopup/)[https://js.devexpress.com/jQuery/Documentation/22\_2/ApiReference/UI\_Components/dxPopup](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxPopup)

> **مرجع فنی — v22.2:** `width` و `height` اندازه Popup را می‌پذیرند و `fullScreen` حالت تمام‌صفحه را کنترل می‌کند.

**سفارشی سازی گرید فیلد تودرتو:**

برای تغییر آپشن های گرید (مثلا افزودن ستون جدید، مخفی کردن یک ستون خاص و...) در ابتدا باید instance مربوط به گرید را داشته باشید.

با استفاده از این کد می‌توانید instance گرید را دریافت کنید:

```
const mySubForm__dataGridInstance = $('#grid_MySubForm').dxDataGrid('instance');
```

برای مثال، جهت مخفی کردن یک ستون خاص باید بدین شکل عمل کنید:

```
mySubForm__dataGridInstance.option('columns[0].visible', false);
```

همانطور که قبلا ذکر شد، گرید فیلد تودرتو در واقع گرید کتابخانه devextreme است.

جهت مشاهده لیست متد ها، آپشن ها و رویداد های DataGrid به این صفحه مراجعه نمایید:

[](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxDataGrid/)[https://js.devexpress.com/jQuery/Documentation/22\_2/ApiReference/UI\_Components/dxDataGrid](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxDataGrid)

> **مرجع فنی — v22.2:** `columns` مجموعه تنظیمات ستون‌های DataGrid است و تنظیم `visible` در سطح ستون برای نمایش یا مخفی‌کردن آن استفاده می‌شود.

* * *

### FileUpload: محدودیت فایل، پیام‌ها و رویدادها

![نمای کلی فیلد FileUpload](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=E947B575-94B4-47B5-9A43-56A714AFE81A.png)

نمای کلی فیلد FileUpload

برای کارهای مربوط به UI فیلد فایل، از `viewModel.fields.FIELD_NAME_Instance` استفاده کنید. در API پای�� `dxFileUploader` می‌توان محدودیت نوع و اندازه فایل، انتخاب چندفایلی، متن‌های رابط و Eventهای انتخاب/شروع Upload را کنترل کرد.

**تفکیک مهم:** این Optionها رفتار Component رابط کاربری را کنترل می‌کنند. ارسال فایل به Provider با `absoluteMethodHelper.runProvider` یک سناریوی Integration جداست که در بخش Provider توضیح داده شده است.

#### خواندن فایل‌های انتخاب‌شده و شمارش آن‌ها

```
const uploader = viewModel.fields.Attachments_Instance;
const files = uploader.option('value') || [];

console.log('File count:', files.length);
console.log('First file:', files[0]);
```

#### محدودکردن نوع، اندازه و تعداد فایل‌ها

```
const uploader = viewModel.fields.Attachments_Instance;

uploader.option({
    accept: '.pdf,.jpg,.png',
    allowedFileExtensions: ['.pdf', '.jpg', '.png'],
    minFileSize: 1024,              // 1 KB
    maxFileSize: 5 * 1024 * 1024,  // 5 MB
    multiple: true
});
```

`minFileSize` و `maxFileSize` در DevExtreme بر حسب **Byte** هستند. `allowedFileExtensions` اعتبار Extension را کنترل می‌کند و `accept` برای فیلتر File Picker مرورگر کاربرد دارد.

#### شخصی‌سازی متن‌های FileUploader

```
const uploader = viewModel.fields.Attachments_Instance;

uploader.option({
    labelText: 'فایل را اینجا رها کنید',
    selectButtonText: 'انتخاب فایل',
    uploadedMessage: 'فایل با موفقیت بارگذاری شد',
    uploadFailedMessage: 'بارگذاری فایل ناموفق بود',
    invalidMinFileSizeMessage: 'حجم فایل کمتر از حد مجاز است',
    invalidMaxFileSizeMessage: 'حجم فایل بیشتر از حد مجاز است'
});
```

#### واکنش به انتخاب فایل و شروع Upload

```
const uploader = viewModel.fields.Attachments_Instance;

uploader.option('onValueChanged', e => {
    const files = e.value || [];
    console.log('Selected files:', files);
});

uploader.option('onUploadStarted', e => {
    console.log('Upload started:', e.file);
});
```

**\[وابسته به نسخه/استقرار\]** `acceptExtensionWithSize` جزو API پایه `dxFileUploader` v22.2 نیست. اگر Build مقصد این Option را به‌صورت اختصاصی پشتیبانی نمی‌کند، از `allowedFileExtensions` و `accept` استفاده کنید.

[مرجع رسمی dxFileUploader v22.2](https://js.devexpress.com/jQuery/Documentation/22_2/ApiReference/UI_Components/dxFileUploader/)

### Container: کنترل ReadOnly و Visibility

**\[نیازمند تأیید فنی\]** این الگو به Helperها و Constantهای داخلی مانند `formHelper`، `FormBuilderStatics` و `GBFieldType` متکی است. پیش از استفاده، وجود همین APIها را در Build مقصد بررسی کنید.

**تعریف فعال کردن و غیر فعال کردن Container :**

```
function makeContainerReadOnly(viewModel, name, parentName, readOnlyStatus) {
    const containerID = formHelper.getFieldObject(viewModel, name, parentName, FormBuilderStatics.Config).Id;    
    const fields = Object.keys(['', null].includes(parentName) ? viewModel.fields : viewModel.fields[parentName])
        .filter(item => item.endsWith(FormBuilderStatics.Config))
        .map(item => item.slice(0, item.length - FormBuilderStatics.Config.length))
        .filter(item => formHelper.getFieldObject(viewModel, item, parentName, FormBuilderStatics.Config).ContainerId === containerID);
    for(const field of fields) {
        const config = formHelper.getFieldObject(viewModel, field, parentName, FormBuilderStatics.Config);
        switch(config.type) {
            case GBFieldType.Container: 
                makeContainerReadOnly(viewModel, field, parentName, readOnlyStatus);
                break;
            case GBFieldType.HTML:
            case GBFieldType.GPS:
                break;
            case GBFieldType.SubForm:
                let subformFields;  
                if(['', null].includes(parentName)) subformFields = viewModel.fields;
                else subformFields = viewModel.fields[parentName];                
                subformFields[field + FormBuilderStatics.ReadOnly](readOnlyStatus);
                break;
            default:
                const instance = formHelper.getFieldObject(viewModel, field, parentName, FormBuilderStatics.Instance);
                if(instance) instance.option('readOnly', readOnlyStatus);
        }
    }
}
// نحوه فراخوانی تابع برای فعال و غیر فعال کردن Container :
makeContainerReadOnly(viewModel, FIELD_NAME_HERE, PARENT_FIELD_NAME_HERE, READONLY_STATUS) 
```

**نمایش و عدم نمایش Container :**

```
viewModel.fields.CONTAINER_FIELD_NAME_Visibility(true); // یا false
// CONTAINER_FIELD_NAME = نام لاتین Container
```

* * *

## absoluteMethodHelper و Integrationهای سمت Client

`absoluteMethodHelper` مجموعه Helperهای سمت Client GraphBPMS برای اجرای عملیات مستندشده‌ای مانند Provider و برخی عملیات فرم/فرآیند/گزارش است. در این بخش، الگوهای جاری و نمونه‌های قدیمی از هم تفکیک می‌شوند تا توسعه‌دهنده از Contract صحیح همان Build استفاده کند.

**الگوی فعلی Provider:** برای Providerهای غیر Paging، API مرجع `absoluteMethodHelper.runProvider(...)` است. برای Paging از `providerDesigner.runProviderPaging(...)` استفاده می‌شود؛ این دو را یکی نکنید.

### ابزارهای عمومی absoluteMethodHelper

**دریافت تاریخ جاری :**

```
absoluteMethodHelper.getPersianDate()
```

* * *

### اجرای Provider با runProvider — الگوی جاری

امضای جاری برای Providerهای غیر Paging به‌صورت زیر است:

```
absoluteMethodHelper.runProvider(providerId, parameters = [])
```

`providerId` شناسه Provider و `parameters` آرایه‌ای از Objectهای `{ name, value }` است. نام Parameter باید دقیقاً با Parameter تعریف‌شده در Provider هماهنگ باشد.

#### Provider بدون پارامتر

```
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

#### Provider با پارامتر و خروجی Structured

```
const parameters = [
    { name: 'UserId', value: 25 },
    { name: 'Status', value: 1 }
];

absoluteMethodHelper
    .runProvider(PROVIDER_ID, parameters)
    .then(result => {
        const rows = JSON.parse(
            result.Content.providerResult || '[]'
        );

        console.log(rows);
    })
    .catch(error => {
        console.error(error);
    });
```

**رفتار خطا در Helper جاری:** در Helper جاری، پاسخ GraphBPMS با `HasError === true` باعث Reject شدن Promise می‌شود؛ بنابراین `.catch(...)` مسیر مرجع مدیریت خطاست. الگوی قدیمی بررسی `HasError` داخل `.then(...)` ممکن است در Legacy دیده شود و باید با Build مقصد تطبیق داده شود.

#### مثال GraphBPMS: دریافت داده و قرار دادن نتیجه در viewModel

```
export const onLoad = function (viewModel) {
    const userId = viewModel.data().UserId();

    absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [{ name: 'UserId', value: userId }]
        )
        .then(result => {
            const rows = JSON.parse(
                result.Content.providerResult || '[]'
            );

            const user = rows[0];
            if (user) {
                viewModel.data().UserFullName(user.FullName ?? '');
            }
        })
        .catch(error => {
            console.error(error);
            toast.show('error', 'دریافت اطلاعات کاربر ناموفق بود');
        });
}
```

در این مثال نام‌های `UserId`، `UserFullName` و `FullName` نمونه‌اند و باید با Field/Output واقعی فرم و Provider جایگزین شوند.

#### ارسال File به Provider

Helper جاری اگر در Parameterها یک مقدار از نوع `File` تشخیص دهد، Request را خودکار به `FormData` تبدیل می‌کند؛ بنابراین در Custom JS لازم نیست FormData را دستی بسازید.

```
global
    .getFileFromUser({
        accept: '*'
    })
    .then(file => {
        return absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [
                { name: 'file', value: file },
                { name: 'category', value: 'document' }
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

#### تبدیل Blob به File پیش از ارسال

File Detection در Helper جاری با `instanceof File` انجام می‌شود. اگر خروجی شما Blob است، آن را ابتدا به File تبدیل کنید:

```
const file = new File(
    [myBlob],
    'document.bin'
);

absoluteMethodHelper.runProvider(
    PROVIDER_ID,
    [{ name: 'file', value: file }]
);
```

#### دریافت File از Provider

اگر Provider خروجی Attachment با Header مناسب برگرداند، Helper فایل را در `result.Content` قرار می‌دهد.

```
absoluteMethodHelper
    .runProvider(
        PROVIDER_ID,
        [{ name: 'documentId', value: 25 }]
    )
    .then(result => {
        const file = result.Content;

        saveAs(file, file.name);
    })
    .catch(error => {
        console.error(error);
    });
```

**نکته:** `saveAs` در Sample Code فعلی محصول استفاده می‌شود، اما عضو `absoluteMethodHelper` نیست. برای File Output، Endpoint مقصد باید Contract مناسب Attachment از جمله `Content-Disposition` را برگرداند.

#### File Input + File Output در یک فراخوانی

```
global
    .getFileFromUser({
        accept: '*'
    })
    .then(inputFile => {
        return absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [
                { name: 'file', value: inputFile },
                { name: 'targetFormat', value: 'pdf' }
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

#### Providerهای Paging

برای Providerهای Paging از `providerDesigner.runProviderPaging(...)` استفاده می‌شود. این مسیر از `absoluteMethodHelper.runProvider(...)` جداست و برای Paging نباید این دو API با هم جایگزین شوند.

```
providerDesigner
    .runProviderPaging(
        PROVIDER_ID,
        parameters,
        0,      // pageIndex
        10,     // pageSize
        '',     // pagingMainFilter
        [
            { name: 'UserName', value: '' }
        ],
        [
            { name: 'UserName', value: 'asc' }
        ]
    )
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.warn(error);
    });
```

**از Transport داخلی عبور نکنید:** برای Provider غیر Paging، فراخوانی مستقیم Endpoint داخلی با `fetch` یا `$.ajax` را جایگزین `absoluteMethodHelper.runProvider` نکنید، مگر برای Build مقصد API رسمی دیگری مستند شده باشد. Helper جاری جزئیاتی مانند Anti-forgery، JSON/FormData، Attachment و Reject کردن خطا را مدیریت می‌کند.

#### استفاده با `async/await`

از آنجا که `runProvider(...)` یک Promise برمی‌گرداند، می‌توان همان Contract را با `async/await` نیز مصرف کرد. مدیریت خطا همچنان باید با `try/catch` انجام شود.

```
export const onButtonFieldClick = async function (buttonName, viewModel, record) {
    if (buttonName !== 'LoadCustomer') {
        return true;
    }

    try {
        const result = await absoluteMethodHelper.runProvider(
            PROVIDER_ID,
            [{ name: 'UserId', value: viewModel.data().UserId() }]
        );

        const rows = JSON.parse(
            result.Content.providerResult || '[]'
        );

        const user = rows[0];
        if (user) {
            viewModel.data().UserFullName(user.FullName ?? '');
        }

        return true;
    } catch (error) {
        console.error(error);
        toast.show('error', 'اجرای Provider ناموفق بود');
        return false;
    }
}
```

**روش پیشنهادی:** در Hookهایی که GraphBPMS بازگشت Promise را کنترل می‌کند، `async` می‌تواند کد را خواناتر کند. در هر دو سبک `.then/.catch` و `async/await` همان Contract Provider را حفظ کنید.

### کار با فرآیندها در absoluteMethodHelper

#### اطلاعات Process جاری با `getProcessInfo()`

`absoluteMethodHelper.getProcessInfo()` اطلاعات Context فرایندی فرم جاری را برمی‌گرداند. Propertyهای اصلی عبارت‌اند از:

| Property | کاربرد |
| --- | --- |
| `ProcessCreationID` | شناسه ایجاد/نمونه فرایند |
| `ProcessID` | شناسه فرایند |
| `ReferencesID` | شناسه مرجع فرایند |
| `TaskID` | شناسه Service Task یا User Task |
| `ElementConfigID` | شناسه ElementConfig مربوط به User Task |
| `EntityConfigID` | شناسه رکورد ثبت‌شده در ProcessEntityConfig |
| `EntityID` | شناسه فرم |
| `EntityMode` | حالت نمایش فرم: جدید، ویرایش یا مشاهده |
| `EntityTypeID` | نوع Element فرایند |
| `FormsRecordID` | شناسه رکورد فرم |

```
const processInfo = absoluteMethodHelper.getProcessInfo();

const processCreationId = processInfo.data.ProcessCreationID;
const taskId = processInfo.data.TaskID;
const formRecordId = processInfo.data.FormsRecordID;
```

#### شروع فرایند — روش جاری

برای شروع فرایند، متد `runProcessWithVerbosityLevel(...)` است. پارامترهای اصلی این متد:

| پارامتر | کاربرد |
| --- | --- |
| `processGroupID` | شناسه گروه فرایند |
| `variables` | متغیرهای ارسالی به فرایند به‌صورت آرایه Objectهای Name/Value |
| `customTitle` | عنوان نمونه فرایند در کارتابل |
| `runType` | نوع Element شروع فرایند |
| `anonymous` | شروع فرایند بدون احراز هویت |
| `formId` | شناسه فرم در صورت نیاز |
| `verboseLevel` | سطح جزئیات خروجی از 0 تا 6 |

#### سطح جزئیات خروجی با `verboseLevel`

| مقدار | نام | خلاصه خروجی |
| --- | --- | --- |
| `6` | Full | خروجی کامل؛ شامل جزئیات Process/Node و متغیرهای انتهایی. مقدار پیش‌فرض در صورت ارسال null. |
| `5` | Detailed | جزئیات نسبتاً کامل بدون متغیرهای انتهایی. |
| `4` | Basic | جزئیات محدود، از جمله ProcessCreationID و وضعیت کلی اجرا. |
| `3` | Minimum | حداقل جزئیات وضعیت اجرا؛ Content در نمونه null است. |
| `2` | PlainLite | وضعیت اجرا به‌همراه متغیرهای انتهایی به‌صورت آرایه Name/Value. |
| `1` | Lite | مانند PlainLite، با تبدیل مقدارهای رشته‌ای JSON به ساختار اصلی آرایه/Object در صورت امکان. |
| `0` | BoostLite | مانند Lite، اما خروجی متغیرها به‌صورت Dictionary از Name به Value. |

همین جدول برای متد شروع و ادامه فرایندِ دارای `verboseLevel` استفاده می‌شود؛ بنابراین تعریف سطوح در ادامه تکرار نمی‌شود.

**نمونه خروجی سطح 6 — Full**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": {
            "ProcessCreationID": 53341,
            "TaskElementResult": null,
            "EventElementResult": {
                "ProcessFlowModel": {
                    "ProcessEnd": true,
                    "Message": "فرآیند به پایان رسید",
                    "EventType": 15,
                    "Variables": [
                        {
                            "Name": "Users",
                            "Value": "1"
                        }
                    ],
                    "ProcessID": 5617,
                    "TaskID": 13048
                },
                "ElementBaseType": 3,
                "ElementType": 15,
                "IsUserTask": false,
                "ReferenceID": 182678,
                "RecordID": 0,
                "IsEndEvent": true,
                "IsTimerIntermediateEvent": false,
                "IsSignalIntermediateCatchEvent": false,
                "InSameLane": false,
                "IsGateway": false,
                "CustomReferenceExplain": "",
                "CustomReferenceExplainType": null,
                "IsReceiveTask": false,
                "IsGraphMobileTask": false,
                "ViewState": null,
                "IsCallActivityTask": false,
                "AssignedTo": null
            }
        }
    }
}
```

**نمونه خروجی سطح 5 — Detailed**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": {
            "ProcessCreationID": 53342,
            "EventElementResult": {
                "ElementBaseType": 3,
                "ElementType": 15,
                "IsUserTask": false,
                "ReferenceID": 182681,
                "RecordID": 0,
                "IsEndEvent": true,
                "IsTimerIntermediateEvent": false,
                "IsSignalIntermediateCatchEvent": false,
                "InSameLane": false,
                "IsGateway": false,
                "CustomReferenceExplain": "",
                "IsReceiveTask": false,
                "IsGraphMobileTask": false,
                "IsCallActivityTask": false
            }
        }
    }
}
```

**نمونه خروجی سطح 4 — Basic**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": {
            "ProcessCreationID": 53343
        }
    }
}
```

**نمونه خروجی سطح 3 — Minimum**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": null
    }
}
```

**نمونه خروجی سطح 2 — PlainLite**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": [
            {
                "Name": "Users",
                "Value": "1"
            }
        ]
    }
}
```

**نمونه خروجی سطح 1 — Lite**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": [
            {
                "Name": "Users",
                "Value": "1"
            }
        ]
    }
}
```

**نمونه خروجی سطح 0 — BoostLite**

```
{
    "state": "done",
    "content": {
        "HasError": false,
        "ErrorCode": -1,
        "Message": "فرآیند به پایان رسید",
        "MessageException": "",
        "Content": {
            "Users": "1"
        }
    }
}
```

#### مثال شروع فرایند

```
absoluteMethodHelper.runProcessWithVerbosityLevel({
        processGroupID : 4110, 
        variables:[{Name:"variableNumber1",Value:"someValue"}], 
        customTitle:'a title for process creation',
        runType:RunProcessType.MessageStartEvent,
        anonymous:false, 
        formId:null, 
        verboseLevel:6  
    }).then(res => 
        {
            console.log(res);
            // جهت باز کردن فرم پس شروع فرآیند می توانید از کد های زیر استفاده کنید 
            // const userTaskData = res.content?.Content?.TaskElementResult?.ProcessFlowModel; 
            // const { RecordID } = res.content?.Content?.TaskElementResult;
            // const { ReferenceID, ProcessCreationID } = userTaskData; 
            // _self: باز شدن فرم در پنجره فعلی (پیشفرض)
            // _blank: باز شدن فرم در پنجره / تب جدید
            // _top: باز شدن فرم در صفحه فعلی بصورت تمام صفحه 
            // userTaskData.target = "_self"; 
            // افزودن رکورد آی دی فرم
            // userTaskData.FormsRecordID = RecordID; 
            // if (!userTaskData) {
                  // toast.show('error', 'فرآیند اجرا شده دارای فرم نمی باشد.');
                  // return;
            // };

            // new serviceManager().formView(userTaskData, ReferenceID, ProcessCreationID, "i");            
            })
    .catch(ex =>
        {
            toast.show('error', translate('OperationFailed'), ex.message);
        });
```

#### شروع فرایند — روش Legacy برای نسخه‌های 2.1.5.131 و پایین‌تر

این روش برای سامانه‌هایی با نسخه 2.1.5.131 و پایین‌تر می باشد

**پارامتر های شروع فرایند :**

**processGroupID :** شناسه گروه فرایند

**processID :** شناسه فرایند

**roleID :** شناسه نقش شروع‌کننده فرایند

**userID :** شناسه کاربر شروع‌کننده فرایند

**fromOut :** true; //شروع فرایند همراه با فرم است

**variables :** متغیری که به فرایند ارسال می‌شود که به صورت آرایه ای Object ها می باشد

**customTitle :** عنوان فرایند ایجاد شده

**runType :** نوع المان شروع فرآیند

**anonymous :** شروع فرایند بدون احراز هویت

**formId :** شناسه فرم در صورت باز شدن فرم پس از شروع فرآیند

```
let processGroupID = 4110; 
let processID = 4531; 
let roleID = curentRoleId; 
let userID = curentUserId; 
let fromOut = true; 
let variables = [{Name: "variableNumber1",Value: "someValue"}]; 
let customTitle = 'a title for process creation'; 
let runType = 500002;
let anonymous = false; 
let formId = null;  
absoluteMethodHelper.runProcess(processGroupID,processID,roleID,userID,fromOut,variables,customTitle,runType,anonymous,formId).then(res => 
    {
      console.log(res);
      // const userTaskData = res.content?.Content?.TaskElementResult?.ProcessFlowModel; 
      // const { RecordID } = res.content?.Content?.TaskElementResult;
      // const { ReferenceID, ProcessCreationID } = userTaskData; 
      // _self: باز شدن فرم در پنجره فعلی (پیشفرض)
      // _blank: باز شدن فرم در پنجره / تب جدید
      // _top: باز شدن فرم در صفحه فعلی بصورت تمام صفحه 
      // userTaskData.target = "_self"; 
      // افزودن رکورد آی دی فرم
      // userTaskData.FormsRecordID = RecordID; 
      // if (!userTaskData) {
            // toast.show('error', 'فرآیند اجرا شده دارای فرم نمی باشد.');
            // return;
      // };

      // new serviceManager().formView(userTaskData, ReferenceID, ProcessCreationID, "i");
      })
      .catch(ex => 
      {
            toast.show('error', translate('OperationFailed'), ex.message);
      });
```

#### شروع فرایند به‌صورت سرویس

در این نوع فرآیند از ابتدا تا پایان هیچ نود بازدارنده ای وجود ندارد و فرآیند بدون وقفه به پایان میرسد

**لیست نودهای (المان) بازدارنده :**

1.UserTask

2.GraphMobileTask

3.ReceiveTask

4.TimerIntermediateCatchEvent

5.SignalIntermediateCatchEvent

6.CallActivityTask

**مثال :**

```
absoluteMethodHelper.runProcessWithVerbosityLevel({
        processGroupID : 4110, 
        variables:[{Name:"variableNumber1",Value:"someValue"}], 
        customTitle:'a title for process creation',
        runType:RunProcessType.MessageStartEvent,
        anonymous:false, 
        formId:null, 
        verboseLevel:0  
    }).then(res => 
        {
            console.log(res);         
        })
    .catch(ex =>
        {
            toast.show('error', translate('OperationFailed'), ex.message);
        });
```

* * *

#### ادامه فرایند با Message — روش جاری

اگر در فرایند Message Boundary/Event مرتبط وجود داشته باشد، متد `continueProcessByMessageWithVerbosityLevel(...)` را برای ادامه فرایند آموزش می‌دهد.

| پارامتر | کاربرد |
| --- | --- |
| `processCreationID` | شناسه نمونه فرایند |
| `stepID` | گام فرایند که Message Event در آن قرار دارد |
| `variables` | متغیرهای ارسالی به فرایند |
| `verboseLevel` | همان سطح جزئیات 0 تا 6 که در بخش شروع فرایند توضیح داده شد |

```
let processCreationID = 52160;
let stepID = 11234;
let variables = [{Name:"variableNumber1",Value:"someValue"}];
let verboseLevel = 6;
absoluteMethodHelper
    .continueProcessByMessageWithVerbosityLevel(processCreationID,stepID,variables,verboseLevel)
    .then(res => 
        {
            console.log(res);
            // جهت باز کردن فرم پس ادامه فرآیند می توانید از کد های زیر استفاده کنید 
            // const userTaskData = res.content?.Content?.TaskElementResult?.ProcessFlowModel; 
            // const { RecordID } = res.content?.Content?.TaskElementResult;
            // const { ReferenceID, ProcessCreationID } = userTaskData; 
            // _self: باز شدن فرم در پنجره فعلی (پیشفرض)
            // _blank: باز شدن فرم در پنجره / تب جدید
            // _top: باز شدن فرم در صفحه فعلی بصورت تمام صفحه 
            // userTaskData.target = "_self"; 
            // افزودن رکورد آی دی فرم
            // userTaskData.FormsRecordID = RecordID; 
            // if (!userTaskData) {
                  // toast.show('error', 'فرآیند اجرا شده دارای فرم نمی باشد.');
                  // return;
            // };

            // new serviceManager().formView(userTaskData, ReferenceID, ProcessCreationID, "i");              
        })
    .catch(ex =>
        {
            toast.show('error', translate('OperationFailed'), ex.message);
        });
```

#### ادامه فرایند — روش Legacy برای نسخه‌های 2.1.5.131 و پایین‌تر

این روش برای سامانه‌هایی با نسخه 2.1.5.131 و پایین‌تر می باشد

**processCreationID :** شناسه ایجاد فرآیند

**stepID :** گام فرایند که در آن Message Event قرار دارد

**variables :** متغییری که به فرایند ارسال می شود که به صورت آرایه ای از Object ها می باشد

**مثال :**

```
let processCreationID = 52160; 
let stepID = 11234; 
let variables = [{Name:"RoleId",Value:curentRoleId}]; 
absoluteMethodHelper
    .continueProcessWithMessage(processCreationID,stepID,variables)
    .then(res => 
        {
            console.log(res);
            // جهت باز کردن فرم پس ادامه فرآیند می توانید از کد های زیر استفاده کنید 
            // const userTaskData = res.content?.Content?.TaskElementResult?.ProcessFlowModel; 
            // const { RecordID } = res.content?.Content?.TaskElementResult;
            // const { ReferenceID, ProcessCreationID } = userTaskData; 
            // _self: باز شدن فرم در پنجره فعلی (پیشفرض)
            // _blank: باز شدن فرم در پنجره / تب جدید
            // _top: باز شدن فرم در صفحه فعلی بصورت تمام صفحه 
            // userTaskData.target = "_self"; 
            // افزودن رکورد آی دی فرم
            // userTaskData.FormsRecordID = RecordID; 
            // if (!userTaskData) {
                  // toast.show('error', 'فرآیند اجرا شده دارای فرم نمی باشد.');
                  // return;
            // };

            // new serviceManager().formView(userTaskData, ReferenceID, ProcessCreationID, "i");              
        })
    .catch(ex =>
        {
            toast.show('error', translate('OperationFailed'), ex.message);
        });
```

* * *

### Throw و Catch کردن Signal Eventهای فرآیند

در صورتی که نیاز دارید سیگنال مد نظر خود را فراخوانی کنید تا با توجه به کلید آن تمامی سیگنال های catch در شروع فرآیند، نمونه سازی و شروع شوند و سیگنال های catch در انتظار میانی ادامه پیدا کنند، می توانید از این سرویس استفاده کنید.

**signalKey :** عنوان یا نامی که مشخص کننده سیگنال مقصد می باشد، کلید سیگنال در زمان تعریف سیگنال مشخص شده است و مشخص کننده و تعریف کننده آن سیگنال می باشد.

**variables :** متغییرهایی که به فرآیند ارسال می شود که به صورت آرایه ای از Object ها می باشد

**مثال :**

```
let signalKey = 'SignalNumber7'; 
let variables = [{Name:"variableNumber1",Value:"someValue"}]; // متغییری که به فرآیند ارسال می شود
absoluteMethodHelper
    .throwProcessSignal(signalKey,variables)
    .then(res => 
        {
            console.log(res);
        })
    .catch(ex =>
        {
            toast.show('error', translate('OperationFailed'), ex.message);
        });
```

* * *

### کار با گزارش و قالب چاپ در absoluteMethodHelper

#### بازکردن گزارش جدولی

با استفاده از دستور زیر در Custom JS می توان گزارش های جدولی را باز کرد

```
 absoluteMethodHelper.openReport(
    ReportId,// شناسه گزارش
    // مقداردهی پیش فرض به پارامتر ها
    // *** اختیاری ***
    [
        {
            Params: [{Name: PARAM_NAME,Value: PARAM_VALUE}],
            ResourceId: RESOURCE_ID,
            ResourceName: RESOURCE_NAME
        },
    ]
)
```

**Reportid :** شناسه گزارش (کد گزارش در صفحه گزارش ساز)

**Param :** پارامتر های ورودی گزارش جدولی که باید به صورت آرایه ای از Object ها پر شود

**ResourceId :** شناسه منبع استفاده شده در گزارش ، این شناسه در پنل مشاهده گزارش و در بالای صفحه pop up قابل مشاهده می باشد

![محل نمایش ResourceId در گزارش](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=08CA4A9D-69DC-4BD1-A7D3-33D367B341CB.png)

محل نمایش ResourceId در گزارش

در صورتی که گزارش دارای چند منبع داده باشد همانند تصویر زیر ResourceId در کنار عنوان هر منبع داده درج شده است

![نمایش ResourceId برای گزارش دارای چند منبع داده](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AA871A9A-9FF0-4E61-9ACB-690E95990E90.png)

نمایش ResourceId برای گزارش دارای چند منبع داده

**مثال :**

```
absoluteMethodHelper.openReport(
    39,[
        {
            Params: [{Name: 'TrackingCode',Value: 24216}],
            ResourceId: 62,
            ResourceName: 'نمایش پیام های اخیر'
        },
    ]
)
```

* * *

#### دریافت خروجی قالب چاپ

از متد زیر برای دریافت خروجی از قالب چاپ طراحی شده برای یک گزارش استفاده می شود. خروجی قالب چاپ می تواند بصورت pdf و یا jpg توسط این متد دریافت شود.

```
absoluteMethodHelper.generateStimulExport(
    EXPORT_ID,
    null,
    null,
    {
        reportId: REPORT_ID,
        exportAccessId: EXPORT_ACCESS_ID,
        exportType: EXPORT_TYPE,
    }
) 
```

پارامتر های ورودی این متد عبارتند از:

**ExportId:** این پارامتر، شناسه قالب چاپ طراحی شده می باشد که در بخش مدیریت گزارش ها و در قسمت لیست قالب های قالب چاپ گزارش نمایش داده می شود.

**Parameters:** این پارامتر، برای ارسال مقدار به پارامتر های ورودی کوئری sql می باشد که می تواند بیش از یک مورد باشد .

**FileName:** این پارامتر، نام فایل خروجی این قالب چاپ را مشخص می کند.

**Type:** این پارامتر، نوع خروجی قالب چاپ را که می تواند pdf و یا jpg باشد را مشخص می کند.

**ResourceId :** شناسه منبع استفاده شده در گزارش ، این شناسه در پنل مشاهده گزارش و در بالای صفحه pop up قابل مشاهده می باشد

برای پیدا کردن `ResourceId` همان الگوی نمایش‌داده‌شده در بخش «بازکردن گزارش جدولی» را مبنا قرار دهید؛ در گزارش چندمنبعی، شناسه کنار عنوان هر منبع داده قابل تشخیص است.

**مثال :**

```
var paramList = [{
            ResourceId: 12, Parameters: [{ Name: 'FormId', Value: formId }]
        }]
absoluteMethodHelper.generateStimulExport(20, paramList, 'pdf');
```

* * *

### کار با فرم در absoluteMethodHelper

#### 1.باز کردن فرم از طریق absoluteMethodHelper :

با استفاده از تابع زیر می‌توانید فرم فرم‌ساز را باز کنید:

```
absoluteMethodHelper.openForm(FormId, RecordId = -1, Options = {})
```

-   پارامتر FormId در واقع شناسه فرم شما را مشخص میکند.
-   پارامتر RecordId شناسه رکورد شما را مشخص میکند که اختیاری و به طور پیش فرض برابر با -1 است.
-   پارامتر Options به صورت Object است و یک سری تنظیماتی در اختیار شما قرار میدهد که در ادامه به آن تنظیمات اشاره میکنیم:
    -   target: در صورتی که تنظیم openMode برابر با page باشد این تنظیم مشخص میکند که فرم در صفحه فعلی باز شود یا صفحه جدید. مقادیر قابل قبول:
        -   self: صفحه فعلی
        -   blank: صفحه جدید
    -   openMode: مشخص میکند که فرم به چه صورتی باز شود، مقادیر قابل قبول:
        -   popup: فرم به صورت popup باز خواهد شد.
        -   page: فرم در تب مرورگر باز خواهد شد.
    -   onSubmitSuccess: یک تابع میگیرد و موقعی که فرم باز شده با موفقیت ارسال شود تابع اجرا خواهد شد.
    -   popupWidth: در صورتی که تنظیم openMode برابر با popup باشد این تنظیم مشخص میکند که عرض popup چقدر باشد.
    -   popupHeight: در صورتی که تنظیم openMode برابر با popup باشد این تنظیم مشخص میکند که ارتفاع popup چقدر باشد.
    -   onShowing: یک تابع میگیرد و در صورتی که تنظیم openMode برابر با popup باشد آن تابع موقع نمایش داده شدن popup اجرا خواهد شد.
    -   onShownPopup: یک تابع میگیرد و در صورتی که تنظیم openMode برابر با popup باشد آن تابع موقعی که popup کاملا نمایش داده شد اجرا خواهد شد.
    -   onHidden: یک تابع میگیرد و در صورتی که تنظیم openMode برابر با popup باشد آن تابع موقعی که popup بسته شد اجرا خواهد شد.
    -   onCloseByButton: یک تابع میگیرد و موقعی که فرم باز شده با استفاده از فیلد دکمه بسته شد اجرا می شود.
    -   anonymous: مشخص میکند که آیا فرم در حالت ناشناس باز شود یا خیر.
    -   showMode: مشخص میکند که فرم در حالت نمایشی (فقط خواندنی) باز شود یا خیر.
    -   queryString: به صورت رشته است و در صورتی که تنظیم openMode برابر با page باشد با استفاده از این تنظیم می‌توانید به صفحه جدید باز شده query string پاس بدهید.
    -   params: به صورت object است و با استفاده از این تنظیم می‌توانید به فرم خود پارامتر پاس بدهید و داخل فرم با استفاده از `viewModel.getParam('param name')` به پارامتر خود دسترسی داشته باشید.

**مثال :**

```
var MessageTempID =  viewModel.fields.MediaListGrid.Fk_MessageTempIDConfig.value() // دریافت مقدار فیلد در فرم اول
var CitizenId = viewModel.fields.MediaListGrid.FK_CitizenIdConfig.value()
absoluteMethodHelper.openForm(
  103113, 0,
  {
      openMode: 'popup',
      onSubmitSuccess: function () {
          ShowMessages();
      },
      onShowing: function (event) {
          loaderDevko.viewModel.fields.FK_MessageTempIDConfig.value(MessageTempID); // مقدار دهی در فرم دوم که به صورت پاپ آپ باز می شود 
          loaderDevko.viewModel.fields.FK_CitizenIdConfig.value(CitizenId);
      }
  }
)
```

* * *

## کار با Toast، Alert و Confirm

### نمایش Toast

```
toast.show('success', 'Hello world !');
```

پارامتر اول نوع Toast و پارامتر دوم پیام است. نوع Toast:

1.  `success`: سبز، برای حالت موفقیت‌آمیز.
2.  `error`: قرمز، برای خطا یا پیام مهم.
3.  `info`: آبی، برای پیام‌های اطلاعاتی.
4.  `warning`: زرد، برای هشدار.

### مدیریت خطای Provider در الگوی جاری

در Helper جاری، خطاهای GraphBPMS و خطاهای Transport به Promise rejection می‌روند؛ بنابراین مسیر استاندارد مدیریت خطا `.catch(...)` است.

```
absoluteMethodHelper
    .runProvider(PROVIDER_ID, parameters)
    .then(result => {
        // success
    })
    .catch(error => {
        console.error(error);
        toast.show('error', 'اجرای Provider ناموفق بود');
    });
```

**\[وابسته به نسخه/استقرار\]** در نمونه‌های قدیمی ممکن است بررسی `result.HasError` داخل `.then(...)` دیده شود. برای Helper جاری، `.catch(...)` مرجع است.

### نمایش Alert

برای نمایش پیامی که تمرکز کاربر را روی یک Popup قرار دهد:

```
DevExpress.ui.dialog.alert('Hello world', 'Test Alert');
```

پارامتر اول پیام و پارامتر دوم Title مربوط به Popup است.

### نمایش Confirm

برای دریافت تأیید کاربر، مثلاً پیش از حذف داده مهم:

```
DevExpress.ui.dialog.confirm('از حذف این داده اطمینان دارید؟', 'هشدار').done(isOK => {
    if(isOK) {
        //...
    }
});
```

پارامتر اول پیام و پارامتر دوم عنوان Popup است.

[مستندات Dialog در DevExtreme](https://js.devexpress.com/jQuery/Documentation/ApiReference/Common/Utils/ui/dialog/)

> **مرجع فنی:** `alert(message, title)` یک PromiseLike برمی‌گرداند که بعد از بستن Alert تکمیل می‌شود. `confirm(message, title)` نیز PromiseLike برمی‌گرداند و نتیجه آن Boolean است؛ همین رفتار مبنای استفاده از `.done(isOK => ...)` در نمونه‌های بالا است.

## برنامه‌نویسی ناهمزمان در JavaScript

### مفهوم ناهمزمان (Asynchronous) در JavaScript به زبان ساده

فرض کنید به یک کافه رفته‌اید. شما سفارش خود را می‌دهید و منتظر می‌مانید تا آماده شود. در این مدت، می‌توانید به اطراف نگاه کنید، با دوستتان صحبت کنید یا کتاب بخوانید. یعنی در حالی که منتظر آماده شدن سفارش‌تان هستید، کارهای دیگری هم می‌توانید انجام دهید.

در برنامه‌نویسی هم همین اتفاق می‌افتد. وقتی یک برنامه JavaScript کاری را انجام می‌دهد که زمان‌بر است (مثل خواندن یک فایل از روی هارد یا درخواست داده از یک سرور)، برنامه می‌تواند به جای اینکه منتظر بماند تا این کار تمام شود، به سراغ انجام کارهای دیگری برود. اینجاست که مفهوم ناهمزمان وارد می‌شود.

به زبان ساده‌تر، ناهمزمان بودن یعنی اینکه یک کار به صورت همزمان با کارهای دیگر انجام شود. در JavaScript، بسیاری از کارها به صورت ناهمزمان انجام می‌شوند تا برنامه سریع‌تر و روان‌تر اجرا شود.

برنامه نویسی ناهمزمان (Asynchronous Programming) در JavaScript به این معناست که کد شما می‌تواند بدون انتظار برای اتمام یک عملیات خاص، به اجرای بخش‌های بعدی بپردازد. این ویژگی به خصوص در کار با عملیات‌های ورودی/خروجی (مثل خواندن از فایل، درخواست از سرور و ...) بسیار مفید است.

در دنیای برنامه‌نویسی JavaScript، به خصوص در محیط‌های مرورگر، بسیاری از عملیات‌ها به صورت ناهمزمان (Asynchronous) انجام می‌شوند. این بدین معناست که یک عملیات ممکن است زمان زیادی طول بکشد و در حین انجام آن، برنامه بتواند به اجرای سایر وظایف بپردازد. برای مدیریت این نوع عملیات‌ها، JavaScript از مفهوم Promise و کلمات کلیدی async/await استفاده می‌کند.

**پس به طور خلاصه، ناهمزمان بودن در JavaScript به برنامه اجازه می‌دهد که در حین انجام کارهای زمان‌بر، به کارهای دیگری هم بپردازد. این ویژگی باعث می‌شود برنامه‌ها سریع‌تر، روان‌تر و کاربرپسندتر شوند.**

### چرا برنامه نویسی ناهمزمان مهم است؟

-   کارایی بهتر: اگر برنامه شما برای دریافت داده از یک سرور منتظر بماند، در این مدت هیچ کار دیگری انجام نخواهد داد. اما با برنامه نویسی ناهمزمان، برنامه می‌تواند در همین حین به انجام کارهای دیگر بپردازد.
-   رابط کاربری روان‌تر: در برنامه‌های تحت وب، اگر درخواست‌های سرور زمان زیادی ببرد، رابط کاربری برای کاربر یخ می‌زند. با ناهمزمانی، کاربر می‌تواند در حین بارگذاری داده‌ها، با سایر بخش‌های برنامه تعامل داشته باشد.

### چگونه ناهمزمان بودن را در JavaScript پیاده‌سازی می‌کنیم؟

JavaScript روش‌های مختلفی برای پیاده‌سازی ناهمزمان بودن دارد، از جمله:

-   Callbacks: قدیمی‌ترین روش برای مدیریت ناهمزمان بودن است.
-   Promises: روشی مدرن‌تر و قدرتمندتر برای مدیریت ناهمزمان بودن است.
-   Async/Await: سینتکسی ساده‌تر برای کار با Promises است.

### برنامه نویسی ناهمزمان با استفاده از Callback function

در پیاده سازی کد ناهمزمان با استفاده از Callback، یک تابع به عنوان آرگومان به تابع دیگری پاس داده می‌شود و زمانی اجرا می‌شود که عملیات ناهمزمان به پایان برسد. برای مثال:

```
function fetchData(callback) {
  setTimeout(() => {
    const data = { message: 'Hello from the server!' };
    callback(data);
  }, 2000);
}

fetchData((data) => {
  console.log(data);
});
```

در مثال بالا، تابع fetchData یک تابع callback به عنوان آرگومان می‌گیرد. پس از 2 ثانیه (شبیه‌سازی یک درخواست به سرور)، تابع callback با داده‌های دریافت شده فراخوانی می‌شود.

### برنامه نویسی ناهمزمان با استفاده از Promise

Promise در JavaScript یک شیء است که نشان‌دهنده نتیجه نهایی یک عملیات ناهمزمان است (موفق یا ناموفق).به زبان ساده، Promise یک شیء است که نشان‌دهنده نتیجه نهایی یک عملیات ناهمزمان است. این عملیات می‌تواند هر چیزی باشد، از خواندن یک فایل گرفته تا درخواست اطلاعات از یک سرور.

### چرا به Promise نیاز داریم؟

قبل از Promise، ما از callback ها برای مدیریت عملیات‌های ناهمزمان استفاده می‌کردیم(مثال بالا). اما Callback ها باعث می‌شدند کدمان پیچیده و سخت‌خوان شود، مخصوصاً وقتی چندین عملیات ناهمزمان پشت سر هم داشتیم. Promise‌ها این مشکل را حل کردند و به ما اجازه می‌دهند کدهای ناهمزمان را به صورت زنجیره‌ای و خواناتر بنویسیم.

پس پیشنهاد ما به شما این است که همیشه به جای Callback function از Promise استفاده کنید.

یک Promise می‌تواند در سه حالت مختلف باشد:

-   Pending: عملیات هنوز در حال انجام است و نتیجه مشخص نیست.
-   Fulfilled: عملیات با موفقیت انجام شده و نتیجه مشخص شده است.
-   Rejected: عملیاتی با شکست مواجه شده و خطایی رخ داده است.

مثال:

```
function fetchData(url) {
    return new Promise((resolve, reject) => {
        // شبیه سازی ارسال درخواست به سرور
        setTimeout(() => {
            resolve({data: 'This is server data'});
        }, 1000);
    });
}

showLoading();

fetchData('https://foo.bar/...')
    .then(response => {
        console.log(response);// output: {data: 'This is server data'}
    })
    .catch(error => {
        console.log(error);
        alert('در دریافت اطلاعات از سرور با خطا مواجه شدیم');
    })
    .finally(() => {
        hideLoading();
    });
```

در مثال بالا، ما یک تابع داریم که یک Promise را ساخته و برمیگرداند و در عملیات این Promise درخواست Ajax را شبیه سازی کرده ایم که بعد از 1 ثانیه سرور به ما پاسخ میدهد و data مورد نظر را به ما تحویل میدهد.

همانطور که در مثال بالا مشاهده میکنید برای استفاده از یک Promise، از متدهای then() و catch() استفاده می‌کنیم.

-   then(): زمانی که Promise با موفقیت انجام شد، این تابع فراخوانی می‌شود.
-   catch(): زمانی که Promise با شکست مواجه شد، این تابع فراخوانی می‌شود.
-   finally(): این تابع در هر زمانی اجرا می‌شود، چه Promise با موفق اجرا شود چه با شکست مواجه شود.

### مزایای Promise:

-   خوانایی بهتر: کدهای ناهمزمان را به صورت زنجیره‌ای و خواناتر می‌نویسیم.
-   مدیریت بهتر خطاها: با استفاده از catch() می‌توانیم خطاهای رخ داده را به راحتی مدیریت کنیم.
-   قابلیت ترکیب: می‌توانیم چندین Promise را با هم ترکیب کنیم.

### Async/Await:

**لطفا تا زمانی که Promise را کاملا درک نکرده اید این قسمت را مطالعه نکنید !**

Async و Await دقیقا چی هستند؟ تصور کنید یک تابع داریم که داخل آن چندین Promise را هندل میکنیم:

```
function foo() {
    return new Promise((resolve, reject) => {
        fetchData('/data1')
            .then(data1 => {
                fetchData('/data2')
                    .then(data2 => {
                        fetchData('/data3')
                            .then(data3 => {
                                resolve({
                                    data1, data2, data3
                                });
                            })
                            .catch(err => {
                                reject(err);
                            });
                    })
                    .catch(err => {
                        reject(err);
                    });
            })
            .catch(err => {
                reject(err);
            });
    });
}

foo()
    .then(allData => {
        console.log(allData);// result: {data1: ..., data2: ..., data3: ...}
    })
    .catch(error => {
        alert('خطا');
    });
```

در مثال بالا میبینید که ما 3 تا Promise داریم و میخواهیم بعد از اتمام هر Promise، Promise بعدی اجرا گردد و مجبور شدیم کد ها را داخل همدیگر بنویسیم !

همانطور که پیداست کد های ما به شدت شلوغ و بهم ریخته هستند، فرض کنید در این کد با خطا مواجه شویم و بخواهیم کد را دی باگ کنیم، مسلما این کار بشدت دشوار و پیچیده خواهد شد. یا فرض کنید به جای 3 عدد Promise، 10 عدد Promise داشتیم! قطعا با جهنمی از Promise و کد های تودرتو مواجه خواهیم شد!

راه حل چیست؟ JavaScript برای حل این مسئله به ما Async و Await را پیشنهاد میکند.

در صورتی که قبل از هر متغییر یا تابعی که Promise برمیگرداند کلمه await را بگذاریم JavaScript تا اجرا شدن کامل Promise صبر میکند:

```
const data1 = await fetchData('/data1');
const data2 = await fetchData('/data2');
const data3 = await fetchData('/data3');

console.log({data1, data2, data3});// output: data1: ..., data2: ..., data3: ...
```

همانطور که میبینیدکد های ما بسیار بسیار خواناتر، توسعه پذیر تر، و حجم آنها بسیار کم شده است و خروجی این کد با کد مثال قبلی دقیقا یکسان است !

اما با اجرای این کد در مرورگر با خطا مواجه خواهیم شد!، چرا؟ به این دلیل که در JavaScript یک قانون برای استفاده از await وجود دارد:

**زمانی که در کد خود از await استفاده کنید، await شما حتما باید داخل یک تابع از نوع async باشد:**

```
async function foo() {
    const data1 = await fetchData('/data1');
    const data2 = await fetchData('/data2');
    const data3 = await fetchData('/data3');

    return {data1, data2, data3};
}

foo()
    .then(allData => {
        console.log(allData);// result: {data1: ..., data2: ..., data3: ...}
    })
    .catch(error => {
        alert('خطا');
    });
```

همانطور که در مثال بالا مشخص است، برای این که یک تابع را از نوع async کنید کافیست قبل از کلمه function از کلمه async استفاده کنید.

در واقع کلمه async به JavaScript میگوید که این تابع میتواند به صورت ناهمگام اجرا شود و دیگر JavaScript به شما خطایی نخواهد داد.

اما یک نکته بسیار ریزی در کد بالا وجود دارد، همانطور که در کد بالا میبینید ما در تابع foo، هیچ Promiseای را return نکرده ایم اما در پایین از then و catch استفاده کرده ایم. چطور چنین چیزی ممکن است؟

**در واقع وقتی تابع شما از نوع async باشد، هر چیزی که داخل تابع شما return شود در قالب یک Promise خواهد بود.**

بنابراین Objectای که داخل تابع foo ، بازگشت داده شده را باید در قالب یک Promise دریافت کنیم.

**پس همیشه به یاد داشته باشید، به طور کلی تمامی تابع های از نوع async خروجی Promise دارند.**

### Promise.all:

در مثال بالا، ما یک کدی نوشتیم که سه نوع داده را از سرور دریافت میکند (data1, data2, data3)

نکته ای که در این کد وجود دارد این است که این 3 کد به طور همزمان اجرا نمیشوند، بلکه بعد از اینکه data1 به طور کامل دریافت شد سپس data2 دریافت می‌شود و بعد از آن data3

فرض کنید که هرکدام از 3 داده ای که دریافت میشوند 1 ثانیه طول بکشد، در این صورت برنامه ما بعد از 3 ثانیه هر 3 داده را خواهد داشت.

به نظر شما اینکه هر 3 داده همزمان دریافت شوند و برنامه به طور همزمان منتظر دریافت هر 3 داده باشد و بعد به سراغ کد های بعدی برود بهتر نیست؟!

شاید با خود بگوئید که هدف ما از await این بود که تا زمانی که این Promise به طور کامل اجرا نشده کد بعدی اجرا نشود. حرف شما درست است اما برنامه ما در این مثال نیازی ندارد که حتما اول data1 به طور کامل دریافت شود، بعد data2 و بعد از آن data3.

**بعضی اوقات ما نیاز داریم که برنامه به طور همزمان منتظر چندین Promise باشد و بعد از اینکه به طور همزمان اجرا شدند با موفقیت تکمیل شدند سپس برنامه به سراغ اجرای خط های بعدی برود.**

در این مواقع میتوانیم از Promise.all استفاده کنیم:

```
async function foo() {
    const [data1, data2, data3] = await Promise.all([
        fetchData('/data1'),
        fetchData('/data2'),
        fetchData('/data3')
    ]);

    return {data1, data2, data3};
}
```

مثالی که زده شده با مثالی که بالاتر زده شد دقیقا یکسان است با این تفاوت که این مثال سریع تر انجام خواهد شد.

**در واقع Promise.all یک آرایه ای از Promise ها را میگیرد و خود یک Promise را بازگشت میدهد و زمانی که تمامی Promise هایی که داخل آرایه resolve شدند Promise مربوط به Promise.all هم resolve خواهد شد.**

پس در مثال بالا، همانطور که گفته شد، فرض کنید هرکدام از این دریافت داده ها از سمت سرور 1 ثانیه طول بکشند، در صورتی که از Promise.all استفاده نشود مدت زمان اجرای کد ها 3 ثانیه خواهد بود اما در صورتی که از Promise.all استفاده شود مدت زمان اجرای کد ها 1 ثانیه خواهد بود.

بنابراین، زمانی که چندین Promise پشت سر هم استفاده میکنید طوری که همزمان یا غیرهمزمان بودن آنها برای شما اهمیتی ندارد از Promise.all استفاده کنید تا کد شما سریع تر اجرا شود.

اگر دو Provider مستقل از یکدیگر هستند، `Promise.all` اجازه می‌دهد هر دو عملیات هم‌زمان شروع شوند و ادامه کد پس از موفقیت هر دو اجرا شود.

```
async function loadIndependentData() {
    const [usersResult, rolesResult] = await Promise.all([
        absoluteMethodHelper.runProvider(USERS_PROVIDER_ID),
        absoluteMethodHelper.runProvider(ROLES_PROVIDER_ID)
    ]);

    return { usersResult, rolesResult };
}
```

#### اجرای هم‌زمان Providerهای مستقل

وقتی عملیات دوم به نتیجه عملیات اول وابسته است، اجرای ترتیبی با `await` خوانایی بیشتری نسبت به Promiseهای تودرتو دارد.

```
async function loadDependentData(viewModel) {
    const firstResult = await absoluteMethodHelper.runProvider(
        FIRST_PROVIDER_ID,
        [{ name: 'RecordId', value: viewModel.data().Id() }]
    );

    const firstRows = JSON.parse(
        firstResult.Content.providerResult || '[]'
    );

    const key = firstRows[0]?.Key;
    if (!key) {
        return;
    }

    const secondResult = await absoluteMethodHelper.runProvider(
        SECOND_PROVIDER_ID,
        [{ name: 'Key', value: key }]
    );

    console.log(secondResult);
}
```

#### اجرای ترتیبی دو عملیات وابسته

در این الگو، Hook تا مشخص‌شدن نتیجه Confirm یک Promise برمی‌گرداند و مقدار Boolean نهایی تعیین می‌کند ارسال ادامه پیدا کند یا متوقف شود.

```
export const beforeSubmit = async function (viewModel) {
    const isOK = await DevExpress.ui.dialog.confirm(
        'از ارسال فرم اطمینان دارید؟',
        'هشدار'
    );

    return isOK;
}
```

#### توقف Submit تا دریافت Confirm

### الگوهای ناهمزمان در Custom JS GraphBPMS

## اسکریپت سمت سرور C#

اسکریپت سمت سرور اجازه می‌دهد دستورات روی Server اجرا شوند. در توضیح این قابلیت، یکی از نتایج اجرای سمت سرور این است که کدهای اجراشده برای کاربری که توسعه‌دهنده سامانه نیست مانند Custom JS سمت Client قابل مشاهده نیستند. برای مثال اگر منبع داده را در Custom JS فراخوانی کنید، دستورهای فراخوانی آن در سمت Client قابل مشاهده‌اند.

### نمونه پایه صفحه C# Script در استدیو فرم ساز

```
public class FormScript :BaseFormScript
{
    public override ResponseModel PreLoad()
    {   
        var result = new ResponseModel();
        return result;
    }
    public override ResponseModel BeforeSave()
    {   
        var result = new ResponseModel();
        return result;
    }
    public override ResponseModel AfterSave()
    {   
        var result = new ResponseModel();
        return result;
    }
    public override ResponseModel BeforeDelete()
    {
        var result = new ResponseModel();
        return result;
    }
    public override ResponseModel AfterDelete()
    {
        var result = new ResponseModel();
        return result;
    }   
}
```

### متدهای اسکریپت سمت سرور

#### `PreLoad`

قبل از باز شدن فرم اجرا می‌شود و در آن می‌توانید به Query String، شناسه کاربر و شناسه نقش دسترسی داشته باشید.

```
public override ResponseModel PreLoad()
{   
      var result = new ResponseModel();
      //scripts
      return result;
}
```

#### `BeforeSave`

قبل از ارسال داده‌های فرم به Server و ذخیره آن‌ها فراخوانی می‌شود. در این متد می‌توانید به داده‌های فرم و مقدار فیلدها دسترسی داشته باشید.

```
public override ResponseModel BeforeSave()
{   
      var result = new ResponseModel();
      // scripts
      return result;
}
```

#### `AfterSave`

زمانی اجرا می‌شود که فرم با موفقیت ثبت شود.

```
public override ResponseModel AfterSave()
{   
      var result = new ResponseModel();
      // scripts
      return result;
}
```

#### `BeforeDelete`

قبل از حذف رکورد فرم اجرا می‌شود.

```
public override ResponseModel BeforeDelete()
{
      var result = new ResponseModel();
      return result;
}
```

#### `AfterDelete`

پس از حذف رکورد فرم اجرا می‌شود.

```
public override ResponseModel AfterDelete()
{
      var result = new ResponseModel();
      return result;
}
```

### دسترسی به داده‌های فرم در اسکریپت سمت سرور

#### تنظیم شناسه رکورد پیش‌فرض فرم

```
Functions["SetRecordId"].DynamicInvoke(value);
// value برابر است با مقدار شناسه رکورد فرم 
```

> اگر برای فرم شناسه رکورد پیش‌فرض تنظیم کرده باشید، فرم در حالت ویرایش باز می‌شود.

#### دریافت شناسه کاربر

```
var userid = (int)Functions["GetUserID"].DynamicInvoke();
```

#### دریافت شناسه نقش

```
var roleid = (int)Functions["GetRoleID"].DynamicInvoke();
```

#### دریافت شناسه سازمان

```
var OrganizationID = (int)Functions["GetOrganizationID"].DynamicInvoke();
```

#### دریافت شناسه فرم

```
var formid = (int)Functions["GetFormId"].DynamicInvoke();
```

#### دریافت شناسه رکورد فرم

```
var recordid = (int)Functions["GetRecordId"].DynamicInvoke();
```

#### دریافت Query String

```
string value = (string)Functions["GetQueryString"].DynamicInvoke("key"); 
// در این مثال کلید برابر با key  و مقدار برابر با value  می باشد
```

#### دریافت اطلاعات Task

```
var task = (dynamic)Functions["GetTask"].DynamicInvoke();
var taskFormTitle = (string)task.FormTitle; // عنوان Task
```

**\[نیازمند تأیید فنی\]** مسیر دقیق دریافت شناسه Task در Build مقصد باید تأیید شود. از `task.FormTitle` فقط برای عنوان Task استفاده کنید و آن را مسیر Task ID در نظر نگیرید.

#### دریافت مقدار پیش‌فرض فیلد در فرم

```
var FormData = (JObject)Functions["GetFormData"].DynamicInvoke();
var value  = (string)FormData["TextField"]; // TextField نام فیلد در پایگاه داده
```

#### تغییر مقدار پیش‌فرض فیلد در فرم

```
var form = (dynamic)Functions["GetForm"].DynamicInvoke();
form["TextField"].TextSetting.DefaultValue = "hello"; //TextField نام فیلد و hello مقدار در نظر گرفته شده برای فیلد می باشد
```

### تنظیمات فیلدهای فرم در C# Script

#### اجباری کردن فیلد

```
var form = (dynamic)Functions["GetForm"].DynamicInvoke();
form["TextField"].Validation.Required = true;
```

#### فقط‌خواندنی کردن فیلد

```
var form = (dynamic)Functions["GetForm"].DynamicInvoke();
form["TextField"].Global.Readonly = true;
```

### فراخوانی منبع داده (Provider)

```
var Parameters = new Dictionary<string,string>();
Parameters.Add("ProviderId","1"); // شناسه منبع داده 
Parameters.Add("ParameterName1","ParameterValue1"); // پارامتر ورودی  منبع داده
Parameters.Add("ParameterName2","ParameterValue2"); // پارامتر ورودی منبع داده
var RunProviderResult = (DataTable)Functions["RunProvider"].DynamicInvoke(Parameters);
var username = (string)RunProviderResult.Rows[0]["UserName"]; // خروجی منبع داده
```

### شروع فرآیند

```
var ProcessParameters = new Dictionary<string,string>();
ProcessParameters.Add("Variables","[{'Name':'ParameterName1','Value':'ParameterValue1'},{'Name':'ParameterName2','Value':'ParameterValue2'}]"); // پارامتر های ورودی فرآیند
ProcessParameters.Add("Process","500001"); // شناسه فرآیند
ProcessParameters.Add("ProcessGroup","2"); // شناسه گروه فرآیند
ProcessParameters.Add("RunType","500002"); // نوع المان شروع فرآیند
ProcessParameters.Add("UserID","123"); // کاربر شروع کننده فرآیند
ProcessParameters.Add("RoleID","123"); // نقش شروع کننده فرآیند 
ProcessParameters.Add("ProcessTitle","Title"); // عنوان فرآیند
ProcessParameters.Add("KeepContinue","false"); 
var re = Functions["RunProcess"].DynamicInvoke(ProcessParameters);
```

-   اگر از `ProcessGroup` استفاده کرده‌اید، استفاده از `Process` لازم نیست.
-   برای شروع از نوع Message، در توضیح این بخش مقدار `50002` برای `RunType` ذکر شده است، در حالی که نمونه کد مقدار `500002` دارد. این دو مقدار متفاوت‌اند و پیش از استفاده باید مقدار صحیح برای نسخه سامانه تأیید شود.

## پیوست — فایل نمونه کامل Custom JS در استدیو فرم ساز

نسخه کامل Sample موجود در استدیو فرم ساز در فایل [`samples/custom-js-studio-sample.js`](data:application/javascript;base64,77u/Ly/Zhdiq2LrbjNixINiz2LHYp9iz2LHbjCDYrNmH2Kog2KfZhtiq2YLYp9mEINio2Ycg2KLYr9ix2LMg2KzYr9uM2K8NCmV4cG9ydCBsZXQgcmVkaXJlY3RVcmwgPSAnJzsNCg0KLy/Yqtin2KjYuduMINqp2Ycg2K/YsSDYstmF2KfZhiDZhNmI2K8g2YHYsdmFINmB2LHYp9iu2YjYp9mG24wg2YXbjNi02YjYrw0KZXhwb3J0IGNvbnN0IG9uTG9hZCA9IGZ1bmN0aW9uICh2aWV3TW9kZWwpIHsNCiAgICBkZWJ1Z2dlcjsNCiAgICAvLyrYqtiw2qnYsSDYr9ixINi12YjYsdiqINmF2K7ZgduMINio2YjYr9mGINmB24zZhNivINin2LIg2LPZhdiqINmB2LHYp9uM2YbYryDZiNuM2Ygg2YXYr9mEINii2YYg2YHbjNmE2K8g2YXZiNis2YjYryDZhtmF24wg2KjYp9i02K8qDQoNCiAgICAvL9mG2YXZiNmG2Ycg2qnYryDYp9iz2KrZgdin2K/ZhyDYp9iyINmG2YXZiNiv2KfYsQ0KICAgIC8v2KzZh9iqINin2LPYqtmB2KfYr9mHINin2LIg2YbZhdmI2K/Yp9ixINit2KrZhdinINiv2LEg2KjYp9mE2Kcg2YXYp9qY2YjZhCDZhtmF2YjYr9in2LEg2KfbjNmF2b7ZiNix2Kog2LTZiNivDQogICAgLy9jb25zdCBjaGFydENvbmZpZyA9IHsNCiAgICAvLyAgICBlbGVtZW50OiBkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgndHN0Q2hhcnQnKSwNCiAgICAvLyAgICBleHBvcnRJZDogMzY4LA0KICAgIC8vICAgIHNob3dab29tVG9vbDogZmFsc2UsLy/YqtmG2LjbjNmFINmG2YXYp9uM2LQv2LnYr9mFINmG2YXYp9uM2LQg2KfYqNiy2KfYsSDZhtmF2KfbjNi0INm+2YbYrNix2Ycg2KfbjA0KICAgIC8vICAgIHNob3dGaWx0ZXJUb29sOiBmYWxzZSwvL9iq2YbYuNuM2YUg2YbZhdin24zYtC/Yudiv2YUg2YbZhdin24zYtCDYp9io2LLYp9ixINmG2YXYp9uM2LQg2b7Zhtis2LHZhyDYqtmG2LjbjNmFINmB24zZhNiq2LEg2YfYpw0KICAgIC8vICAgIHNob3dSZW1vdmVGaWx0ZXJUb29sOiB0cnVlLC8v2KrZhti424zZhSDZhtmF2KfbjNi0L9i52K/ZhSDZhtmF2KfbjNi0INin2KjYstin2LEg2K3YsNmBINmB24zZhNiq2LEg2YfYpw0KICAgIC8vICAgIHNob3dSZWZyZXNoVG9vbDogZmFsc2UsLy/YqtmG2LjbjNmFINmG2YXYp9uM2LQv2LnYr9mFINmG2YXYp9uM2LQg2KfYqNiy2KfYsSDYqtin2LLZhyDYs9in2LLbjCDZhtmF2YjYr9in2LENCiAgICAvLyAgICBwYXJhbUxpc3Q6IFt7DQogICAgLy8gICAgICAgIFJlc291cmNlSWQ6IDI0OSwgUGFyYW1ldGVyczogWw0KICAgIC8vICAgICAgICAgICAgeyBOYW1lOiAnU2VsbFllYXInLCBWYWx1ZTogMTM5OSB9DQogICAgLy8gICAgICAgICAgICAsIHsgTmFtZTogJ1NlbGxNb250aCcsIFZhbHVlOiBudWxsIH0NCiAgICAvLyAgICAgICAgICAgICwgeyBOYW1lOiAnU2VsbEdvb2RzQ291bnQnLCBWYWx1ZTogbnVsbCB9DQogICAgLy8gICAgICAgIF0NCiAgICAvLyAgICB9XQ0KICAgIC8vfQ0KDQogICAgLy/YqNix2KfbjCDYr9ix24zYp9mB2Kog2KfbjNmG2LPYqtmG2LMg24zaqdmH2YHbjNmE2K8g2KjYtdmI2LHYqiDZhdiz2KrZgtuM2YUNCiAgICAvL2RldkhlbHBlci5nZXRXaWRnZXRJbnN0YW5jZSgkKCIjY2hhbmdlcE5vc2F6aUNvZGVCdG4iKSkNCg0KICAgIC8vdmlld01vZGVsLmRhdGEoKS5Mb2NhdGlvbi5zdWJzY3JpYmUoZnVuY3Rpb24gKG5ld1ZhbHVlKSB7DQogICAgLy8gICAgYWxlcnQoIlRoZSBwZXJzb24ncyBuZXcgbmFtZSBpcyAiICsgbmV3VmFsdWUpOw0KICAgIC8vfSk7DQoNCiAgICAvLy0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS3ZhdmC2K/Yp9ixINm+24zYtCDZgdix2LYg2KjYsdin24wg2YHYp9uM2YQg2KLZvtmE2YjYrw0KICAgIC8vaWYgKGdsb2JhbC5nZXRRdWVyeVN0cmluZ1BhcmFtKCJyZWNJZCIpID09IG51bGwgfHwgZ2xvYmFsLmdldFF1ZXJ5U3RyaW5nUGFyYW0oInJlY0lkIikgPT0gdW5kZWZpbmVkKSB7DQogICAgLy8gICAgaWYgKHZpZXdNb2RlbC5kYXRhKCkuRmlsZVVwbG9hZCAhPSB1bmRlZmluZWQpIHsNCg0KICAgIC8vICAgICAgICB2aWV3TW9kZWwuZGF0YSgpLkZpbGVVcGxvYWQoDQogICAgLy8gICAgICAgICAgICBbew0KICAgIC8vICAgICAgICAgICAgICAgIEFyY2hpdmVDb2RlOiBrby5vYnNlcnZhYmxlKCJCNjgzMDc1MC00MjEyLTQxMjgtQkY0Qi01QTQ1MDJGOTU5ODciKSwNCiAgICAvLyAgICAgICAgICAgICAgICBJc0RlbGV0ZWQ6IGtvLm9ic2VydmFibGUoZmFsc2UpLA0KICAgIC8vICAgICAgICAgICAgICAgIEtpbmQ6IGtvLm9ic2VydmFibGUoMSkNCiAgICAvLyAgICAgICAgICAgIH1dDQogICAgLy8gICAgICAgICkNCiAgICAvLyAgICB9DQogICAgLy8gICAgZWxzZSB7DQogICAgLy8gICAgICAgIHZpZXdNb2RlbC5kYXRhKCkuRmlsZVVwbG9hZCA9IGtvLm9ic2VydmFibGUoDQogICAgLy8gICAgICAgICAgICBbew0KICAgIC8vICAgICAgICAgICAgICAgIEFyY2hpdmVDb2RlOiBrby5vYnNlcnZhYmxlKCJCNjgzMDc1MC00MjEyLTQxMjgtQkY0Qi01QTQ1MDJGOTU5ODciKSwNCiAgICAvLyAgICAgICAgICAgICAgICBJc0RlbGV0ZWQ6IGtvLm9ic2VydmFibGUoZmFsc2UpLA0KICAgIC8vICAgICAgICAgICAgICAgIEtpbmQ6IGtvLm9ic2VydmFibGUoMSkNCiAgICAvLyAgICAgICAgICAgIH1dDQogICAgLy8gICAgICAgICkNCg0KICAgIC8vICAgIH0NCiAgICAvL30NCiAgICAvLy0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0NCg0KICAgIC8v2YbZhdmI2YbZhyDaqdivINin2LPYqtmB2KfYr9mHINin2LIg2KfbjNis2qnYsw0KICAgIC8vKtmG2qnYqtmHINmF2YfZhSA6INio2LnYryDYp9iyINiv2LHbjNin2YHYqiDYp9i32YTYp9i52KfYqiDYp9iyINin24zYrNqp2LMg2K3YqtmF2Kcg2KfYsiB0cnkg2KfYs9iq2YHYp9iv2Ycg2YbZhdin24zbjNivDQogICAgLy8kLmFqYXgoew0KICAgIC8vICAgIHVybDogIi9Db250cm9sbGVyTmFtZS9BY3Rpb25OYW1lIiwNCiAgICAvLyAgICB0eXBlOiAiUE9TVCIsDQogICAgLy8gICAgZGF0YToga28udG9KU09OKHsgZGF0YTpkYXRhIH0pLA0KICAgIC8vICAgIHRpbWVvdXQ6IGFqYXhUaW1lT3V0LA0KICAgIC8vICAgIGNvbnRlbnRUeXBlOiAiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsDQogICAgLy8gICAgYmVmb3JlU2VuZDogKHhocikgPT4gew0KICAgIC8vICAgICAgICBsb2FkUGFuZWwuaW5kaWNhdG9yU2hvdygpOw0KICAgIC8vICAgIH0NCiAgICAvL30pLmRvbmUoKHJlc3VsdCkgPT4gew0KICAgIC8vICAgIGlmIChyZXN1bHQuSGFzRXJyb3IpIHsNCiAgICAvLyAgICAgICAgdG9hc3Quc2hvdygnZXJyb3InLCByZXN1bHQuTWVzc2FnZSk7DQogICAgLy8gICAgfQ0KICAgIC8vICAgIGVsc2Ugew0KICAgIC8vICAgICAgICB0cnkgew0KICAgIC8vICAgICAgICAgICAgLy8uLg0KICAgIC8vICAgICAgICB9DQogICAgLy8gICAgICAgIGNhdGNoIChlcnIpIHsNCiAgICAvLyAgICAgICAgICAgIGNvbnNvbGUubG9nKGVyci5tZXNzYWdlKTsNCiAgICAvLyAgICAgICAgICAgIGxvYWRQYW5lbC5pbmRpY2F0b3JIaWRlKCk7DQogICAgLy8gICAgICAgICAgICB0b2FzdC5zaG93KCdlcnJvcicsICfZhdiq2YYg2K7Yt9inJyk7DQogICAgLy8gICAgICAgIH0NCiAgICAvLyAgICB9DQogICAgLy99DQogICAgLy8gICAgKS5mYWlsKChlcnJvclRocm93bikgPT4gew0KICAgIC8vICAgICAgICB0b2FzdC5zaG93KCdlcnJvcicsICfZhdiq2YYg2K7Yt9inJyk7DQogICAgLy8gICAgfSkuYWx3YXlzKCgpID0+IHsNCiAgICAvLyAgICAgICAgbG9hZFBhbmVsLmluZGljYXRvckhpZGUoKTsNCiAgICAvLyAgICB9KTsNCg0KICAgIC8vKirYqtiw2qnYsSDYqNiz24zYp9ixINmF2YfZhSDYr9ixINiq2YjYp9io2Lkg2K7ZiNivINiq2Kcg2K3YryDYp9mF2qnYp9mGINin2LIgdHJ5INin2LPYqtmB2KfYr9mHINmG2YXYp9uM24zYry7Yr9ix2LXZiNix2Kog2KjYsdmI2LIg2K7Yt9inINiv2LEg2KzbjCDYp9izINi02YXYpyDZhdmF2qnZhiDYp9iz2Kog2LnZhdmE24zYp9iqINmB2LHZhSDYs9in2LIg2YXYrtiq2YQg2LTZiNivKioNCiAgICAvLyoq2K/YsSBkb25lIGFqYXgg2K7ZiNivINit2KrZhdinINin2LIgdHJ5INin2LPYqtmB2KfYr9mHINmG2YXYp9uM24zYryoqDQogICAgLy90cnkgew0KICAgIC8vICAgIC8vLi4uDQogICAgLy99DQogICAgLy9jYXRjaCAoZXJyKSB7DQogICAgLy8gICAgY29uc29sZS5sb2coZXJyLm1lc3NhZ2UpOw0KICAgIC8vICAgIGxvYWRQYW5lbC5pbmRpY2F0b3JIaWRlKCk7DQogICAgLy8gICAgdG9hc3Quc2hvdygnZXJyb3InLCAn2K7Yt9inICcpOw0KICAgIC8vfQ0KDQogICAgLy8q2KrYsNqp2LE62K/aqdmF2Ycg2LDYrtuM2LHZhyDYs9in2LLbjCDZgdix2YUg2K/YsSDZgdix2YUg2LPYp9iyINmIINiv2qnZhdmHINiw2K7bjNix2Ycg2LPYp9iy24wg2YHYsdmFINiv2LEg2YHYsdin24zZhtivINmF2KrZgdin2YjYqiDZhduMINio2KfYtNmG2K8qDQogICAgLy/YqNi32YjYsSDZhdir2KfZhCDYr9ixINi12YjYsdiqINmF2K7ZgduMINqp2LHYr9mGINiv2qnZhdmHINiw2K7bjNix2Ycg2YHYsdmFINiz2KfYsiAg2KfbjNmGINiv2qnZhdmHINiv2LEg2YHYsdin24zZhtivINmG2YXYp9uM2LQg2K/Yp9iv2Ycg2YXbjNi02YjYrw0KDQogICAgLy/YtNin2YXZhCDYqtmF2KfZhduMINiq2YbYuNuM2YXYp9iqINmIINmF2YLYp9iv24zYsSDZgduM2YTYryDZh9in24wg2YXZiNis2YjYryDYr9ixINmB2LHZhQ0KICAgIC8vdmlld01vZGVsDQoNCiAgICAvL9is2YfYqiDYp9mG2KrYrtin2Kgg2qnYsdiv2YYg2YXZgtin2K/bjNixINmB24zZhNivINmH2KcNCiAgICAvL3ZpZXdNb2RlbC5kYXRhKCkuZmllbGROYW1lKCkNCg0KICAgIC8v2KzZh9iqINin2YbYqtiu2KfYqCDaqdix2K/ZhiDYrtmI2K8g2YHbjNmE2K8g2Ygg2KfYudmF2KfZhCDYqti624zbjNix2KfYqg0KICAgIC8vdmlld01vZGVsLmZpZWxkcy5maWVsZE5hbWVDb25maWcgLy/YqtmG2LjbjNmF2KfYqiDZgduM2YTYryDYrNmH2Kog2KfYudmF2KfZhCDYqti624zbjNix2KfYqiDYqNixINix2YjbjCDYrtmI2K8g2YHbjNmE2K8NCiAgICAvL3ZpZXdNb2RlbC5maWVsZHMuZmllbGROYW1lX0luc3RhbmNlIC8vINin24zZhtiz2KrZhtizINmB24zZhNivINis2YfYqiDYp9i52YXYp9mEINiq2LrbjNuM2LHYp9iqINio2LEg2LHZiNuMINiu2YjYryDZgduM2YTYrw0KICAgIC8vdmlld01vZGVsLmZpZWxkcy5maWVsZE5hbWVfVmFsaWRhdGlvbiAvL9iq2YbbjNi424zZhdin2Kog2YjZhNuM2K/bjNi02YYg2YHbjNmE2K8g2KzZh9iqINiq2LrbjNuM2LEg2Ygg24zYpyDZhdi02KfZh9iv2Ycg2YjZhNuM2K/bjNi02YYg2YfYp9uMINiz2Kog2LTYr9mHINio2LEg2LHZiNuMINmB24zZhNivDQogICAgLy92aWV3TW9kZWwuZmllbGRzLmZpZWxkTmFtZV9WaXNpYmlsaXR5IC8v2KrZhti424zZhdin2Kog2YbZhdin24zYtCDZgduM2YTYrw0KDQogICAgLy/ZhtmF2YjZhtmHINqp2K8g2KrYutuM24zYsSDYr9in2K/ZhiDYqtmG2LjbjNmF2KfYqiDZgduM2YTYryDZh9inINiv2LEg2YHYsdmFINiz2KfYsg0KICAgIC8vdmlld01vZGVsLmZpZWxkcy5maWVsZE5hbWVfSW5zdGFuY2Uub3B0aW9uKCdkaXNhYmxlZCcsIHRydWUpDQoNCiAgICAvL9mG2YXZiNmG2Ycg2qnYryDYrNmH2Kog2KfZhtis2KfZhSDYqti624zbjNixINmF2LPYqtmC24zZhSDYqNixINix2YjbjCDZgduM2YTYryDZh9in24wg2YHYsdmFINiz2KfYsg0KICAgIC8vZG9jdW1lbnQucXVlcnlTZWxlY3RvcignW2RhdGEtYmluZD0iZHhUZXh0QXJlYTogZmllbGRzLmZpZWxkTmFtZUNvbmZpZyAsIGR4VmFsaWRhdG9yOiBmaWVsZHMuZmllbGROYW1lX1ZhbGlkYXRpb24iXScpLnN0eWxlLmhlaWdodCA9ICIxNTBweCINCg0KICAgIC8v2KzZh9iqINmG2YXYp9uM2LQgbG9hZGVyDQogICAgLy9sb2FkUGFuZWwuaW5kaWNhdG9yU2hvdygpOw0KICAgIC8v2KzZh9iqINmF2K7ZgduMINqp2LHYr9mGIGxvYWRlcg0KICAgIC8vLy9sb2FkUGFuZWwuaW5kaWNhdG9ySGlkZSgpOw0KDQogICAgLy/YrNmH2Kog2q/YsdmB2KrZhiBlbnRpdHlNb2RlINmB2LHYp9uM2YbYryAtINmF2YjYryDZiNin2LHYryDaqdix2K/ZhiDYp9i32YTYp9i52KfYqiDYr9ixINmB2LHZhQ0KICAgIC8vbGV0IGVudGl0eU1vZGUgPSBuZXcgVVJMU2VhcmNoUGFyYW1zKHdpbmRvdy5sb2NhdGlvbi5zZWFyY2gpLmdldCgnZW50aXR5TW9kZScpOw0KDQogICAgLy/ZhtmF2YjZhtmHINqp2K8g2KzZh9iqINin2YHYstmI2K/ZhiDYs9iq2YjZhiDYp9qp2LTZhiDYqNmHINis2K/ZiNmEINiq2YjYr9ix2KrZiA0KICAgIC8vZGV2SGVscGVyLmdldFdpZGdldEluc3RhbmNlKCQoJyPZhtin2YUg2KrZiNiv2LHYqtmIJykpLmFkZENvbHVtbih7DQogICAgLy8gICAgd2lkdGg6IDE1MCwNCiAgICAvLyAgICBjYXB0aW9uOiAi2obYp9m+IiwNCiAgICAvLyAgICBmaXhlZDogdHJ1ZSwNCiAgICAvLyAgICBmaXhlZFBvc2l0aW9uOiAnbGVmdCcsDQogICAgLy8gICAgYWxpZ25tZW50OiAnY2VudGVyJywNCiAgICAvLyAgICBjZWxsVGVtcGxhdGU6IGZ1bmN0aW9uIChjb250YWluZXIsIG9wdGlvbnMpIHsNCiAgICAvLyAgICAgICAgbGV0IGZsYWcgPSBmYWxzZTsvLyFjaGVja1BhaWQodmlld01vZGVsLCBvcHRpb25zLnJvd0luZGV4KTsNCiAgICAvLyAgICAgICAgJCgiPGRpdiAvPiIpLmR4QnV0dG9uKHsNCiAgICAvLyAgICAgICAgICAgIHN0eWxpbmdNb2RlOiAiY29udGFpbmVkIiwNCiAgICAvLyAgICAgICAgICAgIC8vdGV4dDogItqG2KfZviIsDQogICAgLy8gICAgICAgICAgICBpY29uOiAiZmEgZmEtcHJpbnQiLA0KICAgIC8vICAgICAgICAgICAgZGlzYWJsZWQ6IGZsYWcsDQogICAgLy8gICAgICAgICAgICBvbkNsaWNrOiAoKSA9PiB7DQogICAgLy8gICAgICAgICAgICAgICAgICDYrNmH2Kog2KfYs9iq2YHYp9iv2Ycg2KfYsiDZgtin2YTYqCDahtin2b4NCiAgICAvLyAgICAgICAgICAgICAgICB2YXIgcGFyYW1MaXN0ID0gW3sNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgUmVzb3VyY2VJZDogMSwgUGFyYW1ldGVyczogW3sgTmFtZTogJ1JlY29yZElkJywgVmFsdWU6IG9wdGlvbnMucm93LmtleSB9XQ0KICAgIC8vICAgICAgICAgICAgICAgIH0NCiAgICAvLyAgICAgICAgICAgICAgICBdDQogICAgLy8gICAgICAgICAgICAgICAgYWJzb2x1dGVNZXRob2RIZWxwZXIuZ2VuZXJhdGVTdGltdWxFeHBvcnQoMiwgcGFyYW1MaXN0LCAncGRmJyk7DQogICAgLy8gICAgICAgICAgICB9DQogICAgLy8gICAgICAgIH0pLmFwcGVuZFRvKGNvbnRhaW5lcik7DQogICAgLy8gICAgfQ0KICAgIC8vfSk7DQoNCiAgICAvL9mG2YXZiNmG2Ycg2qnYryDYrNmH2Kog2KfYs9iq2YHYp9iv2Ycg2KfYsiDZgtin2YTYqCDahtin2b4NCiAgICAvLyAgICAgICAgICAgICAgICB2YXIgcGFyYW1MaXN0ID0gW3sNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgUmVzb3VyY2VJZDogMSwgUGFyYW1ldGVyczogW3sgTmFtZTogJ1JlY29yZElkJywgVmFsdWU6IG9wdGlvbnMucm93LmtleSB9XQ0KICAgIC8vICAgICAgICAgICAgICAgIH0NCiAgICAvLyAgICAgICAgICAgICAgICBdDQogICAgLy8gICAgICAgICAgICAgICAgYWJzb2x1dGVNZXRob2RIZWxwZXIuZ2VuZXJhdGVTdGltdWxFeHBvcnQoMiwgcGFyYW1MaXN0LCAncGRmJyk7DQoNCiAgICAvL9is2YfYqiDZhtmF2KfbjNi0INm+24zYutin2YUg2KjZhyDaqdin2LHYqNixDQogICAgLy90b2FzdC5zaG93KCdlcnJvcicsIHJlc3VsdC5NZXNzYWdlKTsNCg0KICAgIC8v2YbZhdmI2YbZhyDaqdivINis2YfYqiDYr9ix24zYp9mB2Kog2K/Yp9iv2Ycg2YfYp9uMINmB2LHZhSDYqtmI2K/Ysdiq2YgNCiAgICAvL3ZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVtyb3dJbmRleF0uQmlsbElkKCk7DQoNCiAgICAvLyrYqtiw2qnYsSDZhdmH2YUgOiDYr9ixINi12YjYsdiqINiq2LrbjNuM2LEg2YXZgtin2K/bjNixINis2K/ZiNmEINiq2Ygg2K/YsSDYqtmIINiv2Ygg2K3Yp9mE2Kog2YjYrNmI2K8g2K/Yp9ix2K8qDQogICAgLy8q2K3Yp9mE2Kog2KfZiNmEINix2qnZiNix2K8g2KfYsiDZgtio2YQg2K/YsSDYr9uM2KrYpyDYqNuM2LMg2YjYrNmI2K8g2K/Yp9ix2K8g2qnZhyDYr9ixINin24zZhiDYtdmI2LHYqiDaqdin2LHYqNixINmF24wg2KjYp9uM2LPYqiDYr9mIINiu2Lcg2LLbjNixINix2Kcg2KfYrNix2Kcg2YbZhdin24zYrw0KICAgIC8vdmlld01vZGVsLkludm9pY2VfTGlzdCgpW3Jvd0luZGV4XVtGb3JtQnVpbGRlclN0YXRpY3MuUmVjb3JkU3RhdGVdKFJlY29yZFN0YXRlVHlwZS5lZGl0KTsNCiAgICAvL3ZpZXdNb2RlbC5JbnZvaWNlX1JlY29yZCgpW0Zvcm1CdWlsZGVyU3RhdGljcy5SZWNvcmRTdGF0ZV0oUmVjb3JkU3RhdGVUeXBlLmVkaXQpOw0KICAgIC8vKtit2KfZhNiqINiv2YjZhSDYsdqp2YjYsdivINmH2YbZiNiyINiv2LEg2b7Yp9uM2q/Yp9mHINiv2KfYr9mHINiw2K7bjNix2Ycg2YbYtNiv2Ycg2KfYs9iqINqp2Ycg2K/YsSDYp9uM2YYg2LXZiNix2Kog2YbYqNin24zYryDYr9mIINiu2Lcg2KjYp9mE2Kcg2LHYpyDYp9is2LHYpyDaqdix2K8NCiAgICAvL9io2Ycg2KfbjNmGINiu2KfYt9ixINqp2Ycg2LHaqdmI2LHYryDYrdin2YTYqiDZiNuM2LHYp9uM2LQg2q/YsdmB2KrZhyDZiCDYr9ixINm+2KfbjNqv2KfZhyDYr9in2K/ZhyDYsNiu24zYsdmHINmG2YXbjNi02K8qDQoNCiAgICAvL9is2YfYqiDYp9mB2LLZiNiv2YYgbWFzayDYqNixINix2YjbjCDZgduM2YTYryDZh9inDQogICAgLy92aWV3TW9kZWwuZmllbGRzLmZpZWxkTmFtZV9JbnN0YW5jZS5vcHRpb24oJ21hc2snLCAiXFwwXFw5MDAwMDAwMDAwIik7DQogICAgLy92aWV3TW9kZWwuZmllbGRzLmZpZWxkTmFtZV9JbnN0YW5jZS5vcHRpb24oJ21hc2tJbnZhbGlkTWVzc2FnZScsICLYtNmF2KfYsdmHINmH2YXYsdin2Ycg2YjYp9ix2K8g2LTYr9mHINi12K3bjNitINmG2YXbjCDYqNin2LTYryIpOw0KICAgIC8vdmlld01vZGVsLmZpZWxkcy5maWVsZE5hbWVfSW5zdGFuY2Uub3B0aW9uKCdpbnB1dEF0dHInLCB7IHN0eWxlOiAnZGlyZWN0aW9uOiBsdHI7JyB9KTsNCiAgICAvL3ZpZXdNb2RlbC5maWVsZHMuZmllbGROYW1lX0luc3RhbmNlLm9wdGlvbigndXNlTWFza2VkVmFsdWUnLCB0cnVlKTsNCg0KICAgIC8v2K/YsSDYtdmI2LHYqiDYp9iz2KrZgdin2K/ZhyDYp9iyINin2K/bjNiq2YjYsSDYp9qGINiq24wg2KfZhSDYp9mEINmIINmC2LHYp9ix2K/Yp9iv2YYg2K/aqdmF2Ycg2KjYsdin24wg2KjYp9uM2YbYryDaqdix2K/ZhiDYp9iyINix2YjYtCDYstuM2LEg2KfYs9iq2YHYp9iv2Ycg2YbZhdin24zbjNivDQogICAgLy9pZiAodmlld01vZGVsLmZpZWxkcy5wcmludEJ0bl9IVE1MQ29uZmlnICYmICQoIiNwcmludEJ0biIpKSB7DQogICAgLy8kKCIjcHJpbnRCdG4iKS5keEJ1dHRvbih7DQogICAgLy8gICAgc3R5bGluZ01vZGU6ICJjb250YWluZWQiLA0KICAgIC8vICAgIHR5cGU6ICJkZWZhdWx0IiwNCiAgICAvLyAgICBpY29uOiAicHJpbnQiLA0KICAgIC8vICAgIHRleHQ6ICLahtin2b4iLA0KICAgIC8vICAgIGRpc2FibGVkOiB2aWV3TW9kZWwuZmllbGRzLnByaW50QnRuX0hUTUxDb25maWcuZGlzYWJsZWQsDQogICAgLy8gICAgb25DbGljazogKCkgPT4gew0KICAgIC8vICAgICAgICAvL2RlYnVnZ2VyOw0KICAgIC8vICAgICAgICB2YXIgcGFyYW1MaXN0ID0gW3sNCiAgICAvLyAgICAgICAgICAgIFJlc291cmNlSWQ6IDMsIFBhcmFtZXRlcnM6IFt7IE5hbWU6ICdyZWNvcmRJZCcsIFZhbHVlOiB2aWV3TW9kZWwuZGF0YSgpLklkKCkgfV0NCiAgICAvLyAgICAgICAgfSwNCiAgICAvLyAgICAgICAgeyBSZXNvdXJjZUlkOiA0LCBQYXJhbWV0ZXJzOiBbeyBOYW1lOiAncmVjb3JkSWQnLCBWYWx1ZTogdmlld01vZGVsLmRhdGEoKS5JZCgpIH1dIH0sDQogICAgLy8gICAgICAgIHsgUmVzb3VyY2VJZDogNSwgUGFyYW1ldGVyczogW3sgTmFtZTogJ3JlY29yZElkJywgVmFsdWU6IHZpZXdNb2RlbC5kYXRhKCkuSWQoKSB9XSB9DQogICAgLy8gICAgICAgIF0NCiAgICAvLyAgICAgICAgYWJzb2x1dGVNZXRob2RIZWxwZXIuZ2VuZXJhdGVTdGltdWxFeHBvcnQoNiwgcGFyYW1MaXN0LCAncGRmJyk7DQogICAgLy8gICAgfQ0KICAgIC8vfSk7DQoNCiAgICAvL9mG2YXZiNmG2Ycg2qnYryDaqdin2LEg2KjYpyDZgduM2YTYryDZhtmC2LTZhw0KDQogICAgLy9vbkxvYWRNYXAuc3Vic2NyaWJlKCh2YWx1ZSkgPT4gew0KICAgIC8vICAgIC8v2K/ZgtiqINmB2LHZhdin24zbjNivINqp2Ycg2YHYsdmF2Kog2YXZiNmC2LnbjNiqINmF2YjYsdivINmG24zYp9iyINio2KfbjNivINio2Ycg2LTaqdmEINiy24zYsSDYqNin2LTYrw0KICAgIC8vICAgIGxldCBnZW9Kc29uID0gJ3tcInR5cGVcIjpcIkZlYXR1cmVDb2xsZWN0aW9uXCIsXCJmZWF0dXJlc1wiOlt7XCJ0eXBlXCI6XCJGZWF0dXJlXCIsXCJnZW9tZXRyeVwiOntcInR5cGVcIjpcIlBvbHlnb25cIixcImNvb3JkaW5hdGVzXCI6W1tbNTkuNTI0OTA4OTU2ODczODQ0LDM2LjM1MTM1NjU0MzUxNzUzXSxbNTkuNTYzNTYyNTkyMDI0MDU2LDM2LjM1MTkwOTU2NDAwMDMzXSxbNTkuNTY2NDgzMDg4OTAyMDcsMzYuMzM1NzMyMDkxOTMyNDhdLFs1OS41NDUzNTI0MzUwMTk5MzYsMzYuMzI1MDgzNTQ2MDAzODddLFs1OS41MTcwMDY0MzU5MDk3ODUsMzYuMzIzMDA4OTg0NTcxNjM2XSxbNTkuNTA0OTgwODYwNTI5NzUsMzYuMzMzMzgxMjM5NDQ5ODZdLFs1OS41MDEyMDEzOTM5ODE2OTUsMzYuMzUyNDYyNTgwNTU0NzFdLFs1OS41MjQ5MDg5NTY4NzM4NDQsMzYuMzUxMzU2NTQzNTE3NTNdXV19LFwicHJvcGVydGllc1wiOnt9fV19JzsNCiAgICAvLyAgICBzd2l0Y2ggKHZhbHVlLmZpZWxkTmFtZSkgew0KICAgIC8vICAgICAgICBjYXNlICJHcHNGaWVsZDEiOi8vR3BzRmllbGQxINmG2KfZhSDZgduM2YTYryDYqtiz2Kog2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgLy/YrNmH2Kog2LDYrtuM2LHZhyDYs9in2LLbjCDZhdmI2YLYuduM2Kog2K/YsSDZiNuM2Ygg2YXYr9mEINio2KfbjNivINin2LIg2qnYryDYstuM2LEg2KfYs9iq2YHYp9iv2Ycg2YbZhdmI2K8u2KrZiNis2Ycg2YHYsdmF2KfbjNuM2K8g2qnZhyDZhtin2YUg2YHbjNmE2K8g2K7ZiNivINix2Kcg2YjYp9ix2K8g2YbZhdin24zbjNivDQogICAgLy8gICAgICAgICAgICB2aWV3TW9kZWwuZGF0YSgpLkdwc0ZpZWxkMShnZW9Kc29uKTsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIC8v2YfYsSDZgduM2YTYryDZhtmC2LTZhyDYr9in2LHYp9uMINiq2q/bjCDZhdiu2YHbjCDZhduMINio2KfYtNivINqp2Ycg2K/YsSDYtdmI2LHYqiDYp9mG2KrYrtin2Kgg2YXZiNmC2LnbjNiqINmF2YLYr9in2LEg2YXZiNmC2LnbjNiqINiv2LHZiNmGINii2YYg2YLYsdin2LEg2YXbjCDar9uM2LHYryDZiCDYr9ixINmH2Ybar9in2YUg2LDYrtuM2LHZhyDYs9in2LLbjCDYqNmHINiz2YXYqiDYs9ix2YjYsSDYp9ix2LPYp9mEINmF24wg2LTZiNivINio2Ycg2YfZhduM2YYg2K7Yp9i32LEg2YLYt9i52Ycg2qnYryDYstuM2LEg2YbbjNiyINis2YfYqiDYsNiu24zYsdmHINiz2KfYstuMINmF2YjZgti524zYqiDZhdmI2LHYryDZhti42LEg2KfZhNiy2YXbjCDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAkKCdpbnB1dFtuYW1lPSJHcHNGaWVsZDEiXScpLnZhbChnZW9Kc29uKTsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIC8v2KjYr9iz2Kog2KLZiNix2K/ZhiDYp9mG2K/bjNizINmB24zZhNivINmG2YLYtNmHINio2LEg2KfYs9in2LMg2YbYp9mFINmB24zZhNivINiv2LEg2YTbjNiz2Kog2YbZgti02Ycg2YfYpw0KICAgIC8vICAgICAgICAgICAgc2VsZWN0ZWRNYXBJbmRleCA9IF8uZmluZEluZGV4KG1hcExpc3QsIChvYmopID0+IHsNCiAgICAvLyAgICAgICAgICAgICAgICByZXR1cm4gb2JqLm9wdGlvbnMuZWxlbWVudElkID09PSAiR3BzRmllbGQxIjsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIH0pOw0KICAgIC8vICAgICAgICAgICAgLy/YrNmH2Kog2LHYs9mFINmG2YLYtNmHINiMINiv2YLYqiDZgdix2YXYp9uM24zYryDZgti32LnZhyDaqdivINiy24zYsSDZgdmC2Lcg2KzZh9iqINix2LPZhSDZhdmI2YLYuduM2Kog2K/YsSDZhtmC2LTZhyDZhduMINio2KfYtNivINmIINiv2LEg2YfZhtqv2KfZhSDaqdmE24zaqSDYqNixINix2YjbjCDYsNiu24zYsdmHINmB2LHZhSDYp9i32YTYp9i52KfYqiDZhdmI2YLYuduM2Kog2LHYpyDYqNmHINiz2YXYqiDYs9ix2YjYsSDYp9ix2LPYp9mEINmG2YXbjCDZhtmF2KfbjNivLtio2LHYp9uMINiw2K7bjNix2Ycg2YXZiNmC2LnbjNiqINio2KfbjNivINin2LIg2YLYt9i52Ycg2qnYryDZh9in24wg2KjYp9mE2Kcg2KfYs9iq2YHYp9iv2Ycg2YbZhdmI2K8uDQogICAgLy8gICAgICAgICAgICBHcmFwaE1hcHMuTWFwTWFuYWdlci5kcmF3TGF5ZXJPbk1hcChnZW9Kc29uKTsNCg0KICAgIC8vICAgICAgICAgICAgYnJlYWs7DQogICAgLy8gICAgICAgIGNhc2UgIkdwc0ZpZWxkMiI6Ly9HcHNGaWVsZDIg2YbYp9mFINmB24zZhNivINiq2LPYqiDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAvL9is2YfYqiDYsNiu24zYsdmHINiz2KfYstuMINmF2YjZgti524zYqiDYr9ixINmI24zZiCDZhdiv2YQg2KjYp9uM2K8g2KfYsiDaqdivINiy24zYsSDYp9iz2KrZgdin2K/ZhyDZhtmF2YjYry7YqtmI2KzZhyDZgdix2YXYp9uM24zYryDaqdmHINmG2KfZhSDZgduM2YTYryDYrtmI2K8g2LHYpyDZiNin2LHYryDZhtmF2KfbjNuM2K8NCiAgICAvLyAgICAgICAgICAgIHZpZXdNb2RlbC5kYXRhKCkuR3BzRmllbGQyKGdlb0pzb24pOy8vR3BzRmllbGQyINmG2KfZhSDZgduM2YTYryDYqtiz2Kog2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgLy/Zh9ixINmB24zZhNivINmG2YLYtNmHINiv2KfYsdin24wg2Krar9uMINmF2K7ZgduMINmF24wg2KjYp9i02K8g2qnZhyDYr9ixINi12YjYsdiqINin2YbYqtiu2KfYqCDZhdmI2YLYuduM2Kog2YXZgtiv2KfYsSDZhdmI2YLYuduM2Kog2K/YsdmI2YYg2KLZhiDZgtix2KfYsSDZhduMINqv24zYsdivINmIINiv2LEg2YfZhtqv2KfZhSDYsNiu24zYsdmHINiz2KfYstuMINio2Ycg2LPZhdiqINiz2LHZiNixINin2LHYs9in2YQg2YXbjCDYtNmI2K8g2KjZhyDZh9mF24zZhiDYrtin2LfYsSDZgti32LnZhyDaqdivINiy24zYsSDZhtuM2LIg2KzZh9iqINiw2K7bjNix2Ycg2LPYp9iy24wg2YXZiNmC2LnbjNiqINmF2YjYsdivINmG2LjYsSDYp9mE2LLZhduMINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgICQoJ2lucHV0W25hbWU9Ikdwc0ZpZWxkMiJdJykudmFsKGdlb0pzb24pOy8vR3BzRmllbGQyINmG2KfZhSDZgduM2YTYryDYqtiz2Kog2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgLy/YqNiv2LPYqiDYotmI2LHYr9mGINin2YbYr9uM2LMg2YHbjNmE2K8g2YbZgti02Ycg2KjYsSDYp9iz2KfYsyDZhtin2YUg2YHbjNmE2K8g2K/YsSDZhNuM2LPYqiDZhtmC2LTZhyDZh9inDQogICAgLy8gICAgICAgICAgICBzZWxlY3RlZE1hcEluZGV4ID0gXy5maW5kSW5kZXgobWFwTGlzdCwgKG9iaikgPT4gew0KICAgIC8vICAgICAgICAgICAgICAgIHJldHVybiBvYmoub3B0aW9ucy5lbGVtZW50SWQgPT09ICJHcHNGaWVsZDIiOy8vR3BzRmllbGQyINmG2KfZhSDZgduM2YTYryDYqtiz2Kog2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgfSk7DQogICAgLy8gICAgICAgICAgICAvL9is2YfYqiDYsdiz2YUg2YbZgti02Ycg2Iwg2K/ZgtiqINmB2LHZhdin24zbjNivINmC2LfYudmHINqp2K8g2LLbjNixINmB2YLYtyDYrNmH2Kog2LHYs9mFINmF2YjZgti524zYqiDYr9ixINmG2YLYtNmHINmF24wg2KjYp9i02K8g2Ygg2K/YsSDZh9mG2q/Yp9mFINqp2YTbjNqpINio2LEg2LHZiNuMINiw2K7bjNix2Ycg2YHYsdmFINin2LfZhNin2LnYp9iqINmF2YjZgti524zYqiDYsdinINio2Ycg2LPZhdiqINiz2LHZiNixINin2LHYs9in2YQg2YbZhduMINmG2YXYp9uM2K8u2KjYsdin24wg2LDYrtuM2LHZhyDZhdmI2YLYuduM2Kog2KjYp9uM2K8g2KfYsiDZgti32LnZhyDaqdivINmH2KfbjCDYqNin2YTYpyDYp9iz2KrZgdin2K/ZhyDZhtmF2YjYry4NCiAgICAvLyAgICAgICAgICAgIEdyYXBoTWFwcy5NYXBNYW5hZ2VyLmRyYXdMYXllck9uTWFwKGdlb0pzb24pOw0KICAgIC8vICAgICAgICAgICAgYnJlYWs7DQogICAgLy8gICAgfQ0KICAgIC8vfSk7DQoNCiAgICAvL29uTWFwQ2xpY2suc3Vic2NyaWJlKCh2YWx1ZSkgPT4gew0KICAgIC8vICAgIHN3aXRjaCAodmFsdWUuZmllbGROYW1lKSB7DQogICAgLy8gICAgICAgIGNhc2UgIkdwc0ZpZWxkMSI6Ly9HcHNGaWVsZDEg2YbYp9mFINmB24zZhNivINiq2LPYqiDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAvL9is2YfYqiDYsNiu24zYsdmHINiz2KfYstuMINmF2YjZgti524zYqiDYr9ixINmI24zZiCDZhdiv2YQg2KjYp9uM2K8g2KfYsiDaqdivINiy24zYsSDYp9iz2KrZgdin2K/ZhyDZhtmF2YjYry7YqtmI2KzZhyDZgdix2YXYp9uM24zYryDaqdmHINmG2KfZhSDZgduM2YTYryDYrtmI2K8g2LHYpyDZiNin2LHYryDZhtmF2KfbjNuM2K8NCiAgICAvLyAgICAgICAgICAgIHZpZXdNb2RlbC5kYXRhKCkuR3BzRmllbGQyKG1hcExpc3RbdmFsdWUubWFwSW5kZXhdLmtvR3BzVmFsdWUoKSk7Ly9HcHNGaWVsZDIg2YbYp9mFINmB24zZhNivINiq2LPYqiDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAvL9mH2LEg2YHbjNmE2K8g2YbZgti02Ycg2K/Yp9ix2KfbjCDYqtqv24wg2YXYrtmB24wg2YXbjCDYqNin2LTYryDaqdmHINiv2LEg2LXZiNix2Kog2KfZhtiq2K7Yp9ioINmF2YjZgti524zYqiDZhdmC2K/Yp9ixINmF2YjZgti524zYqiDYr9ix2YjZhiDYotmGINmC2LHYp9ixINmF24wg2q/bjNix2K8g2Ygg2K/YsSDZh9mG2q/Yp9mFINiw2K7bjNix2Ycg2LPYp9iy24wg2KjZhyDYs9mF2Kog2LPYsdmI2LEg2KfYsdiz2KfZhCDZhduMINi02YjYryDYqNmHINmH2YXbjNmGINiu2KfYt9ixINmC2LfYudmHINqp2K8g2LLbjNixINmG24zYsiDYrNmH2Kog2LDYrtuM2LHZhyDYs9in2LLbjCDZhdmI2YLYuduM2Kog2YXZiNix2K8g2YbYuNixINin2YTYstmF24wg2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgJCgnaW5wdXRbbmFtZT0iR3BzRmllbGQyIl0nKS52YWwobWFwTGlzdFt2YWx1ZS5tYXBJbmRleF0ua29HcHNWYWx1ZSgpKTsvL0dwc0ZpZWxkMiDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIC8v2KjYr9iz2Kog2KLZiNix2K/ZhiDYp9mG2K/bjNizINmB24zZhNivINmG2YLYtNmHINio2LEg2KfYs9in2LMg2YbYp9mFINmB24zZhNivINiv2LEg2YTbjNiz2Kog2YbZgti02Ycg2YfYpw0KICAgIC8vICAgICAgICAgICAgc2VsZWN0ZWRNYXBJbmRleCA9IF8uZmluZEluZGV4KG1hcExpc3QsIChvYmopID0+IHsNCiAgICAvLyAgICAgICAgICAgICAgICByZXR1cm4gb2JqLm9wdGlvbnMuZWxlbWVudElkID09PSAiR3BzRmllbGQyIjsvL0dwc0ZpZWxkMiDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIH0pOw0KICAgIC8vICAgICAgICAgICAgLy/YrNmH2Kog2LHYs9mFINmG2YLYtNmHINiMINiv2YLYqiDZgdix2YXYp9uM24zYryDZgti32LnZhyDaqdivINiy24zYsSDZgdmC2Lcg2KzZh9iqINix2LPZhSDZhdmI2YLYuduM2Kog2K/YsSDZhtmC2LTZhyDZhduMINio2KfYtNivINmIINiv2LEg2YfZhtqv2KfZhSDaqdmE24zaqSDYqNixINix2YjbjCDYsNiu24zYsdmHINmB2LHZhSDYp9i32YTYp9i52KfYqiDZhdmI2YLYuduM2Kog2LHYpyDYqNmHINiz2YXYqiDYs9ix2YjYsSDYp9ix2LPYp9mEINmG2YXbjCDZhtmF2KfbjNivLtio2LHYp9uMINiw2K7bjNix2Ycg2YXZiNmC2LnbjNiqINio2KfbjNivINin2LIg2YLYt9i52Ycg2qnYryDZh9in24wg2KjYp9mE2Kcg2KfYs9iq2YHYp9iv2Ycg2YbZhdmI2K8uDQogICAgLy8gICAgICAgICAgICBHcmFwaE1hcHMuTWFwTWFuYWdlci5kcmF3TGF5ZXJPbk1hcChtYXBMaXN0W3ZhbHVlLm1hcEluZGV4XS5rb0dwc1ZhbHVlKCkpOw0KICAgIC8vICAgICAgICAgICAgYnJlYWs7DQogICAgLy8gICAgICAgIGNhc2UgIkdwc0ZpZWxkMiI6Ly9HcHNGaWVsZDIg2YbYp9mFINmB24zZhNivINiq2LPYqiDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAvL9is2YfYqiDYsNiu24zYsdmHINiz2KfYstuMINmF2YjZgti524zYqiDYr9ixINmI24zZiCDZhdiv2YQg2KjYp9uM2K8g2KfYsiDaqdivINiy24zYsSDYp9iz2KrZgdin2K/ZhyDZhtmF2YjYry7YqtmI2KzZhyDZgdix2YXYp9uM24zYryDaqdmHINmG2KfZhSDZgduM2YTYryDYrtmI2K8g2LHYpyDZiNin2LHYryDZhtmF2KfbjNuM2K8NCiAgICAvLyAgICAgICAgICAgIHZpZXdNb2RlbC5kYXRhKCkuR3BzRmllbGQxKG1hcExpc3RbdmFsdWUubWFwSW5kZXhdLmtvR3BzVmFsdWUoKSk7Ly9HcHNGaWVsZDEg2YbYp9mFINmB24zZhNivINiq2LPYqiDZhduMINio2KfYtNivDQogICAgLy8gICAgICAgICAgICAvL9mH2LEg2YHbjNmE2K8g2YbZgti02Ycg2K/Yp9ix2KfbjCDYqtqv24wg2YXYrtmB24wg2YXbjCDYqNin2LTYryDaqdmHINiv2LEg2LXZiNix2Kog2KfZhtiq2K7Yp9ioINmF2YjZgti524zYqiDZhdmC2K/Yp9ixINmF2YjZgti524zYqiDYr9ix2YjZhiDYotmGINmC2LHYp9ixINmF24wg2q/bjNix2K8g2Ygg2K/YsSDZh9mG2q/Yp9mFINiw2K7bjNix2Ycg2LPYp9iy24wg2KjZhyDYs9mF2Kog2LPYsdmI2LEg2KfYsdiz2KfZhCDZhduMINi02YjYryDYqNmHINmH2YXbjNmGINiu2KfYt9ixINmC2LfYudmHINqp2K8g2LLbjNixINmG24zYsiDYrNmH2Kog2LDYrtuM2LHZhyDYs9in2LLbjCDZhdmI2YLYuduM2Kog2YXZiNix2K8g2YbYuNixINin2YTYstmF24wg2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAgICAgJCgnaW5wdXRbbmFtZT0iR3BzRmllbGQxIl0nKS52YWwobWFwTGlzdFt2YWx1ZS5tYXBJbmRleF0ua29HcHNWYWx1ZSgpKTsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIC8v2KjYr9iz2Kog2KLZiNix2K/ZhiDYp9mG2K/bjNizINmB24zZhNivINmG2YLYtNmHINio2LEg2KfYs9in2LMg2YbYp9mFINmB24zZhNivINiv2LEg2YTbjNiz2Kog2YbZgti02Ycg2YfYpw0KICAgIC8vICAgICAgICAgICAgc2VsZWN0ZWRNYXBJbmRleCA9IF8uZmluZEluZGV4KG1hcExpc3QsIChvYmopID0+IHsNCiAgICAvLyAgICAgICAgICAgICAgICByZXR1cm4gb2JqLm9wdGlvbnMuZWxlbWVudElkID09PSAiR3BzRmllbGQxIjsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgICAgIH0pOw0KDQogICAgLy8gICAgICAgICAgICAvL9is2YfYqiDYsdiz2YUg2YbZgti02Ycg2Iwg2K/ZgtiqINmB2LHZhdin24zbjNivINmC2LfYudmHINqp2K8g2LLbjNixINmB2YLYtyDYrNmH2Kog2LHYs9mFINmF2YjZgti524zYqiDYr9ixINmG2YLYtNmHINmF24wg2KjYp9i02K8g2Ygg2K/YsSDZh9mG2q/Yp9mFINqp2YTbjNqpINio2LEg2LHZiNuMINiw2K7bjNix2Ycg2YHYsdmFINin2LfZhNin2LnYp9iqINmF2YjZgti524zYqiDYsdinINio2Ycg2LPZhdiqINiz2LHZiNixINin2LHYs9in2YQg2YbZhduMINmG2YXYp9uM2K8u2KjYsdin24wg2LDYrtuM2LHZhyDZhdmI2YLYuduM2Kog2KjYp9uM2K8g2KfYsiDZgti32LnZhyDaqdivINmH2KfbjCDYqNin2YTYpyDYp9iz2KrZgdin2K/ZhyDZhtmF2YjYry4NCiAgICAvLyAgICAgICAgICAgIEdyYXBoTWFwcy5NYXBNYW5hZ2VyLmRyYXdMYXllck9uTWFwKG1hcExpc3RbdmFsdWUubWFwSW5kZXhdLmtvR3BzVmFsdWUoKSk7DQogICAgLy8gICAgICAgICAgICBicmVhazsNCiAgICAvLyAgICB9DQogICAgLy99KTsNCg0KICAgIC8vJCgnI2J0bkNvbnZlcnQnKS5keEJ1dHRvbih7DQogICAgLy8gICAgdGV4dDogJ0NvbnZlcnQnLA0KICAgIC8vICAgIG9uQ2xpY2s6ICgpID0+IHsNCiAgICAvLyAgICAgICAgLy/Yr9mC2Kog2YHYsdmF2KfbjNuM2K8g2qnZhyDZgdix2YXYqiDZhdmI2YLYuduM2Kog2YXZiNix2K8g2YbbjNin2LIg2KjYp9uM2K8g2KjZhyDYtNqp2YQg2LLbjNixINio2KfYtNivDQogICAgLy8gICAgICAgIGxldCBnZW9Kc29uID0gJ3tcInR5cGVcIjpcIkZlYXR1cmVDb2xsZWN0aW9uXCIsXCJmZWF0dXJlc1wiOlt7XCJ0eXBlXCI6XCJGZWF0dXJlXCIsXCJnZW9tZXRyeVwiOntcInR5cGVcIjpcIlBvbHlnb25cIixcImNvb3JkaW5hdGVzXCI6W1tbNTkuNTI0OTA4OTU2ODczODQ0LDM2LjM1MTM1NjU0MzUxNzUzXSxbNTkuNTYzNTYyNTkyMDI0MDU2LDM2LjM1MTkwOTU2NDAwMDMzXSxbNTkuNTY2NDgzMDg4OTAyMDcsMzYuMzM1NzMyMDkxOTMyNDhdLFs1OS41NDUzNTI0MzUwMTk5MzYsMzYuMzI1MDgzNTQ2MDAzODddLFs1OS41MTcwMDY0MzU5MDk3ODUsMzYuMzIzMDA4OTg0NTcxNjM2XSxbNTkuNTA0OTgwODYwNTI5NzUsMzYuMzMzMzgxMjM5NDQ5ODZdLFs1OS41MDEyMDEzOTM5ODE2OTUsMzYuMzUyNDYyNTgwNTU0NzFdLFs1OS41MjQ5MDg5NTY4NzM4NDQsMzYuMzUxMzU2NTQzNTE3NTNdXV19LFwicHJvcGVydGllc1wiOnt9fV19JzsNCiAgICAvLyAgICAgICAgLy/YrNmH2Kog2LDYrtuM2LHZhyDYs9in2LLbjCDZhdmI2YLYuduM2Kog2K/YsSDZiNuM2Ygg2YXYr9mEINio2KfbjNivINin2LIg2qnYryDYstuM2LEg2KfYs9iq2YHYp9iv2Ycg2YbZhdmI2K8u2KrZiNis2Ycg2YHYsdmF2KfbjNuM2K8g2qnZhyDZhtin2YUg2YHbjNmE2K8g2K7ZiNivINix2Kcg2YjYp9ix2K8g2YbZhdin24zbjNivDQogICAgLy8gICAgICAgIHZpZXdNb2RlbC5kYXRhKCkuR3BzRmllbGQxKGdlb0pzb24pOy8vR3BzRmllbGQxINmG2KfZhSDZgduM2YTYryDYqtiz2Kog2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAvL9mH2LEg2YHbjNmE2K8g2YbZgti02Ycg2K/Yp9ix2KfbjCDYqtqv24wg2YXYrtmB24wg2YXbjCDYqNin2LTYryDaqdmHINiv2LEg2LXZiNix2Kog2KfZhtiq2K7Yp9ioINmF2YjZgti524zYqiDZhdmC2K/Yp9ixINmF2YjZgti524zYqiDYr9ix2YjZhiDYotmGINmC2LHYp9ixINmF24wg2q/bjNix2K8g2Ygg2K/YsSDZh9mG2q/Yp9mFINiw2K7bjNix2Ycg2LPYp9iy24wg2KjZhyDYs9mF2Kog2LPYsdmI2LEg2KfYsdiz2KfZhCDZhduMINi02YjYryDYqNmHINmH2YXbjNmGINiu2KfYt9ixINmC2LfYudmHINqp2K8g2LLbjNixINmG24zYsiDYrNmH2Kog2LDYrtuM2LHZhyDYs9in2LLbjCDZhdmI2YLYuduM2Kog2YXZiNix2K8g2YbYuNixINin2YTYstmF24wg2YXbjCDYqNin2LTYrw0KICAgIC8vICAgICAgICAkKCdpbnB1dFtuYW1lPSJHcHNGaWVsZDEiXScpLnZhbChnZW9Kc29uKTsvL0dwc0ZpZWxkMSDZhtin2YUg2YHbjNmE2K8g2KrYs9iqINmF24wg2KjYp9i02K8NCiAgICAvLyAgICAgICAgLy/YqNiv2LPYqiDYotmI2LHYr9mGINin2YbYr9uM2LMg2YHbjNmE2K8g2YbZgti02Ycg2KjYsSDYp9iz2KfYsyDZhtin2YUg2YHbjNmE2K8g2K/YsSDZhNuM2LPYqiDZhtmC2LTZhyDZh9inDQogICAgLy8gICAgICAgIHNlbGVjdGVkTWFwSW5kZXggPSBfLmZpbmRJbmRleChtYXBMaXN0LCAob2JqKSA9PiB7DQogICAgLy8gICAgICAgICAgICByZXR1cm4gb2JqLm9wdGlvbnMuZWxlbWVudElkID09PSAiR3BzRmllbGQxIjsNCiAgICAvLyAgICAgICAgfSk7DQogICAgLy8gICAgICAgIC8v2KzZh9iqINix2LPZhSDZhtmC2LTZhyDYjCDYr9mC2Kog2YHYsdmF2KfbjNuM2K8g2YLYt9i52Ycg2qnYryDYstuM2LEg2YHZgti3INis2YfYqiDYsdiz2YUg2YXZiNmC2LnbjNiqINiv2LEg2YbZgti02Ycg2YXbjCDYqNin2LTYryDZiCDYr9ixINmH2Ybar9in2YUg2qnZhNuM2qkg2KjYsSDYsdmI24wg2LDYrtuM2LHZhyDZgdix2YUg2KfYt9mE2KfYudin2Kog2YXZiNmC2LnbjNiqINix2Kcg2KjZhyDYs9mF2Kog2LPYsdmI2LEg2KfYsdiz2KfZhCDZhtmF24wg2YbZhdin24zYry7YqNix2KfbjCDYsNiu24zYsdmHINmF2YjZgti524zYqiDYqNin24zYryDYp9iyINmC2LfYudmHINqp2K8g2YfYp9uMINio2KfZhNinINin2LPYqtmB2KfYr9mHINmG2YXZiNivLg0KICAgIC8vICAgICAgICBHcmFwaE1hcHMuTWFwTWFuYWdlci5kcmF3TGF5ZXJPbk1hcChnZW9Kc29uKTsNCiAgICAvLyAgICB9DQogICAgLy99KTsNCg0KICAgIC8vIGNvbnNvbGUubG9nKHZpZXdNb2RlbC5nZXRQYXJhbSgnUEFSQU1fTkFNRScpKTsvLyDYr9ix24zYp9mB2Kog2b7Yp9ix2KfZhdiq2LEg2KfYsdiz2KfZhCDYtNiv2Ycg2KjZhyDZgdix2YUNCg0KICAgIC8vLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLQ0KICAgIGNvbnNvbGUubG9nKCdoZWxsbyBmcm9tIGN1c3RvbSBqcycpOw0KICAgIGNvbnNvbGUubG9nKGtvLm1hcHBpbmcudG9KUyh2aWV3TW9kZWwuZGF0YSkpOw0KfQ0KDQovL9iq2KfYqNi524wg2qnZhyDYr9ixINiy2YXYp9mGINii2YXYp9iv2Ycg2LTYr9mGINmB2LHZhSDZgdix2KfYrtmI2KfZhtuMINmF24zYtNmI2K8NCmV4cG9ydCBjb25zdCBvblJlYWR5ID0gZnVuY3Rpb24gKHZpZXdNb2RlbCkgew0KICAgIC8vIC4uLg0KfQ0KDQovLyDYqtin2KjYuduMINqp2Ycg2YXZiNmC2Lkg2YbZhdin24zYtCDYqtioINii24zYqtmFINmB2LHYp9iu2YjYp9mG24wg2YXbjNi02YjYrw0KZXhwb3J0IGNvbnN0IG9uVGFiTG9hZCA9IGZ1bmN0aW9uICh0YWJQYW5lbE5hbWUsIHRhYlBhbmVsSXRlbU5hbWUsIHZpZXdNb2RlbCkgew0KICAgIGlmICh0YWJQYW5lbE5hbWUgPT09ICdZT1VSX09XTl9UQUJfUEFORUxfTkFNRScpIHsNCiAgICAgICAgaWYgKHRhYlBhbmVsSXRlbU5hbWUgPT09ICdZT1VSX09XTl9UQUJfSVRFTV9OQU1FJykgew0KICAgICAgICAgICAgLy8gLi4uDQogICAgICAgIH0NCiAgICB9DQp9DQoNCi8vINiq2KfYqNi524wg2qnZhyDZhdmI2YLYuSDaqdmE24zaqSDaqdin2LHYqNixINio2LEg2LHZiNuMINiq2Kgg2YfYp9uMINio2LnYryDYp9iyINiq2Kgg2YHYudmE24wg24zYpyDYqNixINix2YjbjCDYr9qp2YXZhyDYqNi52K/bjCDZgdix2KfYrtmI2KfZhtuMINmF24zYtNmI2K8NCmV4cG9ydCBjb25zdCBvblRhYk5leHQgPSBmdW5jdGlvbiAodGFiUGFuZWxOYW1lLCBjdXJyZW50VGFiUGFuZWxJdGVtTmFtZSwgdGFyZ2V0VGFiUGFuZWxJdGVtTmFtZSwgdmlld01vZGVsKSB7DQogICAgaWYgKHRhYlBhbmVsTmFtZSA9PT0gJ1lPVVJfT1dOX1RBQl9QQU5FTF9OQU1FJykgew0KICAgICAgICBpZiAodGFyZ2V0VGFiUGFuZWxJdGVtTmFtZSA9PT0gJ1lPVVJfT1dOX1RBQl9JVEVNX05BTUUnKSB7DQogICAgICAgICAgICAvLyAuLi4NCiAgICAgICAgfQ0KICAgIH0NCn0NCg0KLy8g2KrYp9io2LnbjCDaqdmHINmF2YjZgti5INqp2YTbjNqpINqp2KfYsdio2LEg2KjYsSDYsdmI24wg2KrYqCDZh9in24wg2YLYqNmEINin2LIg2KrYqCDZgdi52YTbjCDbjNinINio2LEg2LHZiNuMINiv2qnZhdmHINmC2KjZhNuMINmB2LHYp9iu2YjYp9mG24wg2YXbjNi02YjYrw0KZXhwb3J0IGNvbnN0IG9uVGFiUHJldmlvdXMgPSBmdW5jdGlvbiAodGFiUGFuZWxOYW1lLCBjdXJyZW50VGFiUGFuZWxJdGVtTmFtZSwgdGFyZ2V0VGFiUGFuZWxJdGVtTmFtZSwgdmlld01vZGVsKSB7DQogICAgaWYgKHRhYlBhbmVsTmFtZSA9PT0gJ1lPVVJfT1dOX1RBQl9QQU5FTF9OQU1FJykgew0KICAgICAgICBpZiAodGFyZ2V0VGFiUGFuZWxJdGVtTmFtZSA9PT0gJ1lPVVJfT1dOX1RBQl9JVEVNX05BTUUnKSB7DQogICAgICAgICAgICAvLyAuLi4NCiAgICAgICAgfQ0KICAgIH0NCn0NCg0KLy8g2KrYp9io2LnbjCDaqdmHINmF2YjZgti5INqp2YTbjNqpINix2YjbjCDZgduM2YTYryDYr9qp2YXZhyDZgdix2KfYrtmI2KfZhtuMINmF24zYtNmI2K8NCmV4cG9ydCBjb25zdCBvbkJ1dHRvbkZpZWxkQ2xpY2sgPSBmdW5jdGlvbiAoYnV0dG9uTmFtZSwgdmlld01vZGVsLCByZWNvcmQpIHsNCiAgICAvL9iq2YjYrNmHKioqINmF2KrYutuM2LEg2LHaqdmI2LHYryDYqtmG2YfYpyDYr9ix2LXZiNix2KrbjCDZvtin2LMg2K/Yp9iv2Ycg2YXbjNi02YjYryDaqdmHINin2LIg2LfYsduM2YIg2K/bjNiq2Kfar9ix24zYryDYs9in2Kgg2YHYsdmFINix2YjbjCDYr9qp2YXZhyDaqdmE24zaqSDYtNiv2Ycg2KjYp9i02K8qKioNCg0KICAgIGlmIChidXR0b25OYW1lID09PSAnWU9VUl9PV05fQlVUVE9OX05BTUUnKSB7DQogICAgICAgIC8vIHJldHVybiBmYWxzZSB0byBhdm9pZCBjb250aW51ZSBzdWJtaXQgb3BlcmF0aW9uDQogICAgICAgIHJldHVybiB0cnVlOw0KICAgIH0NCn0NCg0KLy8g2KrYp9io2LnbjCDaqdmHINmF2YjZgti5INiz2KfYrtiqINin2qnYtNmGINmF2LHYqNmI2Lcg2KjZhyDZhNuM2LPYqiDYsdqp2YjYsdivINmH2KfbjCDZgdix2YUg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25CdWlsZEFjdGlvbnMgPSBmdW5jdGlvbiAoY29udGFpbmVyLCBvcHRpb25zKSB7DQogICAgY29uc3QgZ3JpZEluc3RhbmNlID0gb3B0aW9ucy5jb21wb25lbnQ7Ly8g2K/Ys9iq2LHYs9uMINio2Ycg2LTYpiDar9ix24zYryDYp9i12YTbjCDYr9ixINi12YHYrdmHDQogICAgY29uc3QgcmVjb3JkID0gb3B0aW9ucy5kYXRhOy8vINiv2LPYqtix2LPbjCDYqNmHINiv24zYqtin24wg2LHaqdmI2LHYryDZgdi52YTbjA0KICAgIGNvbnN0IGFjdGlvbkVkaXQgPSBjb250YWluZXIuZmluZCgnLmdpLWVkaXQnKTsvLyDYr9iz2KrYsdiz24wg2KjZhyDYr9qp2YXZhyDZiNuM2LHYp9uM2LQg2qnZhyDYqtmI2LPYtyDYstuM2LHYs9in2K7YqiDYp9uM2KzYp9ivINmF24zYtNmI2K8NCiAgICBjb25zdCBhY3Rpb25EZWxldGUgPSBjb250YWluZXIuZmluZCgnLmdpLWRlbGV0ZScpOy8vINiv2LPYqtix2LPbjCDYqNmHINiv2qnZhdmHINit2LDZgSDaqdmHINiq2YjYs9i3INiy24zYsdiz2KfYrtiqINin24zYrNin2K8g2YXbjNi02YjYrw0KDQogICAgLy8g2YbZhdmI2YbZhyDaqdivINit2LDZgSDYr9qp2YXZhyDYrdiw2YEg2LLbjNix2LPYp9iu2KoNCiAgICAvL2FjdGlvbkRlbGV0ZS5yZW1vdmUoKTsNCg0KICAgIC8vINmG2YXZiNmG2Ycg2qnYryDYrdiw2YEg2K/aqdmF2Ycg2YjbjNix2KfbjNi0INiy24zYsdiz2KfYrtiqDQogICAgLy9hY3Rpb25FZGl0LnJlbW92ZSgpOw0KDQogICAgLy8g2YbZhdmI2YbZhyDaqdivINin2LbYp9mB2Ycg2qnYsdiv2YYg2Kfaqdi02YYg2YXZiNix2K8g2YbYuNixDQogICAgZGV2SGVscGVyLmFwcGVuZEdyaWRCdXR0b24oDQogICAgICAgIGNvbnRhaW5lciwNCiAgICAgICAgJ9i52YbZiNin2YYg2K/aqdmF2YcnLC8vINi52YbZiNin2YbbjCDaqdmHINmF2YjZgti5INmH2KfZiNixINi02K/ZhiDZhdin2YjYsyDYsdmI24wg2K/aqdmF2Ycg2LjYp9mH2LEg2K7ZiNin2YfYryDYtNivDQogICAgICAgICdmYSBmYS1wdXp6bGUtcGllY2UnLC8vINii24zaqdmI2YYg2YXZiNix2K8g2YbYuNixDQogICAgICAgIC8vIGh0dHBzOi8vd3d3Lnczc2Nob29scy5jb20vaWNvbnMvZm9udGF3ZXNvbWVfaWNvbnNfaW50cm8uYXNwDQogICAgICAgIC8vIGh0dHBzOi8vanMuZGV2ZXhwcmVzcy5jb20vRG9jdW1lbnRhdGlvbi9HdWlkZS9UaGVtZXNfYW5kX1N0eWxlcy9JY29ucy8NCiAgICAgICAgJyNFODQwMzMnLC8vINix2YbaryDZhdmI2LHYryDZhti42LENCiAgICAgICAgbnVsbCwNCiAgICAgICAgbnVsbCwNCiAgICAgICAgbnVsbCwNCiAgICAgICAgZnVuY3Rpb24gKCkgey8vINiq2KfYqNi524wg2qnZhyDYqNi52K8g2KfYsiDaqdmE24zaqSDYsdmI24wg2Kfaqdi02YYg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQogICAgICAgICAgICAvLyDZhtmF2YjZhtmHINqp2K8g2KrYp9iy2Ycg2LPYp9iy24wg2YXZhtio2Lkg2K/Yp9iv2Ycg2q/YsduM2K8g2LXZgdit2YcNCiAgICAgICAgICAgIC8vZ3JpZEluc3RhbmNlLmdldERhdGFTb3VyY2UoKS5yZWxvYWQoKTsNCg0KDQogICAgICAgICAgICAvLyDZhtmF2YjZhtmHINqp2K8g2KfYs9iq2YHYp9iv2Ycg2KfYsiDYr9in2K/ZhyDZh9in24wg2LHaqdmI2LHYr9uMINqp2Ycg2Kfaqdi02YYg2KLZhiDaqdmE24zaqSDYtNiv2YcNCiAgICAgICAgICAgIC8vIGFsZXJ0KHJlY29yZFsn2qnYryddKTsNCiAgICAgICAgICAgIC8vINiq2YjYrNmHOiDYqNix2KfbjCDYp9iz2KrZgdin2K/ZhyDYp9iyINiv2KfYr9mHINmH2KfbjCDYsdqp2YjYsdivINio2KfbjNivINin2LIg2KfYs9mFINmE24zYqNmEINii2YYg2KfYs9iq2YHYp9iv2Ycg2qnZhtuM2K8NCg0KDQogICAgICAgICAgICAvLyDZhtmF2YjZhtmHINqp2K8g2KfYs9iq2YHYp9iv2Ycg2KfYsiDYsNmE2LDZhyDZh9in24wg2LHaqdmI2LHYryDYqNinINin2LPYqtmB2KfYr9mHINin2LIgSW5kZXg6DQogICAgICAgICAgICAvLyBhbGVydChPYmplY3QudmFsdWVzKHJlY29yZClbMF0pOw0KICAgICAgICB9LA0KICAgICAgICBmYWxzZQ0KICAgICk7DQp9DQoNCi8v2KrYp9io2LnbjCDaqdmHINmC2KjZhCDYp9iyINiw2K7bjNix2Ycg2YHYsdmFINmB2LHYp9iu2YjYp9mG24wg2YXbjNi02YjYrw0KZXhwb3J0IGNvbnN0IGJlZm9yZVN1Ym1pdCA9IGZ1bmN0aW9uICh2aWV3TW9kZWwpIHsNCiAgICAvLyByZXR1cm4gZmFsc2UgdG8gYXZvaWQgY29udGludWUgc3VibWl0IG9wZXJhdGlvbg0KICAgIHJldHVybiB0cnVlOw0KfQ0KDQovL9iq2KfYqNi524wg2qnZhyDYqNi52K8g2KfYsiDYsNiu24zYsdmHINmB2LHZhSDYqNinINmF2YjZgdmC24zYqiDZgdix2KfYrtmI2KfZhtuMINmF24zYtNmI2K8NCmV4cG9ydCBjb25zdCBvblN1Ym1pdFN1Y2Nlc3MgPSBmdW5jdGlvbiAodmlld01vZGVsKSB7DQogICAgY29uc29sZS5sb2coJ1N1Ym1pdGluZyBkb25lIGZyb20gY3VzdG9tIGpzJyk7DQp9DQoNCi8v2KrYp9io2LnbjCDaqdmHINio2LnYryDYp9iyINin2YLYr9in2YUg2KjZhyDYsNiu24zYsdmHINmB2LHZhSDZiCDZhdmI2KfYrNmH2Ycg2KjYpyDYrti32Kcg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25TdWJtaXRGYWlsZWQgPSBmdW5jdGlvbiAodmlld01vZGVsLCBlcnJvcikgew0KICAgIGNvbnNvbGUuZXJyb3IoJ1N1Ym1pdGluZyBmYWlsZWQgZnJvbSBjdXN0b20ganMnLCBlcnJvcik7DQp9DQoNCi8v2KrYp9io2LnbjCDaqdmHINmC2KjZhCDYp9iyINiw2K7bjNix2Ycg2YHYsdmFINiq2Ygg2K/YsSDYqtmIINmB2LHYp9iu2YjYp9mG24wg2YXbjNi02YjYrw0KZXhwb3J0IGNvbnN0IGJlZm9yZVN1YlN1Ym1pdCA9IGZ1bmN0aW9uIChuYW1lLCByZWNvcmQsIHZpZXdNb2RlbCkgew0KICAgIC8vcmVjb3JkOiDYsdqp2YjYsdiv24wg2qnZhyDZgtix2KfYsSDYp9iz2Kog2LDYrtuM2LHZhyDYtNmI2K8g2YjZhNuMINmH2YbZiNiyINiw2K7bjNix2Ycg2YbYtNiv2Ycg2KfYs9iqDQogICAgLy9uYW1lIDog2YbYp9mFINiq2YjYr9ixINiq2YgNCiAgICAvLyByZXR1cm4gZmFsc2UgdG8gYXZvaWQgY29udGludWUgc3VibWl0IG9wZXJhdGlvbg0KICAgIC8vdHJ5IHsNCiAgICAvLyAgICBpZiAodmlld01vZGVsLkludm9pY2VfTGlzdCkNCiAgICAvLyAgICAgICAgc3dpdGNoIChuYW1lKSB7DQogICAgLy8gICAgICAgICAgICBjYXNlICdJbnZvaWNlJzoNCiAgICAvLyAgICAgICAgICAgICAgICBpZiAodmlld01vZGVsLkludm9pY2VfTGlzdCgpWzBdLkFtb3VudCgpICE9IHJlY29yZC5BbW91bnQgJiYgXy5maW5kSW5kZXgoa28udG9KUyh2aWV3TW9kZWwuSW52b2ljZV9MaXN0KCkpLCBbJ0lkJywgcmVjb3JkLklkXSkgPiAwKSB7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHJlY29yZC5CaWxsSWQgPSBudWxsOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICByZWNvcmQuUGF5SWQgPSBudWxsOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICBsZXQgcm93SW5kZXggPSBfLmZpbmRJbmRleChrby50b0pTKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKSksIFsnSWQnLCByZWNvcmQuSWRdKTsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgdmlld01vZGVsLkludm9pY2VfUmVjb3JkKCkuQmlsbElkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICB2aWV3TW9kZWwuSW52b2ljZV9SZWNvcmQoKS5QYXlJZChudWxsKTsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgdmlld01vZGVsLkludm9pY2VfTGlzdCgpW3Jvd0luZGV4XS5CaWxsSWQobnVsbCk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVtyb3dJbmRleF0uUGF5SWQobnVsbCk7DQogICAgLy8gICAgICAgICAgICAgICAgfQ0KICAgIC8vICAgICAgICAgICAgICAgIGJyZWFrOw0KICAgIC8vICAgICAgICB9DQogICAgLy99DQogICAgLy9jYXRjaCAoZXJyKSB7DQogICAgLy8gICAgY29uc29sZS5sb2coZXJyLm1lc3NhZ2UpOw0KICAgIC8vICAgIHRvYXN0LnNob3coJ2Vycm9yJywgZXJyLm1lc3NhZ2UpOw0KICAgIC8vfQ0KDQogICAgcmV0dXJuIHRydWU7DQp9DQoNCi8v2KrYp9io2LnbjCDaqdmHINio2LnYr9in2LIg2LDYrtuM2LHZhyDZgdix2YUg2KrZiCDYr9ixINiq2Ygg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25TdWJTdWJtaXQgPSBmdW5jdGlvbiAobmFtZSwgcmVjb3JkLCB2aWV3TW9kZWwpIHsNCg0KICAgIC8vKtiq2LDaqdixINiv2LEg2LXZiNix2Kog2YXYrtmB24wg2KjZiNiv2YYg2YHbjNmE2K8g2KfYsiDYs9mF2Kog2YHYsdin24zZhtivINmI24zZiCDZhdiv2YQg2KLZhiDZgduM2YTYryDZhdmI2KzZiNivINmG2YXbjCDYqNin2LTYryoNCg0KICAgIC8vcmVjb3JkIDrYsdqp2YjYsdiv24wg2qnZhyDYsNiu24zYsdmHINi02K/ZhyDYp9iz2KoNCiAgICAvL25hbWUgOiDZhtin2YUg2KrZiNiv2LEg2KrZiA0KICAgIC8vIGZvciBleGFtcGxlDQogICAgLy8gaWYgKG5hbWUgPT0gJ9mG2KfZhSDYqtmI2K/YsSDYqtmIJykNCiAgICAvLyAgICAgZHNveDMzMkRldGFpbFN1Ym1pdCgpOw0KDQogICAgLy90cnkgew0KICAgIC8vICAgIGlmICh2aWV3TW9kZWwuSW52b2ljZV9MaXN0KQ0KICAgIC8vICAgICAgICBzd2l0Y2ggKG5hbWUpIHsNCiAgICAvLyAgICAgICAgICAgIGNhc2UgJ0ludm9pY2UnOg0KICAgIC8vICAgICAgICAgICAgICAgIGlmICh2aWV3TW9kZWwuSW52b2ljZV9MaXN0KClbMF0uQW1vdW50KCkgIT0gcmVjb3JkLkFtb3VudCAmJiBfLmZpbmRJbmRleChrby50b0pTKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKSksIFsnSWQnLCByZWNvcmQuSWRdKSA+IDApIHsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgcmVjb3JkLkJpbGxJZCA9IG51bGw7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHJlY29yZC5QYXlJZCA9IG51bGw7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIGxldCByb3dJbmRleCA9IF8uZmluZEluZGV4KGtvLnRvSlModmlld01vZGVsLkludm9pY2VfTGlzdCgpKSwgWydJZCcsIHJlY29yZC5JZF0pOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICB2aWV3TW9kZWwuSW52b2ljZV9SZWNvcmQoKS5CaWxsSWQobnVsbCk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX1JlY29yZCgpLlBheUlkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICB2aWV3TW9kZWwuSW52b2ljZV9MaXN0KClbcm93SW5kZXhdLkJpbGxJZChudWxsKTsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgdmlld01vZGVsLkludm9pY2VfTGlzdCgpW3Jvd0luZGV4XS5QYXlJZChudWxsKTsNCiAgICAvLyAgICAgICAgICAgICAgICB9DQogICAgLy8gICAgICAgICAgICAgICAgYnJlYWs7DQogICAgLy8gICAgICAgIH0NCiAgICAvL30NCiAgICAvL2NhdGNoIChlcnIpIHsNCiAgICAvLyAgICBjb25zb2xlLmxvZyhlcnIubWVzc2FnZSk7DQogICAgLy8gICAgdG9hc3Quc2hvdygnZXJyb3InLCBlcnIubWVzc2FnZSk7DQogICAgLy99DQp9DQoNCi8v2KrYp9io2LnbjCDaqdmHINmC2KjZhCDYp9iyINit2LDZgSDYsdqp2YjYsdivINmH2KfbjCDZgdix2YUg2KrZiCDYr9ixINiq2Ygg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3QgYmVmb3JlU3ViRGVsZXRlID0gZnVuY3Rpb24gKG5hbWUsIHJlY29yZCwgdmlld01vZGVsKSB7DQogICAgLy8gcmV0dXJuIGZhbHNlIHRvIGF2b2lkIGNvbnRpbnVlIHN1Ym1pdCBvcGVyYXRpb24NCiAgICAvL3RyeSB7DQogICAgLy8gICAgaWYgKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QpDQogICAgLy8gICAgICAgIHN3aXRjaCAobmFtZSkgew0KICAgIC8vICAgICAgICAgICAgY2FzZSAnSW52b2ljZSc6DQogICAgLy8gICAgICAgICAgICAgICAgaWYgKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVswXS5BbW91bnQoKSAhPSByZWNvcmQuQW1vdW50ICYmIF8uZmluZEluZGV4KGtvLnRvSlModmlld01vZGVsLkludm9pY2VfTGlzdCgpKSwgWydJZCcsIHJlY29yZC5JZF0pID4gMCkgew0KICAgIC8vICAgICAgICAgICAgICAgICAgICByZWNvcmQuQmlsbElkID0gbnVsbDsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgcmVjb3JkLlBheUlkID0gbnVsbDsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgbGV0IHJvd0luZGV4ID0gXy5maW5kSW5kZXgoa28udG9KUyh2aWV3TW9kZWwuSW52b2ljZV9MaXN0KCkpLCBbJ0lkJywgcmVjb3JkLklkXSk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX1JlY29yZCgpLkJpbGxJZChudWxsKTsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgdmlld01vZGVsLkludm9pY2VfUmVjb3JkKCkuUGF5SWQobnVsbCk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVtyb3dJbmRleF0uQmlsbElkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICB2aWV3TW9kZWwuSW52b2ljZV9MaXN0KClbcm93SW5kZXhdLlBheUlkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgIH0NCiAgICAvLyAgICAgICAgICAgICAgICBicmVhazsNCiAgICAvLyAgICAgICAgfQ0KICAgIC8vfQ0KICAgIC8vY2F0Y2ggKGVycikgew0KICAgIC8vICAgIGNvbnNvbGUubG9nKGVyci5tZXNzYWdlKTsNCiAgICAvLyAgICB0b2FzdC5zaG93KCdlcnJvcicsIGVyci5tZXNzYWdlKTsNCiAgICAvL30NCiAgICByZXR1cm4gdHJ1ZTsNCn0NCg0KLy/Yqtin2KjYuduMINio2LnYryDYp9iyINit2LDZgSDYsdqp2YjYsdivINmH2KfbjCDZgdix2YUg2KrZiCDYr9ixINiq2Ygg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25TdWJEZWxldGUgPSBmdW5jdGlvbiAobmFtZSwgcmVjb3JkLCB2aWV3TW9kZWwpIHsNCiAgICAvL3RyeSB7DQogICAgLy8gICAgaWYgKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QpDQogICAgLy8gICAgICAgIHN3aXRjaCAobmFtZSkgew0KICAgIC8vICAgICAgICAgICAgY2FzZSAnSW52b2ljZSc6DQogICAgLy8gICAgICAgICAgICAgICAgaWYgKHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVswXS5BbW91bnQoKSAhPSByZWNvcmQuQW1vdW50ICYmIF8uZmluZEluZGV4KGtvLnRvSlModmlld01vZGVsLkludm9pY2VfTGlzdCgpKSwgWydJZCcsIHJlY29yZC5JZF0pID4gMCkgew0KICAgIC8vICAgICAgICAgICAgICAgICAgICByZWNvcmQuQmlsbElkID0gbnVsbDsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgcmVjb3JkLlBheUlkID0gbnVsbDsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgbGV0IHJvd0luZGV4ID0gXy5maW5kSW5kZXgoa28udG9KUyh2aWV3TW9kZWwuSW52b2ljZV9MaXN0KCkpLCBbJ0lkJywgcmVjb3JkLklkXSk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX1JlY29yZCgpLkJpbGxJZChudWxsKTsNCiAgICAvLyAgICAgICAgICAgICAgICAgICAgdmlld01vZGVsLkludm9pY2VfUmVjb3JkKCkuUGF5SWQobnVsbCk7DQogICAgLy8gICAgICAgICAgICAgICAgICAgIHZpZXdNb2RlbC5JbnZvaWNlX0xpc3QoKVtyb3dJbmRleF0uQmlsbElkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgICAgICB2aWV3TW9kZWwuSW52b2ljZV9MaXN0KClbcm93SW5kZXhdLlBheUlkKG51bGwpOw0KICAgIC8vICAgICAgICAgICAgICAgIH0NCiAgICAvLyAgICAgICAgICAgICAgICBicmVhazsNCiAgICAvLyAgICAgICAgfQ0KICAgIC8vfQ0KICAgIC8vY2F0Y2ggKGVycikgew0KICAgIC8vICAgIGNvbnNvbGUubG9nKGVyci5tZXNzYWdlKTsNCiAgICAvLyAgICB0b2FzdC5zaG93KCdlcnJvcicsIGVyci5tZXNzYWdlKTsNCiAgICAvL30NCn0NCg0KLy/Yqtin2KjYuduMINqp2YfYqNi52K8g2KfYsiDYp9mG2KrYrtin2Kgg2q/YstuM2YbZhyDYrNiv24zYryDYqtmI2K/Ysdiq2Ygg2YggINmG2YXYp9uM2LQg2YXYr9in2YQg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25Mb2FkU3ViRm9ybSA9IGZ1bmN0aW9uIChuYW1lLCByZWNvcmQsIHZpZXdNb2RlbCkgew0KICAgIGRlYnVnZ2VyOw0KfQ0KDQovL9iq2KfYqNi524wg2qnZh9io2LnYryDYp9iyINin2YbYqtiu2KfYqCDar9iy24zZhtmHINmI24zYsdin24zYtCDYqtmI2K/Ysdiq2Ygg2YggINmG2YXYp9uM2LQg2YXYr9in2YQg2YHYsdin2K7ZiNin2YbbjCDZhduM2LTZiNivDQpleHBvcnQgY29uc3Qgb25Mb2FkRWRpdFN1YkZvcm0gPSBmdW5jdGlvbiAobmFtZSwgcmVjb3JkLCB2aWV3TW9kZWwpIHsNCiAgICBkZWJ1Z2dlcjsNCn0NCg0KLy/Yp9uM2YYg2KrYp9io2Lkg2LLZhdin2YbbjCDZgdix2K7ZiNin2YbbjCDZhduMINi02YjYryDaqdmHINin2qnYtNmGINit2LDZgSDYr9ixINmE24zYs9iqINiv2KfYr9mHINmH2KfbjCDZgdix2YUg2YHYsdiu2YjYp9mG24wg2LTYr9mHINio2KfYtNivINqp2Ycg2KjYsdin2LPYp9izINii2YYg2YXbjCDYqtmI2KfZhiDYsdqp2YjYsdivINmF2YjYsdiv2YbYuNixINix2Kcg2K3YsNmBINqp2LHYryDbjNinINin2KzYp9iy2Ycg2K3YsNmBINio2Ycg2KfbjNmGINix2qnZiNix2K8g2YbYr9in2K8NCmV4cG9ydCBjb25zdCBvbkJlZm9yZURlbGV0ZSA9IGZ1bmN0aW9uIChjb250YWluZXIsIG9wdGlvbnMpIHsNCiAgICBjb25zdCByZWNvcmQgPSBvcHRpb25zLmRhdGE7Ly8g2K/Ys9iq2LHYs9uMINio2Ycg2K/bjNiq2KfbjCDYsdqp2YjYsdivINmB2LnZhNuMDQoNCiAgICByZXR1cm4gdHJ1ZQ0KfQ==) نیز داخل Package قرار دارد. این Sample شامل Hookها و Commentهای مرجع پیشرفته است و به‌عنوان پیوست در انتهای Article نگه داشته شده تا جریان اصلی آموزش را قطع نکند.

```
//متغیر سراسری جهت انتقال به آدرس جدید
export let redirectUrl = '';

//تابعی که در زمان لود فرم فراخوانی می‌شود
export const onLoad = function (viewModel) {
    debugger;
    //*تذکر در صورت مخفی بودن فیلد از سمت فرایند ویو مدل آن فیلد موجود نمی باشد*

    //نمونه کد استفاده از نمودار
    //جهت استفاده از نمودار حتما در بالا ماژول نمودار ایمپورت شود
    //const chartConfig = {
    //    element: document.getElementById('tstChart'),
    //    exportId: 368,
    //    showZoomTool: false,//تنظیم نمایش/عدم نمایش ابزار نمایش پنجره ای
    //    showFilterTool: false,//تنظیم نمایش/عدم نمایش ابزار نمایش پنجره تنظیم فیلتر ها
    //    showRemoveFilterTool: true,//تنظیم نمایش/عدم نمایش ابزار حذف فیلتر ها
    //    showRefreshTool: false,//تنظیم نمایش/عدم نمایش ابزار تازه سازی نمودار
    //    paramList: [{
    //        ResourceId: 249, Parameters: [
    //            { Name: 'SellYear', Value: 1399 }
    //            , { Name: 'SellMonth', Value: null }
    //            , { Name: 'SellGoodsCount', Value: null }
    //        ]
    //    }]
    //}

    //برای دریافت اینستنس یکهفیلد بصورت مستقیم
    //devHelper.getWidgetInstance($("#changepNosaziCodeBtn"))

    //viewModel.data().Location.subscribe(function (newValue) {
    //    alert("The person's new name is " + newValue);
    //});

    //----------------------------------------مقدار پیش فرض برای فایل آپلود
    //if (global.getQueryStringParam("recId") == null || global.getQueryStringParam("recId") == undefined) {
    //    if (viewModel.data().FileUpload != undefined) {

    //        viewModel.data().FileUpload(
    //            [{
    //                ArchiveCode: ko.observable("B6830750-4212-4128-BF4B-5A4502F95987"),
    //                IsDeleted: ko.observable(false),
    //                Kind: ko.observable(1)
    //            }]
    //        )
    //    }
    //    else {
    //        viewModel.data().FileUpload = ko.observable(
    //            [{
    //                ArchiveCode: ko.observable("B6830750-4212-4128-BF4B-5A4502F95987"),
    //                IsDeleted: ko.observable(false),
    //                Kind: ko.observable(1)
    //            }]
    //        )

    //    }
    //}
    //----------------------------------------

    //نمونه کد استفاده از ایجکس
    //*نکته مهم : بعد از دریافت اطلاعات از ایجکس حتما از try استفاده نمایید
    //$.ajax({
    //    url: "/ControllerName/ActionName",
    //    type: "POST",
    //    data: ko.toJSON({ data:data }),
    //    timeout: ajaxTimeOut,
    //    contentType: "application/json; charset=utf-8",
    //    beforeSend: (xhr) => {
    //        loadPanel.indicatorShow();
    //    }
    //}).done((result) => {
    //    if (result.HasError) {
    //        toast.show('error', result.Message);
    //    }
    //    else {
    //        try {
    //            //..
    //        }
    //        catch (err) {
    //            console.log(err.message);
    //            loadPanel.indicatorHide();
    //            toast.show('error', 'متن خطا');
    //        }
    //    }
    //}
    //    ).fail((errorThrown) => {
    //        toast.show('error', 'متن خطا');
    //    }).always(() => {
    //        loadPanel.indicatorHide();
    //    });

    //**تذکر بسیار مهم در توابع خود تا حد امکان از try استفاده نمایید.درصورت بروز خطا در جی اس شما ممکن است عملیات فرم ساز مختل شود**
    //**در done ajax خود حتما از try استفاده نمایید**
    //try {
    //    //...
    //}
    //catch (err) {
    //    console.log(err.message);
    //    loadPanel.indicatorHide();
    //    toast.show('error', 'خطا ');
    //}

    //*تذکر:دکمه ذخیره سازی فرم در فرم ساز و دکمه ذخیره سازی فرم در فرایند متفاوت می باشند*
    //بطور مثال در صورت مخفی کردن دکمه ذخیره فرم ساز  این دکمه در فرایند نمایش داده می‌شود

    //شامل تمامی تنظیمات و مقادیر فیلد های موجود در فرم
    //viewModel

    //جهت انتخاب کردن مقادیر فیلد ها
    //viewModel.data().fieldName()

    //جهت انتخاب کردن خود فیلد و اعمال تغییرات
    //viewModel.fields.fieldNameConfig //تنظیمات فیلد جهت اعمال تغییرات بر روی خود فیلد
    //viewModel.fields.fieldName_Instance // اینستنس فیلد جهت اعمال تغییرات بر روی خود فیلد
    //viewModel.fields.fieldName_Validation //تنیظیمات ولیدیشن فیلد جهت تغییر و یا مشاهده ولیدیشن های ست شده بر روی فیلد
    //viewModel.fields.fieldName_Visibility //تنظیمات نمایش فیلد

    //نمونه کد تغییر دادن تنظیمات فیلد ها در فرم ساز
    //viewModel.fields.fieldName_Instance.option('disabled', true)

    //نمونه کد جهت انجام تغییر مستقیم بر روی فیلد های فرم ساز
    //document.querySelector('[data-bind="dxTextArea: fields.fieldNameConfig , dxValidator: fields.fieldName_Validation"]').style.height = "150px"

    //جهت نمایش loader
    //loadPanel.indicatorShow();
    //جهت مخفی کردن loader
    ////loadPanel.indicatorHide();

    //جهت گرفتن entityMode فرایند - مود وارد کردن اطلاعات در فرم
    //let entityMode = new URLSearchParams(window.location.search).get('entityMode');

    //نمونه کد جهت افزودن ستون اکشن به جدول تودرتو
    //devHelper.getWidgetInstance($('#نام تودرتو')).addColumn({
    //    width: 150,
    //    caption: "چاپ",
    //    fixed: true,
    //    fixedPosition: 'left',
    //    alignment: 'center',
    //    cellTemplate: function (container, options) {
    //        let flag = false;//!checkPaid(viewModel, options.rowIndex);
    //        $("<div />").dxButton({
    //            stylingMode: "contained",
    //            //text: "چاپ",
    //            icon: "fa fa-print",
    //            disabled: flag,
    //            onClick: () => {
    //                  جهت استفاده از قالب چاپ
    //                var paramList = [{
    //                    ResourceId: 1, Parameters: [{ Name: 'RecordId', Value: options.row.key }]
    //                }
    //                ]
    //                absoluteMethodHelper.generateStimulExport(2, paramList, 'pdf');
    //            }
    //        }).appendTo(container);
    //    }
    //});

    //نمونه کد جهت استفاده از قالب چاپ
    //                var paramList = [{
    //                    ResourceId: 1, Parameters: [{ Name: 'RecordId', Value: options.row.key }]
    //                }
    //                ]
    //                absoluteMethodHelper.generateStimulExport(2, paramList, 'pdf');

    //جهت نمایش پیغام به کاربر
    //toast.show('error', result.Message);

    //نمونه کد جهت دریافت داده های فرم تودرتو
    //viewModel.Invoice_List()[rowIndex].BillId();

    //*تذکر مهم : در صورت تغییر مقادیر جدول تو در تو دو حالت وجود دارد*
    //*حالت اول رکورد از قبل در دیتا بیس وجود دارد که در این صورت کاربر می بایست دو خط زیر را اجرا نماید
    //viewModel.Invoice_List()[rowIndex][FormBuilderStatics.RecordState](RecordStateType.edit);
    //viewModel.Invoice_Record()[FormBuilderStatics.RecordState](RecordStateType.edit);
    //*حالت دوم رکورد هنوز در پایگاه داده ذخیره نشده است که در این صورت نباید دو خط بالا را اجرا کرد
    //به این خاطر که رکورد حالت ویرایش گرفته و در پایگاه داده ذخیره نمیشد*

    //جهت افزودن mask بر روی فیلد ها
    //viewModel.fields.fieldName_Instance.option('mask', "\\0\\9000000000");
    //viewModel.fields.fieldName_Instance.option('maskInvalidMessage', "شماره همراه وارد شده صحیح نمی باشد");
    //viewModel.fields.fieldName_Instance.option('inputAttr', { style: 'direction: ltr;' });
    //viewModel.fields.fieldName_Instance.option('useMaskedValue', true);

    //در صورت استفاده از ادیتور اچ تی ام ال و قراردادن دکمه برای بایند کردن از روش زیر استفاده نمایید
    //if (viewModel.fields.printBtn_HTMLConfig && $("#printBtn")) {
    //$("#printBtn").dxButton({
    //    stylingMode: "contained",
    //    type: "default",
    //    icon: "print",
    //    text: "چاپ",
    //    disabled: viewModel.fields.printBtn_HTMLConfig.disabled,
    //    onClick: () => {
    //        //debugger;
    //        var paramList = [{
    //            ResourceId: 3, Parameters: [{ Name: 'recordId', Value: viewModel.data().Id() }]
    //        },
    //        { ResourceId: 4, Parameters: [{ Name: 'recordId', Value: viewModel.data().Id() }] },
    //        { ResourceId: 5, Parameters: [{ Name: 'recordId', Value: viewModel.data().Id() }] }
    //        ]
    //        absoluteMethodHelper.generateStimulExport(6, paramList, 'pdf');
    //    }
    //});

    //نمونه کد کار با فیلد نقشه

    //onLoadMap.subscribe((value) => {
    //    //دقت فرمایید که فرمت موقعیت مورد نیاز باید به شکل زیر باشد
    //    let geoJson = '{\"type\":\"FeatureCollection\",\"features\":[{\"type\":\"Feature\",\"geometry\":{\"type\":\"Polygon\",\"coordinates\":[[[59.524908956873844,36.35135654351753],[59.563562592024056,36.35190956400033],[59.56648308890207,36.33573209193248],[59.545352435019936,36.32508354600387],[59.517006435909785,36.323008984571636],[59.50498086052975,36.33338123944986],[59.501201393981695,36.35246258055471],[59.524908956873844,36.35135654351753]]]},\"properties\":{}}]}';
    //    switch (value.fieldName) {
    //        case "GpsField1"://GpsField1 نام فیلد تست می باشد
    //            //جهت ذخیره سازی موقعیت در ویو مدل باید از کد زیر استفاده نمود.توجه فرمایید که نام فیلد خود را وارد نمایید
    //            viewModel.data().GpsField1(geoJson);//GpsField1 نام فیلد تست می باشد
    //            //هر فیلد نقشه دارای تگی مخفی می باشد که در صورت انتخاب موقعیت مقدار موقعیت درون آن قرار می گیرد و در هنگام ذخیره سازی به سمت سرور ارسال می شود به همین خاطر قطعه کد زیر نیز جهت ذخیره سازی موقعیت مورد نظر الزمی می باشد
    //            $('input[name="GpsField1"]').val(geoJson);//GpsField1 نام فیلد تست می باشد
    //            //بدست آوردن اندیس فیلد نقشه بر اساس نام فیلد در لیست نقشه ها
    //            selectedMapIndex = _.findIndex(mapList, (obj) => {
    //                return obj.options.elementId === "GpsField1";//GpsField1 نام فیلد تست می باشد
    //            });
    //            //جهت رسم نقشه ، دقت فرمایید قطعه کد زیر فقط جهت رسم موقعیت در نقشه می باشد و در هنگام کلیک بر روی ذخیره فرم اطلاعات موقعیت را به سمت سرور ارسال نمی نماید.برای ذخیره موقعیت باید از قطعه کد های بالا استفاده نمود.
    //            GraphMaps.MapManager.drawLayerOnMap(geoJson);

    //            break;
    //        case "GpsField2"://GpsField2 نام فیلد تست می باشد
    //            //جهت ذخیره سازی موقعیت در ویو مدل باید از کد زیر استفاده نمود.توجه فرمایید که نام فیلد خود را وارد نمایید
    //            viewModel.data().GpsField2(geoJson);//GpsField2 نام فیلد تست می باشد
    //            //هر فیلد نقشه دارای تگی مخفی می باشد که در صورت انتخاب موقعیت مقدار موقعیت درون آن قرار می گیرد و در هنگام ذخیره سازی به سمت سرور ارسال می شود به همین خاطر قطعه کد زیر نیز جهت ذخیره سازی موقعیت مورد نظر الزمی می باشد
    //            $('input[name="GpsField2"]').val(geoJson);//GpsField2 نام فیلد تست می باشد
    //            //بدست آوردن اندیس فیلد نقشه بر اساس نام فیلد در لیست نقشه ها
    //            selectedMapIndex = _.findIndex(mapList, (obj) => {
    //                return obj.options.elementId === "GpsField2";//GpsField2 نام فیلد تست می باشد
    //            });
    //            //جهت رسم نقشه ، دقت فرمایید قطعه کد زیر فقط جهت رسم موقعیت در نقشه می باشد و در هنگام کلیک بر روی ذخیره فرم اطلاعات موقعیت را به سمت سرور ارسال نمی نماید.برای ذخیره موقعیت باید از قطعه کد های بالا استفاده نمود.
    //            GraphMaps.MapManager.drawLayerOnMap(geoJson);
    //            break;
    //    }
    //});

    //onMapClick.subscribe((value) => {
    //    switch (value.fieldName) {
    //        case "GpsField1"://GpsField1 نام فیلد تست می باشد
    //            //جهت ذخیره سازی موقعیت در ویو مدل باید از کد زیر استفاده نمود.توجه فرمایید که نام فیلد خود را وارد نمایید
    //            viewModel.data().GpsField2(mapList[value.mapIndex].koGpsValue());//GpsField2 نام فیلد تست می باشد
    //            //هر فیلد نقشه دارای تگی مخفی می باشد که در صورت انتخاب موقعیت مقدار موقعیت درون آن قرار می گیرد و در هنگام ذخیره سازی به سمت سرور ارسال می شود به همین خاطر قطعه کد زیر نیز جهت ذخیره سازی موقعیت مورد نظر الزمی می باشد
    //            $('input[name="GpsField2"]').val(mapList[value.mapIndex].koGpsValue());//GpsField2 نام فیلد تست می باشد
    //            //بدست آوردن اندیس فیلد نقشه بر اساس نام فیلد در لیست نقشه ها
    //            selectedMapIndex = _.findIndex(mapList, (obj) => {
    //                return obj.options.elementId === "GpsField2";//GpsField2 نام فیلد تست می باشد
    //            });
    //            //جهت رسم نقشه ، دقت فرمایید قطعه کد زیر فقط جهت رسم موقعیت در نقشه می باشد و در هنگام کلیک بر روی ذخیره فرم اطلاعات موقعیت را به سمت سرور ارسال نمی نماید.برای ذخیره موقعیت باید از قطعه کد های بالا استفاده نمود.
    //            GraphMaps.MapManager.drawLayerOnMap(mapList[value.mapIndex].koGpsValue());
    //            break;
    //        case "GpsField2"://GpsField2 نام فیلد تست می باشد
    //            //جهت ذخیره سازی موقعیت در ویو مدل باید از کد زیر استفاده نمود.توجه فرمایید که نام فیلد خود را وارد نمایید
    //            viewModel.data().GpsField1(mapList[value.mapIndex].koGpsValue());//GpsField1 نام فیلد تست می باشد
    //            //هر فیلد نقشه دارای تگی مخفی می باشد که در صورت انتخاب موقعیت مقدار موقعیت درون آن قرار می گیرد و در هنگام ذخیره سازی به سمت سرور ارسال می شود به همین خاطر قطعه کد زیر نیز جهت ذخیره سازی موقعیت مورد نظر الزمی می باشد
    //            $('input[name="GpsField1"]').val(mapList[value.mapIndex].koGpsValue());//GpsField1 نام فیلد تست می باشد
    //            //بدست آوردن اندیس فیلد نقشه بر اساس نام فیلد در لیست نقشه ها
    //            selectedMapIndex = _.findIndex(mapList, (obj) => {
    //                return obj.options.elementId === "GpsField1";//GpsField1 نام فیلد تست می باشد
    //            });

    //            //جهت رسم نقشه ، دقت فرمایید قطعه کد زیر فقط جهت رسم موقعیت در نقشه می باشد و در هنگام کلیک بر روی ذخیره فرم اطلاعات موقعیت را به سمت سرور ارسال نمی نماید.برای ذخیره موقعیت باید از قطعه کد های بالا استفاده نمود.
    //            GraphMaps.MapManager.drawLayerOnMap(mapList[value.mapIndex].koGpsValue());
    //            break;
    //    }
    //});

    //$('#btnConvert').dxButton({
    //    text: 'Convert',
    //    onClick: () => {
    //        //دقت فرمایید که فرمت موقعیت مورد نیاز باید به شکل زیر باشد
    //        let geoJson = '{\"type\":\"FeatureCollection\",\"features\":[{\"type\":\"Feature\",\"geometry\":{\"type\":\"Polygon\",\"coordinates\":[[[59.524908956873844,36.35135654351753],[59.563562592024056,36.35190956400033],[59.56648308890207,36.33573209193248],[59.545352435019936,36.32508354600387],[59.517006435909785,36.323008984571636],[59.50498086052975,36.33338123944986],[59.501201393981695,36.35246258055471],[59.524908956873844,36.35135654351753]]]},\"properties\":{}}]}';
    //        //جهت ذخیره سازی موقعیت در ویو مدل باید از کد زیر استفاده نمود.توجه فرمایید که نام فیلد خود را وارد نمایید
    //        viewModel.data().GpsField1(geoJson);//GpsField1 نام فیلد تست می باشد
    //        //هر فیلد نقشه دارای تگی مخفی می باشد که در صورت انتخاب موقعیت مقدار موقعیت درون آن قرار می گیرد و در هنگام ذخیره سازی به سمت سرور ارسال می شود به همین خاطر قطعه کد زیر نیز جهت ذخیره سازی موقعیت مورد نظر الزمی می باشد
    //        $('input[name="GpsField1"]').val(geoJson);//GpsField1 نام فیلد تست می باشد
    //        //بدست آوردن اندیس فیلد نقشه بر اساس نام فیلد در لیست نقشه ها
    //        selectedMapIndex = _.findIndex(mapList, (obj) => {
    //            return obj.options.elementId === "GpsField1";
    //        });
    //        //جهت رسم نقشه ، دقت فرمایید قطعه کد زیر فقط جهت رسم موقعیت در نقشه می باشد و در هنگام کلیک بر روی ذخیره فرم اطلاعات موقعیت را به سمت سرور ارسال نمی نماید.برای ذخیره موقعیت باید از قطعه کد های بالا استفاده نمود.
    //        GraphMaps.MapManager.drawLayerOnMap(geoJson);
    //    }
    //});

    // console.log(viewModel.getParam('PARAM_NAME'));// دریافت پارامتر ارسال شده به فرم

    //-------------------------------------------------
    console.log('hello from custom js');
    console.log(ko.mapping.toJS(viewModel.data));
}

//تابعی که در زمان آماده شدن فرم فراخوانی می‌شود
export const onReady = function (viewModel) {
    // ...
}

// تابعی که موقع نمایش تب آیتم فراخوانی می‌شود
export const onTabLoad = function (tabPanelName, tabPanelItemName, viewModel) {
    if (tabPanelName === 'YOUR_OWN_TAB_PANEL_NAME') {
        if (tabPanelItemName === 'YOUR_OWN_TAB_ITEM_NAME') {
            // ...
        }
    }
}

// تابعی که موقع کلیک کاربر بر روی تب های بعد از تب فعلی یا بر روی دکمه بعدی فراخوانی می‌شود
export const onTabNext = function (tabPanelName, currentTabPanelItemName, targetTabPanelItemName, viewModel) {
    if (tabPanelName === 'YOUR_OWN_TAB_PANEL_NAME') {
        if (targetTabPanelItemName === 'YOUR_OWN_TAB_ITEM_NAME') {
            // ...
        }
    }
}

// تابعی که موقع کلیک کاربر بر روی تب های قبل از تب فعلی یا بر روی دکمه قبلی فراخوانی می‌شود
export const onTabPrevious = function (tabPanelName, currentTabPanelItemName, targetTabPanelItemName, viewModel) {
    if (tabPanelName === 'YOUR_OWN_TAB_PANEL_NAME') {
        if (targetTabPanelItemName === 'YOUR_OWN_TAB_ITEM_NAME') {
            // ...
        }
    }
}

// تابعی که موقع کلیک روی فیلد دکمه فراخوانی می‌شود
export const onButtonFieldClick = function (buttonName, viewModel, record) {
    //توجه*** متغیر رکورد تنها درصورتی پاس داده می‌شود که از طریق دیتاگرید ساب فرم روی دکمه کلیک شده باشد***

    if (buttonName === 'YOUR_OWN_BUTTON_NAME') {
        // return false to avoid continue submit operation
        return true;
    }
}

// تابعی که موقع ساخت اکشن مربوط به لیست رکورد های فرم فراخوانی می‌شود
export const onBuildActions = function (container, options) {
    const gridInstance = options.component;// دسترسی به شئ گرید اصلی در صفحه
    const record = options.data;// دسترسی به دیتای رکورد فعلی
    const actionEdit = container.find('.gi-edit');// دسترسی به دکمه ویرایش که توسط زیرساخت ایجاد می‌شود
    const actionDelete = container.find('.gi-delete');// دسترسی به دکمه حذف که توسط زیرساخت ایجاد می‌شود

    // نمونه کد حذف دکمه حذف زیرساخت
    //actionDelete.remove();

    // نمونه کد حذف دکمه ویرایش زیرساخت
    //actionEdit.remove();

    // نمونه کد اضافه کردن اکشن مورد نظر
    devHelper.appendGridButton(
        container,
        'عنوان دکمه',// عنوانی که موقع هاور شدن ماوس روی دکمه ظاهر خواهد شد
        'fa fa-puzzle-piece',// آیکون مورد نظر
        // https://www.w3schools.com/icons/fontawesome_icons_intro.asp
        // https://js.devexpress.com/Documentation/Guide/Themes_and_Styles/Icons/
        '#E84033',// رنگ مورد نظر
        null,
        null,
        null,
        function () {// تابعی که بعد از کلیک روی اکشن فراخوانی می‌شود
            // نمونه کد تازه سازی منبع داده گرید صفحه
            //gridInstance.getDataSource().reload();


            // نمونه کد استفاده از داده های رکوردی که اکشن آن کلیک شده
            // alert(record['کد']);
            // توجه: برای استفاده از داده های رکورد باید از اسم لیبل آن استفاده کنید


            // نمونه کد استفاده از ذلذه های رکورد با استفاده از Index:
            // alert(Object.values(record)[0]);
        },
        false
    );
}

//تابعی که قبل از ذخیره فرم فراخوانی می‌شود
export const beforeSubmit = function (viewModel) {
    // return false to avoid continue submit operation
    return true;
}

//تابعی که بعد از ذخیره فرم با موفقیت فراخوانی می‌شود
export const onSubmitSuccess = function (viewModel) {
    console.log('Submiting done from custom js');
}

//تابعی که بعد از اقدام به ذخیره فرم و مواجهه با خطا فراخوانی می‌شود
export const onSubmitFailed = function (viewModel, error) {
    console.error('Submiting failed from custom js', error);
}

//تابعی که قبل از ذخیره فرم تو در تو فراخوانی می‌شود
export const beforeSubSubmit = function (name, record, viewModel) {
    //record: رکوردی که قرار است ذخیره شود ولی هنوز ذخیره نشده است
    //name : نام تودر تو
    // return false to avoid continue submit operation
    //try {
    //    if (viewModel.Invoice_List)
    //        switch (name) {
    //            case 'Invoice':
    //                if (viewModel.Invoice_List()[0].Amount() != record.Amount && _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]) > 0) {
    //                    record.BillId = null;
    //                    record.PayId = null;
    //                    let rowIndex = _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]);
    //                    viewModel.Invoice_Record().BillId(null);
    //                    viewModel.Invoice_Record().PayId(null);
    //                    viewModel.Invoice_List()[rowIndex].BillId(null);
    //                    viewModel.Invoice_List()[rowIndex].PayId(null);
    //                }
    //                break;
    //        }
    //}
    //catch (err) {
    //    console.log(err.message);
    //    toast.show('error', err.message);
    //}

    return true;
}

//تابعی که بعداز ذخیره فرم تو در تو فراخوانی می‌شود
export const onSubSubmit = function (name, record, viewModel) {

    //*تذکر در صورت مخفی بودن فیلد از سمت فرایند ویو مدل آن فیلد موجود نمی باشد*

    //record :رکوردی که ذخیره شده است
    //name : نام تودر تو
    // for example
    // if (name == 'نام تودر تو')
    //     dsox332DetailSubmit();

    //try {
    //    if (viewModel.Invoice_List)
    //        switch (name) {
    //            case 'Invoice':
    //                if (viewModel.Invoice_List()[0].Amount() != record.Amount && _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]) > 0) {
    //                    record.BillId = null;
    //                    record.PayId = null;
    //                    let rowIndex = _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]);
    //                    viewModel.Invoice_Record().BillId(null);
    //                    viewModel.Invoice_Record().PayId(null);
    //                    viewModel.Invoice_List()[rowIndex].BillId(null);
    //                    viewModel.Invoice_List()[rowIndex].PayId(null);
    //                }
    //                break;
    //        }
    //}
    //catch (err) {
    //    console.log(err.message);
    //    toast.show('error', err.message);
    //}
}

//تابعی که قبل از حذف رکورد های فرم تو در تو فراخوانی می‌شود
export const beforeSubDelete = function (name, record, viewModel) {
    // return false to avoid continue submit operation
    //try {
    //    if (viewModel.Invoice_List)
    //        switch (name) {
    //            case 'Invoice':
    //                if (viewModel.Invoice_List()[0].Amount() != record.Amount && _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]) > 0) {
    //                    record.BillId = null;
    //                    record.PayId = null;
    //                    let rowIndex = _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]);
    //                    viewModel.Invoice_Record().BillId(null);
    //                    viewModel.Invoice_Record().PayId(null);
    //                    viewModel.Invoice_List()[rowIndex].BillId(null);
    //                    viewModel.Invoice_List()[rowIndex].PayId(null);
    //                }
    //                break;
    //        }
    //}
    //catch (err) {
    //    console.log(err.message);
    //    toast.show('error', err.message);
    //}
    return true;
}

//تابعی بعد از حذف رکورد های فرم تو در تو فراخوانی می‌شود
export const onSubDelete = function (name, record, viewModel) {
    //try {
    //    if (viewModel.Invoice_List)
    //        switch (name) {
    //            case 'Invoice':
    //                if (viewModel.Invoice_List()[0].Amount() != record.Amount && _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]) > 0) {
    //                    record.BillId = null;
    //                    record.PayId = null;
    //                    let rowIndex = _.findIndex(ko.toJS(viewModel.Invoice_List()), ['Id', record.Id]);
    //                    viewModel.Invoice_Record().BillId(null);
    //                    viewModel.Invoice_Record().PayId(null);
    //                    viewModel.Invoice_List()[rowIndex].BillId(null);
    //                    viewModel.Invoice_List()[rowIndex].PayId(null);
    //                }
    //                break;
    //        }
    //}
    //catch (err) {
    //    console.log(err.message);
    //    toast.show('error', err.message);
    //}
}

//تابعی کهبعد از انتخاب گزینه جدید تودرتو و  نمایش مدال فراخوانی می‌شود
export const onLoadSubForm = function (name, record, viewModel) {
    debugger;
}

//تابعی کهبعد از انتخاب گزینه ویرایش تودرتو و  نمایش مدال فراخوانی می‌شود
export const onLoadEditSubForm = function (name, record, viewModel) {
    debugger;
}

//این تابع زمانی فرخوانی می شود که اکشن حذف در لیست داده های فرم فرخوانی شده باشد که براساس آن می توان رکورد موردنظر را حذف کرد یا اجازه حذف به این رکورد نداد
export const onBeforeDelete = function (container, options) {
    const record = options.data;// دسترسی به دیتای رکورد فعلی

    return true
}
```
