---
id: 163
title: "تنظیمات WebConfig"
englishTitle: "Web.config Enterprise Hardening"
category: "07-installation"
wikiPath: "163"
wikiUrl: "https://wiki.graphbpms.ir/163"
breadcrumbs: ["نصب و بروزرسانی سامانه","تنظیمات WebConfig"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:30:21"
---

# تنظیمات WebConfig

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `نصب و بروزرسانی سامانه` ❯ `تنظیمات WebConfig`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/163](https://wiki.graphbpms.ir/163)

# تنظیمات Web.Config سامانه گراف

این مستند شامل کلیدهای تنظیمات `appSettings` در فایل `Web.Config` سامانه GraphBPMS است که جنبه‌های مختلف اجرا، امنیت، لاگ‌گیری، مسیر فایل‌ها، کش و سرویس‌ها را کنترل می‌کنند.

## تنظیمات Pipeline و اجرای همزمان

### UseTaskFriendlySynchronizationContext

یک Pipeline ناهمزمان جدید که برای WebSocket و Signal ضروری است. اگر مقدار آن `false` باشد، ممکن است باعث Deadlock شود.

```
<add key="aspnet:UseTaskFriendlySynchronizationContext" value="true"/>
```

## نسخه‌بندی هسته

### BpmsVersionFiles

نسخه فایل‌های هسته BPMS که می‌تواند در نسخه‌بندی Release مورد استفاده قرار گیرد.

```
<add key="BpmsVersionFiles" value="2.0.0"/>
```

## تنظیمات Active Query Builder

### DirectoryJS

مسیر فایل‌های JavaScript مورد استفاده Active Query Builder.

```
<add key="DirectoryJS" value="~/ActiveQueryBuilderSource/js/release/"/>
```

### DirectoryCSS

مسیر فایل‌های CSS مورد استفاده Active Query Builder.

```
<add key="DirectoryCSS" value="~/ActiveQueryBuilderSource/js/css/"/>
```

### HttpCompressionEnabled

فعال یا غیرفعال کردن فشرده‌سازی ترافیک HTTP بین کلاینت و سرور.

```
<add key="HttpCompressionEnabled" value="false"/>
```

## تنظیمات Razor و MVC

### webpages:Version

مورد استفاده Razor Engine جهت ارتقا از MVC 4 به MVC 5 (از 2.0.0.0 به 3.0.0.0).

```
<add key="webpages:Version" value="3.0.0.0"/>
```

هنگامی که `WebMatrix.WebData.dll` در پوشه `/bin` اپلیکیشن ASP.NET MVC 4 قرار می‌گیرد، URL مربوط به Forms Authentication را تصاحب می‌کند. افزودن این اسمبلی به اپلیکیشن (مثلاً با انتخاب «ASP.NET Web Pages with Razor Syntax» در دیالوگ Add Deployable Dependencies) باعث می‌شود ریدایرکت ورود به `/account/logon` به‌جای `/account/login` که توسط Account Controller پیش‌فرض MVC انتظار می‌رود، انجام شود. برای جلوگیری از این رفتار و استفاده از URL مشخص‌شده در بخش authentication فایل web.config، می‌توانید یک appSetting به نام `PreserveLoginUrl` با مقدار true اضافه کنید.

## تنظیمات اعتبارسنجی سمت کلاینت

### ClientValidationEnabled

MVC3 و MVC4 از اعتبارسنجی بدون مزاحمت سمت کلاینت پشتیبانی می‌کنند. در این روش قواعد اعتبارسنجی با استفاده از Attributeهایی که به عناصر HTML تولیدشده اضافه می‌شوند، تعریف می‌شوند. این قواعد توسط کتابخانه JavaScript موجود تفسیر می‌شود و از مقادیر Attribute برای پیکربندی کتابخانه jQuery Validation استفاده می‌کند که کار اعتبارسنجی واقعی را انجام می‌دهد. با تنظیم مقادیر `ClientValidationEnabled` و `UnobtrusiveJavaScriptEnabled` روی true یا false می‌توان اعتبارسنجی سمت کلاینت را فعال یا غیرفعال کرد. این تنظیم در سطح اپلیکیشن اعمال می‌شود.

```
<add key="ClientValidationEnabled" value="true"/>
```

## تنظیمات OWIN

### owin:AutomaticAppStartup

این یک تنظیم پیکربندی است که در اپلیکیشن‌های Self-hosted OWIN استفاده می‌شود. این تنظیم کنترل می‌کند که OWIN چگونه کلاس Startup مسئول راه‌اندازی اپلیکیشن شما را کشف کند:

-   **Value:** این تنظیم می‌تواند `true` (پیش‌فرض) یا `false` باشد.
-   **True (پیش‌فرض):** کشف خودکار را فعال می‌کند. OWIN کلاسی با Attribute `OwinStartup` در اسمبلی‌های شما جستجو می‌کند. این کلاس برای پیکربندی Pipeline اپلیکیشن استفاده می‌شود.
-   **False:** کشف خودکار را غیرفعال می‌کند. باید کلاس Startup را با تنظیم دیگری به نام `owin:AppStartup` به‌صورت دستی مشخص کنید.

```
<add key="owin:AutomaticAppStartup" value="true"/>
```

## محدودیت‌های JSON

### aspnet:MaxJsonDeserializerMembers

**هدف:** پیچیدگی داده‌های JSON ورودی را محدود می‌کند تا از حملات Denial-of-Service (DoS) جلوگیری شود و عملکرد بهبود یابد.

**Value:** این تنظیم یک مقدار صحیح می‌گیرد که نشان‌دهنده حداکثر تعداد جفت‌های کلید-مقدار مجاز در JSON هنگام Deserialize است.

**پیش‌فرض:** به‌طور پیش‌فرض، ASP.NET محدودیت Deserialization JSON را به 1,000 عضو به دلایل امنیتی محدود می‌کند.

**نحوه کار:** وقتی اپلیکیشن ASP.NET شما یک درخواست JSON دریافت می‌کند، فریم‌ورک تلاش می‌کند آن را به ساختارهای داده قابل استفاده Deserialize کند. تنظیم `aspnet:MaxJsonDeserializerMembers` یک آستانه روی تعداد اعضای مجاز در این فرآیند تعریف می‌کند. اگر Payload JSON از این حد فراتر رود، Deserialization شکست می‌خورد و اپلیکیشن معمولاً یک Exception صادر می‌کند.

```
<add key="aspnet:MaxJsonDeserializerMembers" value="5000000"/>
```

### aspnet:MaxJsonLength

**هدف:** اندازه درخواست‌ها یا پاسخ‌های JSON ورودی را محدود می‌کند تا از حملات DoS جلوگیری شود و عملکرد بهبود یابد.

**Value:** این تنظیم یک مقدار صحیح می‌گیرد که نشان‌دهنده حداکثر تعداد کاراکترهای مجاز در رشته JSON است.

**پیش‌فرض:** ASP.NET MVC 4 و پایین‌تر: 102,400 کاراکتر (حدود 100 KB). ASP.NET MVC 5 و بالاتر: 2,097,152 کاراکتر (حدود 2 MB).

```
<add key="aspnet:MaxJsonLength" value="2147483644"/>
```

## مسیرهای فایل سیستم

### TempFilePath

مسیر فایل‌های موقت سیستم.

```
<add key="TempFilePath" value="/Files/Temp"/>
```

### UserFilePath

مسیر فایل‌های کاربران سیستم.

```
<add key="UserFilePath" value="/Files/Users"/>
```

### NewsFilePath

مسیر فایل‌های اخبار سیستم.

```
<add key="NewsFilePath" value="/Files/News"/>
```

### ImagesGalleryFilePath

مسیر فایل‌های گالری تصاویر اخبار سیستم.

```
<add key="ImagesGalleryFilePath" value="/Files/ImagesGallery"/>
```

### DynamicPagesPathFilePath

مسیر فایل‌های صفحات پویا سیستم.

```
<add key="DynamicPagesPathFilePath" value="/Files/Temp"/>
```

### EncryptFilesEnabled

فعال کردن رمزنگاری فایل‌های آرشیو.

```
<add key="EncryptFilesEnabled" value="false"/>
```

### ArchiveNotAllowedFileTypes

لیست Deny برای انواع فایل‌های قابل آپلود.

```
<add key="ArchiveNotAllowedFileTypes" value="exe.msi"/>
```

## تنظیمات لاگ‌گیری

### MaximumLogRecordCount

حداکثر تعداد رکورد ذخیره لاگ در جدول SystemLog.

```
<add key="MaximumLogRecordCount" value="2147483640"/>
```

### SaveControllerLog

SaveSelectLog true/false => true به معنای درج لاگ برای شروع و پایان Controller.

```
<add key="SaveControllerLog" value="true"/>
```

### SaveSelectLog

ذخیره لاگ Queryهای Select.

```
<add key="SaveSelectLog" value="true"/>
```

### SaveInsertLog

ذخیره لاگ Queryهای Insert.

```
<add key="SaveInsertLog" value="true"/>
```

### SaveUpdateLog

ذخیره لاگ Queryهای Update.

```
<add key="SaveUpdateLog" value="true"/>
```

### SaveDeleteLog

ذخیره لاگ Queryهای Delete.

```
<add key="SaveSelectLog" value="true"/>
```

### ShouldLogAgent

ShouldLogAgent true/false => true به معنای درج لاگ برای User Browser Agent.

```
<add key="ShouldLogAgent" value="true"/>
```

### isProviderParamLogActive

مشخص کردن وضعیت ذخیره لاگ‌های سرویس Provider در جدول `dbo.Provider`.

```
<add key="isProviderParamLogActive" value="true"/>
```

### isNotificationLogActive

لاگ‌گیری Notification.

```
<add key="isNotificationLogActive" value="true"/>
```

### LogVerbosityLevel

سطح جزئیات لاگ:

1.  فقط نمایش «Error»
2.  نمایش آخرین پیام Exception
3.  نمایش StackTrace

```
<add key="LogVerbosityLevel" value="3"/>
```

## تنظیمات نشست و احراز هویت

### SessionMultiple

قابلیت ورود همزمان چند کاربر با یک نام کاربری و رمز عبور در سامانه.

```
<add key="SessionMultiple" value="true"/>
```

### IdleTime

عمر Token بر حسب دقیقه.

```
<add key="IdleTime" value="200"/>
```

### ShowCaptcha

نمایش Captcha در زمان ورود به سامانه.

```
<add key="ShowCaptcha" value="false"/>
```

### UseSecureCookie

اگر SSL روی IIS پیکربندی نشده است، مقدار false برای فلگ UseSecureCookie تنظیم کنید.

```
<add key="UseSecureCookie" value="false"/>
```

### autoFormsAuthentication

جلوگیری از تعارض با Auth ما / جلوگیری از ریدایرکت درخواست به `/Account/Login?ReturnUrl=%2f` (باید False باشد).

```
<add key="autoFormsAuthentication" value="false"/>
```

### enableSimpleMembership

غیرفعال شده تا از Microsoft SimpleMembership استفاده نشود (ما از ASP.NET Identity استفاده می‌کنیم).

```
<add key="enableSimpleMembership" value="false"/>
```

### IsLoginPersistent

جلوگیری از خروج کاربر در صورت بسته شدن مرورگر.

```
<add key="IsLoginPersistent" value="true"/>
```

### EnableSingleUserModeLogin

قفل کردن کاربران به یک ورود واحد.

```
<add key="EnableSingleUserModeLogin" value="false"/>
```

## تنظیمات Timeout و Rate Limit

### AjaxScriptTimeOut

مهلت زمانی اسکریپت Ajax.

```
<add key="AjaxScriptTimeOut" value="1000000"/>
```

### WebApiRequestTimeOut

مهلت زمانی درخواست WebApi.

```
<add key="WebApiRequestTimeOut" value="600000"/>
```

### RequestRateLimitPerSeconds

حد محدودیت درخواست‌های هر کاربر بر حسب ثانیه. مقدار پیش‌فرض 0.1 است به این معنی که هر کاربر می‌تواند در 0.1 ثانیه یک درخواست ارسال کند.

```
<add key="RequestRateLimitPerSeconds" value="0.1"/>
```

### aspnet:RequestQueueLimitPerSession

تنظیم پیکربندی در اپلیکیشن‌های ASP.NET که حداکثر تعداد درخواست‌های همزمان مجاز برای هر نشست کاربر را کنترل می‌کند.

-   **هدف:** جلوگیری از overwhelm شدن سرور با درخواست‌های بیش از حد از یک کاربر.
-   **پیش‌فرض:** ASP.NET 4.6.2 و پایین‌تر: بدون محدودیت. ASP.NET 4.7 و بالاتر: 50.

```
<add key="aspnet:RequestQueueLimitPerSession" value="2147483647"/>
```

## تنظیمات امنیت کلمه عبور

### PasswordDifficultyScore

با توجه به گزینه‌های موجود می‌توانید سطح امنیت کلمه عبور کاربر در زمان ساخت اکانت را مشخص کنید:

-   **Blank:** رمز عبور خالی (فقط خالی و/یا فاصله)
-   **VeryWeak:** خیلی کوتاه (کمتر از 5 حرف)، فقط حروف یک‌حالته یا فقط اعداد
-   **Weak:** حداقل 8 حرف، یک شرط قوی برآورده شده است (حداقل 8 حرف با 1 یا بیشتر حروف بزرگ، حروف کوچک، اعداد و کاراکترهای خاص)
-   **Medium:** حداقل 8 حرف، دو شرط قوی برآورده شده است
-   **Strong:** حداقل 8 حرف، سه شرط قوی برآورده شده است
-   **VeryStrong:** حداقل 16 حرف، تمام شرایط قوی برآورده شده است

```
<add key="PasswordDifficultyScore" value="Strong"/>
```

## تنظیمات Script Task

### ScriptTask:EnableCSharpLanguage

قابلیت استفاده از Script Task C# در طراحی فرآیند. اگر در فرآیند از Script Task C# استفاده کرده باشید، غیرفعال کردن این ویژگی باعث می‌شود که از Script Task رد شود.

```
<add key="ScriptTask:EnableCSharpLanguage" value="true"/>
```

### ScriptTask:MaximumTimeout

حداکثر زمان اجرای مجاز بر حسب ثانیه.

```
<add key="ScriptTask:MaximumTimeout" value="90"/>
```

### ScriptTask:MaximumStatements

حداکثر تعداد دستورات مجاز در Script.

```
<add key="ScriptTask:MaximumStatements" value="20000"/>
```

## تنظیمات Bundling

### ShouldUseMinifyBundle

به شما امکان استفاده از نسخه کامل یا Minify شده Bundle را می‌دهد.

```
<add key="ShouldUseMinifyBundle" value="true"/>
```

## تنظیمات SSO تهران

### SSOTehranWebConfigSettingEnabled

```
<add key="SSOTehranWebConfigSettingEnabled" value="true"/>
```

### AppClientID\_tehran

```
<add key="AppClientID_tehran" value=""/>
```

### AppSecretKey\_tehran

```
<add key="AppSecretKey_tehran" value=""/>
```

### Authority\_URL\_Tehran

```
<add key="Authority_URL_Tehran" value=""/>
```

### Redirect\_URL\_Tehran

```
<add key="Redirect_URL_Tehran" value=""/>
```

## هدرهای امنیتی

این تنظیمات می‌توانند در پیکربندی IIS نیز تنظیم شوند. در صورتی که از هاست اشتراکی استفاده می‌کنید یا به IIS دسترسی ندارید، می‌توانید این تنظیمات را اینجا اعمال کنید.

### Global\_X\_FRAME\_Setting

هدر HTTP X-Frame-Options می‌تواند برای نشان دادن اینکه آیا مرورگر باید اجازه رندر یک صفحه را در `<frame>`، `<iframe>`، `<embed>` یا `<object>` داشته باشد یا خیر، استفاده شود. سایت‌ها می‌توانند از این برای جلوگیری از حملات Clickjacking استفاده کنند:

-   **DENY:** صفحه نمی‌تواند در Frame نمایش داده شود، صرف‌نظر از سایتی که تلاش می‌کند این کار را انجام دهد.
-   **SAMEORIGIN:** صفحه فقط در صورتی می‌تواند نمایش داده شود که همه Frameهای والد same origin با خود صفحه باشند.

```
<add key="Global_X_FRAME_Setting" value="SAMEORIGIN"/>
```

### X\_XSS\_Protection

هدر HTTP X-XSS-Protection یک ویژگی در Internet Explorer، Chrome و Safari است که از بارگذاری صفحات هنگام تشخیص حملات Cross-Site Scripting (XSS) جلوگیری می‌کند:

-   `0` => فیلتر XSS را غیرفعال می‌کند.
-   `1` => فیلتر XSS را فعال می‌کند. اگر حمله XSS تشخیص داده شود، مرورگر صفحه را پاک‌سازی می‌کند.
-   `1; mode=block` => فیلتر XSS را فعال می‌کند. به‌جای پاک‌سازی صفحه، مرورگر از رندر صفحه جلوگیری می‌کند اگر حمله تشخیص داده شود.
-   `1; report=<reporting-uri>` => (فقط Chromium) فیلتر XSS را فعال می‌کند. صفحه را پاک‌سازی کرده و نقض را گزارش می‌دهد.

```
<add key="X_XSS_Protection" value="1; mode=block"/>
```

### Content\_Security\_Policy

به‌طور پیش‌فرض روی آن‌ها اعمال می‌شود. این منابعی را که می‌توانند کدی مانند `<script>` بارگذاری کنند محدود می‌کند و شیوه‌های بالقوه ناامن مانند استفاده از `eval()` را ممنوع می‌کند.

-   پیش‌فرض: `"script-src 'self'; object-src 'self';"`
-   Manifest V2 syntax: `"content_security_policy": "script-src 'self' https://graphbpms.com; object-src 'self'"`
-   Manifest V3 syntax: Manifest V3 اجازه URLهای Remote در `script-src` صفحات Extension را نمی‌دهد.

```
<add key="Content_Security_Policy" value="object-src 'none';"/>
```

### Referrer-Policy

هدر HTTP Referrer-Policy کنترل می‌کند که چه مقدار اطلاعات Referrer (ارسال‌شده با هدر Referer) باید در درخواست‌ها گنجانده شود:

-   **no-referrer:** هدر Referer حذف می‌شود.
-   **no-referrer-when-downgrade:** Origin، Path و QueryString را در Referer ارسال می‌کند وقتی سطح امنیت پروتکل یکسان است یا بهبود می‌یابد.
-   **origin:** فقط Origin را در هدر Referer ارسال می‌کند.
-   **origin-when-cross-origin:** برای درخواست same-origin، Origin، Path و QueryString را ارسال می‌کند.
-   **same-origin:** Origin، Path و QueryString را برای درخواست‌های same-origin ارسال می‌کند.
-   **strict-origin:** فقط Origin را وقتی سطح امنیت پروتکل یکسان است ارسال می‌کند.
-   **strict-origin-when-cross-origin (پیش‌فرض):** Origin، Path و QueryString را برای درخواست same-origin ارسال می‌کند.
-   **unsafe-url:** Origin، Path و QueryString را برای هر درخواستی ارسال می‌کند.

```
<add key="Referrer-Policy" value="no-referrer"/>
```

### X\_Content\_Type\_Options

هدر پاسخ HTTP X-Content-Type-Options یک نشانگر است که توسط سرور برای نشان دادن اینکه MIME Typeهای اعلام‌شده در هدرهای Content-Type باید دنبال شوند و تغییر نکنند استفاده می‌شود. هدر `nosniff` یک درخواست را مسدود می‌کند اگر مقصد درخواست از نوع Style باشد و MIME Type `text/css` نباشد، یا از نوع Script باشد و MIME Type یک MIME Type JavaScript نباشد.

```
<add key="X_Content_Type_Options" value=""/>
```

## تنظیمات Timer

### EnableTimer

فعال یا غیرفعال کردن سرویس تایمر.

```
<add key="EnableTimer" value="false"/>
```

## تنظیمات موبایل

### AndroidSDKPath

مسیر Android SDK.

```
<add key="AndroidSDKPath" value="String Path"/>
```

### AllowedMobilesSoftwares

لیست دستگاه‌های قابل قبول برای مرور وب‌سایت.

```
<add key="AllowedMobilesSoftwares" value="*"/>
```

## تنظیمات Cache

### CacheSlidingExpirationInterval

این ویژگی مدت زمانی را تعریف می‌کند که یک آیتم Cache بعد از آخرین دسترسی معتبر می‌ماند. به‌طور معمول بر حسب میلی‌ثانیه، ثانیه یا دقیقه تنظیم می‌شود.

**مثال:** اگر `CacheSlidingExpirationInterval` روی 60 ثانیه تنظیم شود و یک آیتم Cache دسترسی شود، تایمر انقضای آن دوباره شروع می‌شود. آیتم برای 60 ثانیه دیگر از آن نقطه دسترسی معتبر می‌ماند. اگر آیتم در 60 ثانیه دسترسی نشود، به‌طور خودکار از Cache حذف می‌شود.

```
<add key="CacheSlidingExpirationInterval" value="24"/>
```

### CacheAbsoluteExpirationInterval

یک زمان انقضای ثابت برای یک آیتم Cache تعریف می‌کند. آیتم در یک نقطه زمانی از پیش تعریف‌شده، صرف‌نظر از دسترسی، نامعتبر می‌شود.

**مثال:** اگر `CacheAbsoluteExpirationInterval` روی 3 ساعت تنظیم شود، یک آیتم Cache که در ساعت 10:00 صبح اضافه شده، به‌طور خودکار در ساعت 1:00 بعدازظهر نامعتبر و از Cache حذف می‌شود، حتی اگر چندین بار در آن بازه دسترسی شود.

```
<add key="CacheAbsoluteExpirationInterval" value="true"/>
```

## تنظیمات Provider

### ProviderLimitations

محدودیت Provider.

```
<add key="ProviderLimitations" value="true"/>
```
