---
id: 163
title: "تنظیمات WebConfig"
englishTitle: "Web.config Enterprise Hardening"
category: "07-installation"
wikiPath: "163"
wikiUrl: "https://wiki.graphbpms.ir/163"
breadcrumbs: ["تنظیمات WebConfig"]
priority: 4
isCanonical: true
updatedAt: "2026-11-02T02:05:08"
---

# تنظیمات WebConfig

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `تنظیمات WebConfig`  
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

- **Value:** این تنظیم می‌تواند `true` (پیش‌فرض) یا `false` باشد.

- **True (پیش‌فرض):** کشف خودکار را فعال می‌کند. OWIN کلاسی با Attribute `OwinStartup` در اسمبلی‌های شما جستجو می‌کند. این کلاس برای پیکربندی Pipeline اپلیکیشن استفاده می‌شود.

- **False:** کشف خودکار را غیرفعال می‌کند. باید کلاس Startup را با تنظیم دیگری به نام `owin:AppStartup` به‌صورت دستی مشخص کنید.

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

- فقط نمایش «Error»

- نمایش آخرین پیام Exception

- نمایش StackTrace

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

این کلید منسوخ و از مدار خارج شده است و جایگزین آن کلید `RateLimitEndpoints` است. برای تنظیم محدودیت نرخ به همان کلید و بخش «[محدودیت نرخ درخواست (Rate Limiting)](#rate-limit)» مراجعه کنید.

### RateLimitEndpoints

تنها کلید پیکربندی محدودیت نرخ درخواست (Rate Limiting) در Web.Config. مقدار آن فهرست قوانین اندپوینت‌هاست و به یکی از دو فرمت نوشته می‌شود: رشته‌ای (قوانین جداشده با سمی‌کالن یا شکست خط) یا آرایه JSON. کلید دیگری برای فعال‌سازی لازم نیست. شرح کامل فرمت‌ها، شروط پویا، اولویت‌بندی و اندپوینت‌های پیش‌فرض در بخش «[محدودیت نرخ درخواست (Rate Limiting)](#rate-limit)» همین صفحه آمده است.

```
<add key="RateLimitEndpoints" value="POST /Login/Authenticate:5:60;/Process/RunProcess:30:60:true"/>
```

### aspnet:RequestQueueLimitPerSession

تنظیم پیکربندی در اپلیکیشن‌های ASP.NET که حداکثر تعداد درخواست‌های همزمان مجاز برای هر نشست کاربر را کنترل می‌کند.

- **هدف:** جلوگیری از overwhelm شدن سرور با درخواست‌های بیش از حد از یک کاربر.

- **پیش‌فرض:** ASP.NET 4.6.2 و پایین‌تر: بدون محدودیت. ASP.NET 4.7 و بالاتر: 50.

```
<add key="aspnet:RequestQueueLimitPerSession" value="2147483647"/>
```

## تنظیمات امنیت کلمه عبور

### PasswordDifficultyScore

با توجه به گزینه‌های موجود می‌توانید سطح امنیت کلمه عبور کاربر در زمان ساخت اکانت را مشخص کنید:

- **Blank:** رمز عبور خالی (فقط خالی و/یا فاصله)

- **VeryWeak:** خیلی کوتاه (کمتر از 5 حرف)، فقط حروف یک‌حالته یا فقط اعداد

- **Weak:** حداقل 8 حرف، یک شرط قوی برآورده شده است (حداقل 8 حرف با 1 یا بیشتر حروف بزرگ، حروف کوچک، اعداد و کاراکترهای خاص)

- **Medium:** حداقل 8 حرف، دو شرط قوی برآورده شده است

- **Strong:** حداقل 8 حرف، سه شرط قوی برآورده شده است

- **VeryStrong:** حداقل 16 حرف، تمام شرایط قوی برآورده شده است

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

### AppClientID_tehran

```
<add key="AppClientID_tehran" value=""/>
```

### AppSecretKey_tehran

```
<add key="AppSecretKey_tehran" value=""/>
```

### Authority_URL_Tehran

```
<add key="Authority_URL_Tehran" value=""/>
```

### Redirect_URL_Tehran

```
<add key="Redirect_URL_Tehran" value=""/>
```

## هدرهای امنیتی

این تنظیمات می‌توانند در پیکربندی IIS نیز تنظیم شوند. در صورتی که از هاست اشتراکی استفاده می‌کنید یا به IIS دسترسی ندارید، می‌توانید این تنظیمات را اینجا اعمال کنید.

### Global_X_FRAME_Setting

هدر HTTP X-Frame-Options می‌تواند برای نشان دادن اینکه آیا مرورگر باید اجازه رندر یک صفحه را در `<frame>`، `<iframe>`، `<embed>` یا `<object>` داشته باشد یا خیر، استفاده شود. سایت‌ها می‌توانند از این برای جلوگیری از حملات Clickjacking استفاده کنند:

- **DENY:** صفحه نمی‌تواند در Frame نمایش داده شود، صرف‌نظر از سایتی که تلاش می‌کند این کار را انجام دهد.

- **SAMEORIGIN:** صفحه فقط در صورتی می‌تواند نمایش داده شود که همه Frameهای والد same origin با خود صفحه باشند.

```
<add key="Global_X_FRAME_Setting" value="SAMEORIGIN"/>
```

### X_XSS_Protection

هدر HTTP X-XSS-Protection یک ویژگی در Internet Explorer، Chrome و Safari است که از بارگذاری صفحات هنگام تشخیص حملات Cross-Site Scripting (XSS) جلوگیری می‌کند:

- `0` => فیلتر XSS را غیرفعال می‌کند.

- `1` => فیلتر XSS را فعال می‌کند. اگر حمله XSS تشخیص داده شود، مرورگر صفحه را پاک‌سازی می‌کند.

- `1; mode=block` => فیلتر XSS را فعال می‌کند. به‌جای پاک‌سازی صفحه، مرورگر از رندر صفحه جلوگیری می‌کند اگر حمله تشخیص داده شود.

- `1; report=<reporting-uri>` => (فقط Chromium) فیلتر XSS را فعال می‌کند. صفحه را پاک‌سازی کرده و نقض را گزارش می‌دهد.

```
<add key="X_XSS_Protection" value="1; mode=block"/>
```

### Content_Security_Policy

به‌طور پیش‌فرض روی آن‌ها اعمال می‌شود. این منابعی را که می‌توانند کدی مانند `<script>` بارگذاری کنند محدود می‌کند و شیوه‌های بالقوه ناامن مانند استفاده از `eval()` را ممنوع می‌کند.

- پیش‌فرض: `"script-src 'self'; object-src 'self';"`

- Manifest V2 syntax: `"content_security_policy": "script-src 'self' https://graphbpms.com; object-src 'self'"`

- Manifest V3 syntax: Manifest V3 اجازه URLهای Remote در `script-src` صفحات Extension را نمی‌دهد.

```
<add key="Content_Security_Policy" value="object-src 'none';"/>
```

### Referrer-Policy

هدر HTTP Referrer-Policy کنترل می‌کند که چه مقدار اطلاعات Referrer (ارسال‌شده با هدر Referer) باید در درخواست‌ها گنجانده شود:

- **no-referrer:** هدر Referer حذف می‌شود.

- **no-referrer-when-downgrade:** Origin، Path و QueryString را در Referer ارسال می‌کند وقتی سطح امنیت پروتکل یکسان است یا بهبود می‌یابد.

- **origin:** فقط Origin را در هدر Referer ارسال می‌کند.

- **origin-when-cross-origin:** برای درخواست same-origin، Origin، Path و QueryString را ارسال می‌کند.

- **same-origin:** Origin، Path و QueryString را برای درخواست‌های same-origin ارسال می‌کند.

- **strict-origin:** فقط Origin را وقتی سطح امنیت پروتکل یکسان است ارسال می‌کند.

- **strict-origin-when-cross-origin (پیش‌فرض):** Origin، Path و QueryString را برای درخواست same-origin ارسال می‌کند.

- **unsafe-url:** Origin، Path و QueryString را برای هر درخواستی ارسال می‌کند.

```
<add key="Referrer-Policy" value="no-referrer"/>
```

### X_Content_Type_Options

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

## محدودیت نرخ درخواست (Rate Limiting)

### مقدمه و کاربرد

محدودیت نرخ درخواست (Rate Limiting) مکانیزمی است که تعداد درخواست‌های مجاز یک کلاینت به یک اندپوینت را در یک بازه زمانی مشخص محدود می‌کند. این قابلیت از اندپوینت‌ها در برابر حملات منع سرویس (DoS/DDoS)، حملات حدس کلمه عبور (Brute-Force)، فراخوانی‌های مکرر و معیوب ناشی از لوپ در کدهای کلاینت و اشباع منابع سرور (Thread Pool و Connection Pool) محافظت می‌کند.

در Web.Config کنترل محدودیت نرخ و قوانین اندپوینت‌ها **منحصراً از طریق یک کلید به نام** `RateLimitEndpoints` درون `appSettings` انجام می‌شود. کلید دیگری (مانند `RateLimitEnabled`) لازم نیست و نباید اضافه شود.

کلید قدیمی `RequestRateLimitPerSeconds` و اتریبیوت `RateLimitAttribute` (که روی کنترلرهایی مانند `LoginController` و `AnonymousFormController` استفاده می‌شد) از مدار خارج و حذف شده‌اند و جای آن‌ها را `RateLimitEndpoints` گرفته است.

### نحوه عملکرد (چرخه حیات درخواست)

سیستم محدودیت نرخ در لایه Middleware خط‌لوله اجرایی برنامه مستقر است و هر درخواست ورودی را پیش از ورود به اکشن‌ها و کنترلرهای BPMS بررسی می‌کند:

- اگر برای مسیر درخواست هیچ قانونی تعریف نشده باشد، درخواست بدون تاخیر مستقیماً به کنترلر می‌رسد.

- اگر حداقل یک قانون برای مسیر وجود داشته باشد، متد HTTP تطبیق داده می‌شود و شروط پویا (Condition) ارزیابی می‌شوند.

- هویت کلاینت تشخیص داده می‌شود: آدرس IP کلاینت (با پشتیبانی از پروکسی و `X-Forwarded-For`) یا شناسه کاربر لاگین‌شده (`UserId`).

- شمارنده درخواست‌ها در پنجره زمانی افزایش می‌یابد. اگر سهمیه مجاز باشد، هدرهای `X-RateLimit-*` به پاسخ افزوده می‌شود و درخواست عبور می‌کند.

- اگر سهمیه تمام شده باشد، درخواست بلافاصله قطع می‌شود (بدون ورود به BPMS) و پاسخ `429 Too Many Requests` همراه با هدرهای `Retry-After` و `X-RateLimit-Reset` و پیام خطای سفارشی برگردانده می‌شود.

### روش‌های تنظیم محدودیت نرخ

محدودیت نرخ در چهار محل قابل تنظیم است:

- **Web.Config:** کلید `RateLimitEndpoints` در `appSettings` (همین صفحه).

- **تنظیمات سیستم (SystemConfiguration):** رکورد `RateLimitEndpoints` در جدول `SystemConfiguration` با مقدار JSON؛ تغییرات آن بدون ریست سرور و به‌صورت زنده اعمال می‌شود.

- **وب‌سرویس منبع داده (Provider سرویس‌باس):** بخش «محدودیت نرخ درخواست» در فرم سرویس‌باس.

- **وب‌سرویس فرآیند (Process ESB):** پنجره تنظیمات وب‌سرویس فرآیند در طراح فرآیند BPMN.

تنظیماتی که در فرم Provider و ESB وارد می‌شوند در جدول یا ستون جداگانه ذخیره نمی‌شوند، بلکه با نام `ServiceBus_{Id}` درون همان کلید `RateLimitEndpoints` در `SystemConfiguration` ثبت می‌شوند. برای جزئیات هر روش به مستند مربوط به آن مراجعه کنید.

### تنظیم در Web.Config

قوانین در مقدار کلید `RateLimitEndpoints` نوشته می‌شوند و دو فرمت پشتیبانی می‌شود. موتور `RateLimitEndpointManager` هنگام بارگذاری، اگر مقدار با `[` شروع و با `]` تمام شود، آن را به‌صورت خودکار JSON در نظر می‌گیرد.

روش اول: فرمت رشته‌ای (تفکیک با سمی‌کالن)

قوانین با کاراکتر `;` یا شکست خط (Newline) از هم جدا می‌شوند. خطوطی که با `#` شروع شوند کامنت هستند و نادیده گرفته می‌شوند. ساختار هر قانون:

```
[METHOD ]PATH:LIMIT:WINDOW_IN_SECONDS[:LIMIT_BY_USER[:MESSAGE[:CONDITION]]]
```

- **METHOD (اختیاری):** متد اختصاصی مانند `POST`، `GET`، `PUT` یا `DELETE`. در صورت عدم ذکر، قانون روی همه متدها اعمال می‌شود.

- **PATH (اجباری):** مسیر اندپوینت در سامانه. اسلش ابتدایی به‌صورت خودکار نرمال می‌شود. مثال: `/Process/RunProcess`

- **LIMIT (اجباری):** حداکثر تعداد درخواست مجاز در بازه زمانی (عدد صحیح بزرگ‌تر از صفر).

- **WINDOW_IN_SECONDS (اجباری):** طول بازه زمانی بر حسب ثانیه (عدد صحیح بزرگ‌تر از صفر).

- **LIMIT_BY_USER (اختیاری):** مقدار `true` یا `false` (پیش‌فرض: `false`). با `false` سهمیه‌بندی بر پایه آدرس IP کلاینت (با پشتیبانی از پروکسی و `X-Forwarded-For`) و با `true` بر اساس شناسه کاربر لاگین‌شده (`UserId`) انجام می‌شود.

- **MESSAGE (اختیاری):** متن خطای سفارشی یا کلید ترجمه از فایل‌های منابع زبان. عبارت `{0}` با ثانیه‌های باقیمانده جایگزین می‌شود.

- **CONDITION (اختیاری):** شرط ارزیابی محتوای درخواست (بدنه JSON، فرم، کوئری یا هدر). جزئیات در بخش «شروط پویا» آمده است.

نمونه قانون ساده (حداکثر ۵ درخواست در ۶۰ ثانیه روی متد POST):

```
<add key="RateLimitEndpoints" value="POST /Login/Authenticate:5:60"/>
```

نمونه قانون بر مبنای کاربر (`UserId`):

```
<add key="RateLimitEndpoints" value="/Process/SubmitContinueProcess:60:60:true"/>
```

نمونه قانون با پیام سفارشی:

```
<add key="RateLimitEndpoints" value="POST /Login/Authenticate:5:60:false:تعداد ورود بیش از حد مجاز است. لطفاً {0} ثانیه دیگر تلاش فرمایید."/>
```

نمونه قانون شرطی:

```
<add key="RateLimitEndpoints" value="POST /Process/RunProcess:5:60:true:سهمیه موبایل پر شد:Headers:X-Source == 'MobileApp'"/>
```

نمونه چندقانونی ترکیبی:

```
<add key="RateLimitEndpoints" value="
    # اندپوینت‌های حساس لاگین بر مبنای IP
    POST /Login/Authenticate:5:60:false:تعداد تلاش‌های ورود ناموفق بیش از حد مجاز است. لطفاً {0} ثانیه دیگر تلاش نمایید.;
    /Login/CheckUserNameExists:10:1;

    # اندپوینت‌های فرآیندی بر مبنای شناسه کاربری
    /Process/SubmitContinueProcess:60:60:true;
    /Process/RunProcess:30:60:true;

    # اندپوینت شرطی با پیام اختصاصی
    POST /SharedApi/ThrowProcessSignal:5:60:false:سیگنال‌های حیاتی محدود می‌باشند:Content.EndVariables[0].Value == true;

    # قانون بازگشتی (Fallback) برای همان مسیر
    POST /SharedApi/ThrowProcessSignal:60:60:false
"/>
```

روش دوم: فرمت JSON

مقدار همین کلید را می‌توان مستقیماً به‌صورت آرایه‌ای از آبجکت‌های JSON مقداردهی کرد:

```
<add key="RateLimitEndpoints" value='[
  {
    "Name": "Login_Auth_Rule",
    "Path": "/Login/Authenticate",
    "HttpMethod": "POST",
    "Limit": 5,
    "WindowInSeconds": 60,
    "LimitByUser": false,
    "Message": "تعداد تلاش‌های ورود بیش از حد مجاز است. لطفاً {0} ثانیه دیگر تلاش فرمایید."
  },
  {
    "Name": "Process_Submit_User",
    "Path": "/Process/SubmitContinueProcess",
    "Limit": 60,
    "WindowInSeconds": 60,
    "LimitByUser": true
  },
  {
    "Name": "Signal_VIP_Condition",
    "Path": "/SharedApi/ThrowProcessSignal",
    "HttpMethod": "POST",
    "Limit": 5,
    "WindowInSeconds": 60,
    "LimitByUser": false,
    "Condition": "Content.EndVariables[0].Value == true",
    "Message": "ارسال سیگنال‌های حیاتی محدود است."
  },
  {
    "Name": "Signal_Fallback",
    "Path": "/SharedApi/ThrowProcessSignal",
    "HttpMethod": "POST",
    "Limit": 60,
    "WindowInSeconds": 60,
    "LimitByUser": false
  }
]'/>
```

### شروط پویا (Condition)

با استفاده از Condition می‌توان قانون را فقط برای درخواست‌هایی اعمال کرد که محتوای آن‌ها شرط مورد نظر را برآورده کند. موتور ارزیابی شروط از ساختارهای تودرتو و آرایه‌ها پشتیبانی می‌کند. پیشوندهای تعیین‌کننده منبع داده:

- **بدنه JSON:** پیشوندهای `Body:`، `Json:`، `Payload:` یا مستقیماً نام فیلد. مثال: `Content.EndVariables[0].Value == true`

- **داده‌های فرم:** پیشوندهای `FormData:`، `Form:`، `FormUrlEncoded:`. مثال: `FormData:action == "submit"`

- **پارامترهای آدرس (Query String):** پیشوندهای `Query:`، `QueryString:`، `QueryParams:`. مثال: `Query:isHeavy == "true"`

- **هدرهای درخواست:** پیشوندهای `Headers:`، `Header:`. مثال: `Headers:X-Client-Tier == "VIP"`

عملگرهای پشتیبانی‌شده:

- **مقایسه‌ای:** `==`، `!=`، `>`، `<`، `>=`، `<=`

- **متنی و مجموعه‌ای:** `contains` (شامل بودن زیررشته یا عضویت در آرایه)

- **منطقی:** `&&` (و منطقی) و `||` (یا منطقی)

### اولویت‌بندی و قانون بازگشتی (Fallback)

وقتی چند قانون برای یک مسیر تعریف شده باشد، موتور آن‌ها را به این ترتیب ارزیابی می‌کند:

- ابتدا قوانین دارای شرط (Condition) بررسی می‌شوند. در بین آن‌ها، قانونی که متد اختصاصی دارد (مثلاً `POST`) بر قانون با متد عمومی تقدم دارد.

- در صورت برقراری شرط یک قانون، همان قانون انتخاب می‌شود و بررسی پایان می‌یابد.

- اگر هیچ‌یک از شروط برقرار نباشد، قانون عمومی بدون شرط به‌عنوان سهمیه پایه (Fallback) اعمال می‌شود.

### اندپوینت‌های پیش‌فرض

در مقدار پیش‌فرض Web.Config، اندپوینت‌های پرکاربرد سامانه در حوزه‌های احراز هویت، فرم‌های عمومی بدون لاگین، پیشبرد کارتابل فرآیندی، سرویس‌های اشتراکی و محیط تست شبیه‌ساز پیکربندی شده‌اند. هر مورد به‌صورت «حداکثر درخواست / بازه (ثانیه)» و مبنای محدودسازی آمده است.

**احراز هویت و فرم‌های عمومی (مبنا: آدرس IP):**

- `/Login/Authenticate`: ۱۰ درخواست / ۱ ثانیه. حفاظت در برابر حملات حدس کلمه عبور (Brute-Force).

- `/Login/CheckUserNameExists`: ۱۰ درخواست / ۱ ثانیه. جلوگیری از سرشماری نام کاربری (Username Enumeration).

- `/Login/RegisterNewUser`: ۱۰ درخواست / ۱ ثانیه. جلوگیری از ساخت انبوه حساب کاربری توسط بات‌ها.

- `/AnonymousForm/Submit`: ۱۰ درخواست / ۱ ثانیه. کنترل بار ارسال داده فرم‌های عمومی ناشناس.

- `/AnonymousForm/GenerateCaptcha`: ۱۰ درخواست / ۱ ثانیه. جلوگیری از بار سنگین پردازنده در رندر مداوم کپچا.

- `/AnonymousForm/CheckCaptchaCodeWithToken`: ۱۰ درخواست / ۱ ثانیه. جلوگیری از آزمون و خطای توکن کپچا.

- `/AnonymousForm/CheckCaptchaCode`: ۱۰ درخواست / ۱ ثانیه. اعتبارسنجی کپچا در فرم‌های عادی بدون توکن.

- `/AnonymousForm/RunProcess`: ۲۰ درخواست / ۶۰ ثانیه. جلوگیری از ایجاد بیش از حد پرونده فرآیندی توسط کاربران ناشناس.

- `/AnonymousForm/ContinueProcess`: ۳۰ درخواست / ۶۰ ثانیه. کنترل گام‌های پیشبرد فرآیند ناشناس.

**کارتابل و فرآیندها (مبنا: شناسه کاربر `UserId`):**

- `/Process/SubmitContinueProcess`: ۶۰ درخواست / ۶۰ ثانیه. ثبت نهایی و پیشبرد کارتابل.

- `/Process/ContinueProcess`: ۶۰ درخواست / ۶۰ ثانیه. پیشبرد گام‌به‌گام فرآیندها.

- `/Process/SubmitForm`: ۶۰ درخواست / ۶۰ ثانیه. ارسال و ذخیره داده فرم‌های پویا.

- `/Process/RunProcess`: ۳۰ درخواست / ۶۰ ثانیه. شروع فرآیند جدید و جلوگیری از ثبت تکراری ناشی از دبل‌کلیک.

- `/Process/ContinueProcessByMobileTask`: ۶۰ درخواست / ۶۰ ثانیه. پیشبرد تسک‌ها از اپلیکیشن موبایل یا PWA.

- `/Process/ContinueProcessWithMessage`: ۴۰ درخواست / ۶۰ ثانیه. ارسال پیام به رویدادهای میانی پیام.

- `/Process/ContinueProcessByMessageWithVerbosityLevel`: ۴۰ درخواست / ۶۰ ثانیه. ارسال پیام با تعیین سطح لاگ.

- `/Process/TriggerContinueProcess`: ۴۰ درخواست / ۶۰ ثانیه. تریگر پیشبرد خودکار مراحل متوقف‌شده.

- `/Process/ProcessFlowManager`: ۳۰ درخواست / ۶۰ ثانیه. عملیات مدیریتی جریان اجرای فرآیند و شاخه‌های موازی.

- `/Process/ThrowSignal`: ۲۰ درخواست / ۶۰ ثانیه. ارسال سیگنال‌های فرآیندی.

- `/Process/RevertProcess`: ۲۰ درخواست / ۶۰ ثانیه. بازگردانی و رد پرونده به مراحل پیشین.

- `/Process/StartReccuringProcess`: ۲۰ درخواست / ۶۰ ثانیه. شروع فرآیندهای دوره‌ای و زمان‌بندی‌شده.

**سرویس‌های اشتراکی (مبنا: آدرس IP):**

- `/SharedApi/RunProcess`: ۱۲۰ درخواست / ۶۰ ثانیه. شروع فرآیند از سامانه‌های ثالث.

- `/SharedApi/ContinueProcessWithMessage`: ۱۲۰ درخواست / ۶۰ ثانیه. ارسال پیام سرویس‌های سازمانی به فرآیندها.

- `/SharedApi/ContinueProcessByMessageWithVerbosityLevel`: ۱۲۰ درخواست / ۶۰ ثانیه. ارسال پیام با جزئیات لاگ در وب‌سرویس‌های برون‌سازمانی.

- `/SharedApi/ThrowProcessSignal`: ۶۰ درخواست / ۶۰ ثانیه. ارسال سیگنال‌های بین‌سازمانی.

- `/SharedApi/RunProcessWithTimer`: ۳۰۰ درخواست / ۶۰ ثانیه. عملیات دسته‌ای (Batch) با تایمرهای پیشرفته.

- `/SharedApi/ContinueProcessWithTimer`: ۳۰۰ درخواست / ۶۰ ثانیه. پیشبرد فرآیندها با کنترل زمان انقضای تایمر.

**محیط تست شبیه‌ساز (مبنا: شناسه کاربر `UserId`):**

- `/ProcessTestEnvironment/RunProcess`: ۶۰ درخواست / ۶۰ ثانیه. اجرای شبیه‌ساز فرآیند در محیط طراحی و دیباگ BPMN.

- `/ProcessTestEnvironment/MoveProcess`: ۱۲۰ درخواست / ۶۰ ثانیه. انتقال دستی توکن فرآیند روی گره‌های شبیه‌ساز.

### پاسخ هنگام عبور از سقف مجاز (429)

وقتی کلاینت از سقف سهمیه عبور کند، سرور بلافاصله با کد `429 Too Many Requests` و هدرهای استاندارد پاسخ می‌دهد:

```
HTTP/1.1 429 Too Many Requests
Content-Type: application/json; charset=utf-8
Retry-After: 48
X-RateLimit-Limit: 10
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1729345200

{
    "HasError": true,
    "Message": "تعداد درخواست‌های شما بیش از حد مجاز است. لطفاً 48 ثانیه دیگر مجدداً تلاش نمایید.",
    "RetryAfter": 48
}
```

کلاینت‌ها باید مقدار هدر `Retry-After` را (بر حسب ثانیه) رعایت کنند و پس از آن مجدداً تلاش کنند.

### نکات تکمیلی

- تغییر در Web.Config توسط IIS بازخوانی می‌شود و نیازی به کامپایل مجدد فایل‌های DLL ندارد، اما موجب ریست شدن دامنه کاربردی (AppDomain) و خروج موقت کاربران آنلاین می‌شود. برای اعمال زنده تغییرات بدون ریست، از روش «تنظیمات سیستم» (`SystemConfiguration`) استفاده کنید.

- اگر محدودیت بر مبنای IP است و کاربران شبکه محلی با یک IP خروجی (NAT/Proxy) دیده می‌شوند، ممکن است خطای ۴۲۹ برای همه آن‌ها صادر شود. در این حالت یا قانون را بر مبنای کاربر (`LimitByUser = true`) تنظیم کنید، یا هدر `X-Forwarded-For` را در پروکسی فعال نمایید.

## مرجع جامع و عملیاتی کلیدهای appSettings

این بخش مرجع کامل کلیدهای `appSettings` سامانه شامل نوع داده، مقدار پیش‌فرض، سطح حساسیت، شرح رفتار و نمونه پیکربندی، ویژه مهندسان انتشار (Publish)، تیم‌های DevOps و مدیران سیستم است. محیط هدف: Microsoft IIS 8.5 به بالا بر بستر .NET Framework 4.7.2 در حالت Integrated Pipeline.

### اصول معماری پیکربندی و چرخه حیات Web.Config در IIS

فایل `Web.config` در ریشه سامانه، رفتارهای اساسی موتور زمان اجرای دات‌نت (ASP.NET Runtime Pipeline)، خط‌لوله امنیتی OWIN، سیاست‌های ممیزی دیتابیس، کنترل‌های ارتباطی شبکه، درگاه‌های وب‌سرویس و سهمیه‌بندی منابع را هدایت می‌کند.

چرخه بازخوانی و بازیافت دامنه کاربردی (AppDomain Recycling)

در IIS، کدهای دات‌نت درون یک دامنه کاربردی (AppDomain) داخل پروسه `w3wp.exe` اجرا می‌شوند. متغیرهای تگ `<appSettings>` در اولین درخواست بازخوانی شده و در کش حافظه نگهداری می‌شوند.

**هشدار برای تیم‌های استقرار و انتشار:** با هر ویرایش و ذخیره فایل `Web.config` روی سرور، مکانیزم File Change Notification در IIS فعال شده و فوراً **AppDomain Recycle** انجام می‌شود. پیامدها:

- **خروج ناگهانی کاربران فعال:** تمام نشست‌های موجود در حافظه (InProc Sessions) از بین می‌رود و کاربران آنلاین با پیام انقضای نشست به صفحه لاگین هدایت می‌شوند.

- **افت موقت کارایی (Cold Start):** کش‌های کدهای کامپایل‌شده، نگاشت‌های ORM و توکن‌ها پاک می‌شود و درخواست‌های نخست تا گرم شدن مجدد برنامه (Warm-up) چند ثانیه تاخیر دارند.

- **دستورالعمل انتشار:** فایل `Web.config` را در ساعات اوج بار ترافیکی تغییر ندهید. ویرایش‌های پیکربندی را در پنجره تعمیرات (Maintenance Window) یا با هماهنگی قبلی انجام دهید.

معماری دولایه پیکربندی: Web.config در برابر SystemConfiguration

- **لایه اول (Web.config):** متغیرهایی که در لحظه لود اولیه دات‌نت، ثبت کانتینر Autofac، پایپ‌لاین OWIN یا پروتکل‌های سوکت ویندوز الزامی هستند (مانند `aspnet:UseTaskFriendlySynchronizationContext` یا سقف اعضای JSON).

- **لایه دوم (SystemConfiguration در پایگاه داده):** متغیرهایی که نیازمند اعمال زنده و بدون قطعی (Zero-Downtime Live Updates) هستند (مانند قوانین سهمیه‌بندی Rate Limit و شروط پویا). در اغلب ماژول‌ها ابتدا Web.config بررسی می‌شود و در صورت خالی بودن، مقدار از دیتابیس خوانده می‌شود.

### جدول ماتریسی کاتالوگ کلیدهای appSettings

نمای کلی کلیدهای پشتیبانی‌شده سامانه به تفکیک نوع داده، مقدار پیش‌فرض، سطح حساسیت و حوزه مسئولیت:

ردیفنام کلیدنوع دادهمقدار پیش‌فرضسطح حساسیتدسته‌بندی و حوزه مسئولیت

۱`aspnet:UseTaskFriendlySynchronizationContext`Booleantrueبحرانیزیرساخت دات‌نت؛ پیشگیری از Deadlock ناهمگام
۲`aspnet:MaxJsonLength`Int322147483647متوسطسقف مجاز حجم دیسریالایز JSON فرم‌ها و سرویس‌ها
۳`aspnet:MaxJsonDeserializerMembers`Int322147483647متوسطسقف اعضای اشیاء JSON در دیسریالایزر MVC
۴`aspnet:RequestQueueLimitPerSession`Int322147483647متوسطسقف صف درخواست‌های همزمان یک نشست کلاینت
۵`webpages:Version`String3.0.0.0استانداردنسخه موتور ASP.NET WebPages / Razor
۶`webpages:Enabled`Booleanfalseمتوسطمسدودسازی اجرای مستقیم فایل‌های cshtml بدون کنترلر
۷`PreserveLoginUrl`Booleantrueاستانداردحفظ آدرس صفحه ورود در ریدایرکت‌های احراز هویت
۸`ClientValidationEnabled`Booleantrueاستاندارداعتبارسنجی سمت کلاینت با متادیتاهای مدل
۹`UnobtrusiveJavaScriptEnabled`Booleantrueاستاندارداعتبارسنجی غیرمزاحم جاوااسکریپت با دیتااتریبیوت HTML5
۱۰`owin:AutomaticAppStartup`Booleantrueبحرانیکشف و اجرای خودکار کلاس Startup میدل‌ویرهای OWIN
۱۱`autoFormsAuthentication`Booleanfalseبحرانیغیرفعال‌سازی ماژول سنتی FormsAuth به نفع OWIN
۱۲`enableSimpleMembership`Booleanfalseبحرانیغیرفعال‌سازی پرووایدر قدیمی SimpleMembership
۱۳`TempFilePath`VirtualPath/Files/Temp/متوسطمسیر نگهداری فایل‌های موقت و خروجی اکسپورت‌ها
۱۴`UserFilePath`VirtualPath/Files/Users/متوسطمسیر تصاویر پروفایل و امضای دیجیتال پرسنل
۱۵`NewsFilePath`VirtualPath/Files/News/متوسطمسیر پیوست‌ها و تصاویر ماژول اخبار سازمانی
۱۶`ImagesGalleryFilePath`VirtualPath/Files/ImagesGallery/متوسطمسیر گالری تصاویر سیستم و آیکون‌های فرم
۱۷`DynamicPagesPath`VirtualPath/Files/Temp/متوسطمسیر صفحات داینامیک و قالب‌های کامپایل‌شده
۱۸`EncryptFilesEnabled`Booleanfalseامنیت دیسکخط‌مشی عمومی رمزنگاری متقارن فایل‌ها بر روی استوریج
۱۹`ArchiveNotAllowedFileTypes`CommaListexe,msiالزام افتافیلتر پسوند و امضای باینری فایل‌های اجرایی خطرناک
۲۰`largeFileMinLength`Int64 (Bytes)1000000000کارایی حافظهآستانه بایت تفکیک استریم فایل‌های حجیم (1GB)
۲۱`AllowAnonymousReadFileServices`Booleanfalseامنیت دسترسیمجوز دانلود فایل‌های بایگانی توسط کاربران ناشناس
۲۲`AllowAnonymousWriteFileServices`Booleanfalseامنیت دسترسیمجوز بارگذاری فایل در بایگانی توسط کاربران ناشناس
۲۳`ArchiveHelper:MinIoEndpoint`Host:Portخالیزیرساخت ابریآدرس سرور یا کلاستر آبجکت استوریج MinIO
۲۴`ArchiveHelper:MinIoAccessKey`Credentialخالیاطلاعات حساسکلید دسترسی (Access Key) در کلاستر MinIO
۲۵`ArchiveHelper:MinIoSecretKey`Credentialخالیاطلاعات حساسرمز عبور محرمانه (Secret Key) کلاستر MinIO
۲۶`ArchiveHelper:MinIoWithSSL`Booleanfalseامنیت انتقالاستفاده از پروتکل امن HTTPS در تبادل با MinIO
۲۷`ArchiveHelper:MinIoIsSSLSecure`Booleanfalseاعتبارسنجی SSLاعتبارسنجی رسمی گواهی SSL در اتصال به MinIO
۲۸`ArchiveHelper:MinIoDefaultRequestHeadersExpectContinue`Booleanfalseاستانداردارسال هدر Expect: 100-continue در ارسال به MinIO
۲۹`SystemSecurityProtocols`CommaListTls,Tls11,Tls12,Tls13...امنیت شبکهپروتکل‌های خروجی مجاز SSL/TLS و سوییچ none
۳۰`IgnoreSystemSecurityProtocolsForSelfRequests`Booleantrueمتوسطنادیده گرفتن الزامات SSL برای درخواست‌های لوکال سرور
۳۱`Global_X_FRAME_Setting`HeaderValSAMEORIGINالزام افتاهدر امنیتی ضد حمله Clickjacking (X-Frame-Options)
۳۲`X_XSS_Protection`HeaderVal1; mode=blockالزام افتاهدر فعال‌سازی محافظت از تزریق اسکریپت در مرورگر
۳۳`Content_Security_Policy`HeaderValobject-src 'none';الزام افتاهدر استاندارد CSP جهت محدودسازی آبجکت‌های ناامن
۳۴`Referrer-Policy`HeaderValno-referrerالزام افتاعدم افشای آدرس صفحه قبلی به سایت‌های ثالث
۳۵`X_Content_Type_Options`HeaderValnosniffالزام افتامسدودسازی حدس نوع محتوا توسط مرورگر (ضد Sniffing)
۳۶`DefaultConnectionLimit`Int32200کارایی شبکهسقف سوکت‌های همزمان خروجی به هر هاست (پیش‌فرض دات‌نت: ۲)
۳۷`Expect100Continue`Booleanfalseلیتنسی شبکهغیرفعال‌سازی هندشیک دو مرحله‌ای برای کاهش تاخیر HTTP
۳۸`UseNagleAlgorithm`Booleanfalseلیتنسی شبکهغیرفعال‌سازی بافر TCP نیگل جهت ارسال بلادرنگ بسته‌ها
۳۹`TrustedProxies`CommaListخالیامنیت لودبالانسرلیست IP پروکسی‌های معتبر جهت استخراج امن هدر XFF
۴۰`AnonymousFormAccess`CommaList* (همه)کنترل دسترسیلیست سفید IPهای مجاز به مشاهده فرم‌های ناشناس
۴۱`CustomJsDenyRegexes`RegexPatternعبارات XHR/AJAXامنیت فرانت‌اندفیلتر کدهای خطرناک جاوااسکریپت در فرم‌ساز پویا
۴۲`SessionMultiple`Booleantrueتجربه کاربریاجازه باز شدن چند تب همزمان برای یک نشست
۴۳`IdleTime`Int32 (Minutes)200امنیت فیزیکیمهلت عدم فعالیت کاربر پیش از قفل خودکار به دقیقه
۴۴`IsLoginPersistent`Booleantrueتجربه کاربریپایداری کوکی ورود با بستن و باز کردن مرورگر
۴۵`EnableSingleUserModeLogin`Booleanfalseامنیت اکانتممنوعیت لاگین همزمان با یک حساب از چند دستگاه مختلف
۴۶`LoginTimeoutHours`Int32 (Hours)48احراز هویتطول عمر کوکی احراز هویت OWIN ApplicationCookie
۴۷`ShowCaptcha`Booleanfalseضد Brute-Forceفعال‌سازی تصویر امنیتی کپچا در صفحه ورود به سامانه
۴۸`PasswordDifficultyScore`EnumStrongالزام افتاحداقل دشواری رمز عبور (Weak, Medium, Strong...)
۴۹`SSOTehranWebConfigSettingEnabled`Booleanfalseسوییچ منبعسوییچ خواندن تنظیمات SSO تهران از Web.config به جای DB
۵۰`AppClientID_tehran`Credentialخالیاحراز هویتشناسه کلاینت سامانه در سرور SSO تهران
۵۱`AppSecretKey_tehran`Credentialخالیاحراز هویترمز محرمانه کلاینت در سرور SSO تهران
۵۲`Authority_URL_Tehran`URLخالیاحراز هویتآدرس سرور احراز هویت مرکزی SSO تهران
۵۳`Redirect_URL_Tehran`URLخالیاحراز هویتنشانی ریدایرکت بازگشتی سامانه پس از لاگین در SSO
۵۴`RateLimitEndpoints`Delimited / JSON۳۰ رول پیش‌فرضبحرانی (DDoS)کلید یگانه کنترل نرخ درخواست‌ها و سهمیه‌بندی اندپوینت‌ها
۵۵`RateLimitEndpointsJson`JSON Arrayخالیمکمل سهمیهکلید اختیاری مکمل جهت تعریف قوانین به فرمت JSON
۵۶`ScriptTask:EnableCSharpLanguage`Booleantrueامنیت فرآیندمجوز اجرای کدهای زبان C# در تسک اسکریپت BPMN
۵۷`ScriptTask:MaximumTimeout`Int16 (Seconds)90کنترل منبعسقف مهلت ثانیه‌ای اجرای تسک‌های اسکریپتی فرآیند
۵۸`ScriptTask:MaximumStatements`Int3220000ضد لوپ بی‌نهایتحداکثر دستورات مجاز در اسکریپت برای پیشگیری از قفل سرور
۵۹`EnableTimer`Booleanfalseوب‌فارم حساسراه‌اندازی زمان‌بند Quartz.NET رویدادهای تایمر فرآیندی
۶۰`DatabaseProvider:CommandTimeoutSeconds`Int32 (Seconds)0 (نامحدود)پایداری دیتابیستایم‌اوت اجرای کوئری در ارائه‌دهندگان داده
۶۱`ProviderLimitations`DSL Filter* (همه)امنیت دیتابیسمحدودسازی اشیاء و عملیات مجاز پایگاه‌داده در Provider
۶۲`RestServiceAllowedHosts`CommaList* (همه)الزام افتا (SSRF)لیست سفید دامنه‌های مجاز وب‌سرویس REST Provider
۶۳`WsdlServiceAllowedHosts`CommaList* (همه)الزام افتا (SSRF)لیست سفید سرویس‌های وب SOAP/WSDL Provider
۶۴`AjaxScriptTimeOut`Int32 (ms)1000000تایم‌اوت شبکهمهلت پاسخگویی اسکریپت‌های آژاکس معمولی در کلاینت
۶۵`LargeDataAjaxScriptTimeOut`Int32 (ms)180000تایم‌اوت شبکهمهلت پاسخگویی تبادلات داده‌ای و گزارشات سنگین آژاکس
۶۶`WebApiRequestTimeOut`Int32 (ms)600000تایم‌اوت شبکهتایم‌اوت شیء HttpClient در فراخوانی‌های برون‌سازمانی
۶۷`MaximumLogRecordCount`Int322147483640مدیریت لاگسقف مجاز بازیابی رکوردهای لاگ در مانیتورینگ سیستم
۶۸`SaveControllerLog`Booleantrueممیزی سیستمثبت تاریخچه فراخوانی اکشن‌های کنترلرهای وب در لاگ
۶۹`SaveSelectLog`Booleantrueبحرانی کاراییثبت کوئری‌های SELECT (توصیه پروداکشن: false)
۷۰`SaveInsertLog`Booleantrueممیزی دادهثبت ممیزی عملیات درج در جدول SystemLogAsync
۷۱`SaveUpdateLog`Booleantrueممیزی دادهثبت ممیزی عملیات ویرایش رکوردها در لاگ دیتابیس
۷۲`SaveDeleteLog`Booleantrueممیزی دادهثبت ممیزی عملیات حذف رکوردها جهت کشف تخلفات
۷۳`ShouldLogAgent`Booleantrueاطلاعات کلاینتثبت هدر User-Agent مرورگر کلاینت در رکوردهای لاگ
۷۴`isProviderParamLogActive`Booleanfalseدیباگ منبع دادهثبت لاگ پارامترهای ارسالی به پروسیجرها و منابع داده
۷۵`isNotificationLogActive`Booleantrueممیزی اعلانثبت تاریخچه وقایع ارسال اعلان‌ها و وب‌پوش‌ها
۷۶`LogVerbosityLevel`Enum3 (AllInners)افشای اطلاعاتسطح جزئیات خطاهای صادرشده به کاربران و کلاینت‌ها
۷۷`DbDurationLogs`Booleanfalseپروفایلر کوئریردگیری فرامین کند دیتابیس در جدول dbo.DbDurationLogs
۷۸`isCacheLogActive`Booleanfalseدیباگ کشثبت وقایع ثبت و ابطال آیتم‌ها در کش حافظه سیستم
۷۹`EnableNotification`Booleantrueسوییچ سراسریکلید کلان فعال‌سازی ماژول اعلان‌های درون‌برنامه‌ای
۸۰`Push:HeartbeatSeconds`Int32 (Seconds)15نگه‌دارنده سوکتفاصله سیگنال‌های پینگ نگه‌دارنده اتصال زنده SSE
۸۱`Push:WebPushEnabled`Booleantrueنوتیفیکیشن مرورگرارسال اعلان‌های وب‌پوش استاندارد مرورگر (VAPID)
۸۲`Push:SSEEnabled`Booleantrueاستریم رویدادفعال‌سازی Server-Sent Events برای پیام‌های آنی کارتابل
۸۳`Push:SendLegacySubscriptionsWhenSseDelivered`Booleanfalseارسال مجددارسال وب‌پوش حتی پس از دریافت موفق در کانال زنده SSE
۸۴`Push:MaxTabsPerBrowser`Int3212کنترل منبع سوکتسقف تب‌های متصل به کانال SSE در یک مرورگر کلاینت
۸۵`Push:MaxTabsPerUser`Int3248کنترل منبع سوکتسقف کل تب‌های متصل یک کاربر در تمام کامپیوترها/گوشی‌ها
۸۶`Push:WebPushFailureThreshold`Int3210مدارشکن (Breaker)آستانه شکست متوالی وب‌پوش پیش از تعلیق موقت ارسال
۸۷`Push:WebPushCooldownSeconds`Int32 (Seconds)120تنفس مدارشکندوره تنفس مدارشکن در شرایط قطعی اینترنت سرور
۸۸`Push:RoleParallelism`Int328موازی‌سازی تردهاتعداد تردهای همزمان در دیسپچ نوتیفیکیشن به اعضای نقش
۸۹`Push:SseActiveTabOnly`Booleanfalseبهینه‌سازی کلاینتارسال رویداد SSE فقط به تبی که کاربر روی آن فوکوس دارد
۹۰`BpmsVersionFiles`VersionStr2.14.128الزامی در انتشارشماره نسخه دارایی‌های وب جهت ابطال فوری کش کلاینت‌ها
۹۱`DirectoryJS`VirtualPath~/ActiveQuery...مسیر ماژولمسیر اسکریپت‌های ویژوال کوئری‌بیلدر
۹۲`DirectoryCSS`VirtualPath~/ActiveQuery...مسیر ماژولمسیر استایل‌های ویژوال کوئری‌بیلدر
۹۳`HttpCompressionEnabled`Booleanfalseفشرده‌سازیفشرده‌سازی در لایه اپلیکیشن (معمولاً در IIS فعال می‌شود)
۹۴`MultipleQueriesPerSession`Booleantrueکوئری‌بیلدراجازه اجرای همزمان چند کوئری در یک سشن دیزاینر
۹۵`ShouldUseMinifyBundle`Booleantrueسرعت لود صفحهاستفاده از فایل‌های فشرده و باندل‌شده JS/CSS در صفحات
۹۶`CacheSlidingExpirationInterval`Int32 (Hours)24کش حافظهبازه انقضای شناور آیتم‌های کش هویتی به ساعت
۹۷`CacheAbsoluteExpirationInterval`Int32 (Hours)48کش حافظهسقف مطلق و قطعی انقضای آیتم‌های کش به ساعت
۹۸`AndroidSDKPath`PhysicalPathخالیسرور بیلد APKمسیر نصب Android SDK جهت کامپایل خودکار اپلیکیشن
۹۹`AllowedMobilesSoftwares`Selector* (همه)کلاینت موبایلکلاینت‌های مجاز موبایل (*، APK بومی یا PWA)
۱۰۰`Sync:RequireCentralOrganization`Booleantrueامنیت همگام‌سازیمحدودسازی فرآیند Sync فقط به کاربران سازمان مرکزی
۱۰۱`Sync:SkipMissingPermissionOperations`Booleantrueپایداری Syncنادیده گرفتن دسترسی‌های ناشناخته هنگام ایمپورت پرمیشن
۱۰۲`Sync:ImportTransactionTimeoutMinutes`Int32 (Minutes)15تایم‌اوت Syncسقف زمان تراکنش ایمپورت پکیج‌های فرآیندی در محیط مقصد
۱۰۳`secureSettings / JwtSecret`SecretKeyخالیرمزنگاری کلانکلید امضای متقارن توکن‌های احراز هویت JWT

### شناسنامه فنی و راهنمای عملیاتی کلیدها

در این بخش هر گروه از کلیدها همراه با رفتار در کد، نمونه پیکربندی XML، مقایسه مقادیر پروداکشن در برابر توسعه و خطاهای محتمل شرح داده شده است.

### زیرساخت زمان اجرا و هسته پردازش وب (Core Runtime و OWIN)

aspnet:UseTaskFriendlySynchronizationContext

**نوع:** Boolean | **حساسیت:** بحرانی | **پیش‌فرض:** `true` | **مقادیر معتبر:** `true` یا `false`

این تنظیم به موتور زمان اجرای ASP.NET فرمان می‌دهد از کانتکست جدید و سازگار با وظایف (Task-Friendly SynchronizationContext) استفاده کند و ریسه‌های ThreadPool را هنگام دستورات I/O ناهمگام آزاد می‌کند (پلتفرم مبنا: .NET 4.5+ Async Pipeline). در GraphBPMS که تراکنش‌های دیتابیس با Dapper Async و ارتباطات میدل‌ویرها و سرویس‌های خارجی با `async/await` توسعه یافته‌اند، این مقدار الزامی است. عدم تنظیم آن روی `true` منجر به قفل شدن متقابل ریسه‌ها (Deadlock) و قفل شدن کل پروسه IIS می‌شود.

```
<!-- توصیه قطعی: همیشه true در تمامی سرورها -->
<add key="aspnet:UseTaskFriendlySynchronizationContext" value="true" />
```

aspnet:MaxJsonLength و aspnet:MaxJsonDeserializerMembers

**نوع:** Int32 | **حساسیت:** متوسط | **پیش‌فرض:** `2147483647` (حداکثر سقف مجاز دات‌نت) | **پیش‌فرض اولیه دات‌نت:** ۱۰۲,۴۰۰ کاراکتر برای MaximumJsonLength و ۱,۰۰۰ عضو برای Members

خطای ناشی از کمبود مقدار: `Error during serialization or deserialization using the JSON JavaScriptSerializer`

مدل‌های بزرگ فرآیندی با صدها فیلد، جداول اطلاعاتی تودرتو و خروجی‌های حجیم وب‌سرویس‌ها نیازمند دیسریالایز مقادیر بزرگ JSON هستند. قرار دادن هر دو متغیر روی `2147483647` تضمین می‌کند هیچ فرم الکترونیکی پیچیده‌ای به دلیل محدودیت بایت یا تعداد فیلدهای مدل شکسته نشود.

```
<add key="aspnet:MaxJsonLength" value="2147483647" />
<add key="aspnet:MaxJsonDeserializerMembers" value="2147483647" />
```

aspnet:RequestQueueLimitPerSession

**نوع:** Int32 | **حساسیت:** متوسط

به‌صورت پیش‌فرض در IIS هر نشست (Session) تنها اجازه صف‌بندی همزمان ۵۰ درخواست را دارد و با درخواست ۵۱ام، سرور خطای `503 Service Unavailable` صادر می‌کند. در داشبوردهای BPMS که چندین ویجت، چارت و گرید به‌صورت موازی با آژاکس واکشی می‌شوند و کاربر چند تب را همزمان باز می‌کند، این سقف سریع پر می‌شد. با تنظیم این کلید روی سقف حداکثری، این محدودیت مرتفع می‌شود.

```
<add key="aspnet:RequestQueueLimitPerSession" value="2147483647" />
```

تنظیمات هسته صفحات وب و استارت‌آپ امنیتی OWIN

**حساسیت:** بحرانی

- `owin:AutomaticAppStartup`: با مقدار `true` کلاس `Startup.cs` را به‌عنوان نقطه ورود OWIN ثبت می‌کند. این کلاس مسئول ثبت میدل‌ویرهای احراز هویت، کوکی، توکن خصوصی و میدل‌ویر ریت‌لیمیت است. هرگز نباید `false` شود.

- `webpages:Enabled`: روی `false` تنظیم شده تا دسترسی مستقیم و فراخوانی فایل‌های نمای `.cshtml` بدون عبور از فیلترهای امنیتی کنترلر مسدود شود.

- `autoFormsAuthentication` و `enableSimpleMembership`: هر دو `false` هستند تا ماژول‌های قدیمی دات‌نت مانع عملکرد صحیح OWIN Cookie Authentication نشوند.

- `ClientValidationEnabled` و `UnobtrusiveJavaScriptEnabled`: هر دو `true` جهت فعال‌سازی اعتبارسنجی سمت کاربر با کدهای کم‌حجم HTML5.

```
<add key="owin:AutomaticAppStartup" value="true" />
<add key="webpages:Version" value="3.0.0.0" />
<add key="webpages:Enabled" value="false" />
<add key="PreserveLoginUrl" value="true" />
<add key="ClientValidationEnabled" value="true" />
<add key="UnobtrusiveJavaScriptEnabled" value="true" />
<add key="autoFormsAuthentication" value="false" />
<add key="enableSimpleMembership" value="false" />
```

### مدیریت فایل‌ها، مسیرهای استوریج و ذخیره‌سازی ابری MinIO

مسیرهای ذخیره‌سازی محلی فایل‌ها بر روی دیسک

**نوع:** Virtual Path | **حساسیت:** متوسط

این متغیرها ساختار پوشه‌های ذخیره‌سازی را در ریشه وب‌سایت تعیین می‌کنند. در استقرار سامانه‌های سازمانی، این پوشه‌ها می‌توانند روی دیسک‌های SAN/NAS یا به‌اشتراک‌گذاشته‌شده با پروتکل SMB نگاشت شوند:

نام کلیدمقدار استانداردشرح محتوا و تمهیدات دیسک

`TempFilePath``/Files/Temp/`مسیر نگهداری موقت اسناد در جریان آپلود قطعه‌ای، گزارشات اکسل و پی‌دی‌اف‌های موقت (نیازمند مجوز Full Modify)
`UserFilePath``/Files/Users/`تصاویر پروفایل کاربران، امضاهای رسمی و اسناد هویتی پرسنلی
`NewsFilePath``/Files/News/`فایل‌ها، ویدیوها و تصاویر بارگذاری‌شده در ماژول پورتال اخبار
`ImagesGalleryFilePath``/Files/ImagesGallery/`گالری گرافیکی فرم‌ساز و آیکون‌های استفاده‌شده در دیاگرام فرآیندها
`DynamicPagesPath``/Files/Temp/`مسیر نگهداری کدهای کامپایل‌شده فرم‌های داینامیک

```
<add key="TempFilePath" value="/Files/Temp/" />
<add key="UserFilePath" value="/Files/Users/" />
<add key="NewsFilePath" value="/Files/News/" />
<add key="ImagesGalleryFilePath" value="/Files/ImagesGallery/" />
<add key="DynamicPagesPath" value="/Files/Temp/" />
```

ArchiveNotAllowedFileTypes (ممیزی عمیق فایل‌های آپلودی)

**نوع:** فهرست جداشده با کاما | **حساسیت:** الزام افتا

یکی از رایج‌ترین بردارهای نفوذ به سرورهای وب، آپلود وب‌شل‌ها یا بدافزارهای اجرایی در پوشه‌های استوریج است. کلاس `ArchiveFileValidator.cs` این تنظیم را خوانده و فایل را با دو لایه امنیتی می‌سنجد:

- **بررسی پسوند فایل:** مقایسه با لیست سیاه پسوندهای غیرمجاز.

- **ممیزی امضای بایت‌های جادویی (Magic Bytes Inspection):** حتی اگر کاربر نام فایل مخرب را به `invoice.pdf` تغییر دهد، سیستم دو بایت اول فایل را می‌خواند؛ در صورت وجود امضای `0x4D, 0x5A` (شناسه MZ فایل‌های اجرایی دات‌نت و ویندوز PE)، تگ‌های وب‌سرور `<%@ Page` یا ساختارهای Batch، فایل مسدود شده و خطای امنیتی ثبت می‌شود.

```
<!-- توصیه مرکز افتا برای محیط‌های عملیاتی -->
<add key="ArchiveNotAllowedFileTypes" value="exe,msi,dll,scr,sys,cpl,ocx,bat,cmd,aspx,ashx,asmx,vbs,ps1,sh,jsp,cgi" />
```

تنظیمات استریم فایل‌های حجیم و رمزنگاری استوریج

**حساسیت:** متوسط

- `largeFileMinLength`: آستانه بایت تفکیک فایل‌های حجیم (پیش‌فرض: `1000000000` بایت، حدود ۱ گیگابایت). اسناد بالاتر از این حجم در بافر رم لود نمی‌شوند و مستقیماً از طریق استریم بافرشده روی هارد دیسک خوانده یا نوشته می‌شوند تا از خطای OutOfMemoryException جلوگیری شود.

- `EncryptFilesEnabled`: خط‌مشی عمومی رمزنگاری متقارن فایل‌ها در استوریج (پیش‌فرض: `false`). در سازمان‌های با محرمانگی بالا با تنظیم روی `true`، فایل‌ها قبل از ذخیره روی هارد دیسک با الگوریتم متقارن رمز می‌شوند.

- `AllowAnonymousReadFileServices` و `AllowAnonymousWriteFileServices`: پیش‌فرض هر دو `false` است و فراخوانی ناشناس سرویس‌های بایگانی را مسدود می‌کند.

```
<add key="largeFileMinLength" value="1000000000" />
<add key="EncryptFilesEnabled" value="false" />
<add key="AllowAnonymousReadFileServices" value="false" />
<add key="AllowAnonymousWriteFileServices" value="false" />
```

پیکربندی کلاستر ذخیره‌سازی ابری MinIO (سازگار با S3)

**حساسیت:** بحرانی

در زیرساخت‌های نوین سازمانی، فایل‌ها به‌جای ذخیره روی دیسک محلی وب‌سرور، درون کلاسترهای آبجکت استوریج MinIO ذخیره می‌شوند:

نام کلیدنوع دادهشرح تنظیم و نمونه مقدار

`ArchiveHelper:MinIoEndpoint`Stringآدرس سرور یا کلاستر لودبالانس MinIO همراه پورت (مثال: `s3.organization.ir:9000`)
`ArchiveHelper:MinIoAccessKey`Stringشناسه دسترسی یا Access Key تولیدشده در کنسول مدیریتی MinIO
`ArchiveHelper:MinIoSecretKey`Stringرمز عبور محرمانه (Secret Key) متناظر با Access Key
`ArchiveHelper:MinIoWithSSL`Booleanاستفاده از پروتکل امن HTTPS (توصیه: `true`)
`ArchiveHelper:MinIoIsSSLSecure`Booleanاعتبارسنجی سرتیفیکیت SSL (در صورت استفاده از گواهی سازمانی داخلی مقدار `false` بگذارید)
`ArchiveHelper:MinIoDefaultRequestHeadersExpectContinue`Booleanارسال هدر Expect: 100-continue (پیش‌فرض: `false`)

```
<!-- نمونه پیکربندی کلاستر MinIO سازمان -->
<add key="ArchiveHelper:MinIoEndpoint" value="minio-cluster.intranet.ir:9000" />
<add key="ArchiveHelper:MinIoAccessKey" value="YOUR_ACCESS_KEY" />
<add key="ArchiveHelper:MinIoSecretKey" value="YOUR_SECRET_KEY" />
<add key="ArchiveHelper:MinIoWithSSL" value="true" />
<add key="ArchiveHelper:MinIoIsSSLSecure" value="true" />
<add key="ArchiveHelper:MinIoDefaultRequestHeadersExpectContinue" value="false" />
```

### امنیت شبکه، پروتکل‌های رمزنگاری و هدرهای افتا (AFTA Compliance)

SystemSecurityProtocols و مدیریت ارتباطات خروجی SSL/TLS

**نوع:** Network Protocol | **حساسیت:** بحرانی (Security)

- **مقدار پیش‌فرض:** `Tls,Tls11,Tls12,Tls13,Ssl3,SystemDefault`

- **توصیه سخت‌سازی پروداکشن:** `Tls12,Tls13` (حذف پروتکل‌های ضعیف SSL3 و TLS 1.0)

- **ویژگی اختصاصی کلیدواژه none:** غیرفعال‌سازی بررسی گواهی SSL (Bypass Callback)

در کلاس `SecurityProtocolHelper.cs`، اگر رشته این تنظیم حاوی کلمه `none` باشد (مثلاً `Tls12,none`)، سیستم متد `ServerCertificateValidationCallback` را طوری تنظیم می‌کند که تمام گواهی‌های SSL (حتی نامعتبر، منقضی‌شده یا Self-Signed) را تایید کند. این ویژگی فقط برای محیط‌های توسعه و تست داخلی طراحی شده و در سرور پروداکشن نباید استفاده شود.

```
<!-- تنظیم استاندارد و امن برای سرورهای عملیاتی سازمان -->
<add key="SystemSecurityProtocols" value="Tls12,Tls13" />

<!-- عدم ایجاد خطای SSL برای درخواست‌های لوکال تایمر به خود سرور -->
<add key="IgnoreSystemSecurityProtocolsForSelfRequests" value="true" />
```

هدرهای استاندارد امنیتی پاسخ HTTP (الزامات مرکز مدیریت راهبردی افتا)

**حساسیت:** الزام ممیزی. این هدرها در متد `Afta_headers` در فایل `Global.asax.cs` خوانده شده و به تمام خروجی‌های وب‌سایت اضافه می‌شوند:

نام کلید در وب‌کانفیگمقدار استانداردهدر HTTP تولیدشدههدف دفاعی در برابر حملات سایبری

`Global_X_FRAME_Setting``SAMEORIGIN``X-Frame-Options`جلوگیری از بارگذاری صفحات درون iframe در سایت‌های ناشناس (ضد حمله Clickjacking)
`X_XSS_Protection``1; mode=block``X-XSS-Protection`دستور به مرورگر برای مسدودسازی صفحه در صورت کشف کدهای تزریقی XSS
`Content_Security_Policy``object-src 'none';``Content-Security-Policy`مسدودسازی بارگذاری افزونه‌های مخرب، جاوا اپلت و فلش
`Referrer-Policy``no-referrer``Referrer-Policy`عدم ارسال اطلاعات حساس موجود در URL به سرورهای مقصد هنگام کلیک روی لینک‌ها
`X_Content_Type_Options``nosniff``X-Content-Type-Options`اجبار مرورگر به پیروی از MIME-Type اعلامی سرور و عدم حدس نوع محتوا

```
<add key="Global_X_FRAME_Setting" value="SAMEORIGIN" />
<add key="X_XSS_Protection" value="1; mode=block" />
<add key="Content_Security_Policy" value="object-src 'none';" />
<add key="Referrer-Policy" value="no-referrer" />
<add key="X_Content_Type_Options" value="nosniff" />
```

بهینه‌سازی کارایی شبکه و ارتباطات خروجی سوکت

**حساسیت:** بحرانی کارایی

- `DefaultConnectionLimit`: در .NET Framework مقدار پیش‌فرض سقف اتصالات خروجی برای برنامه‌های غیر وب تنها ۲ اتصال همزمان است که برای فراخوانی وب‌سرویس‌های سازمانی بسیار پایین است و صف‌های طویل تاخیر ایجاد می‌کند. با مقدار `200` تا `1000`، ظرفیت همزمانی سرور افزایش می‌یابد.

- `Expect100Continue`: با مقدار `false`، مرحله اضافی ارسال هدر انتظاری حذف شده و بدنه درخواست بلافاصله به وب‌سرویس‌های بیرونی ارسال می‌شود که لیتنسی شبکه را کاهش می‌دهد.

- `UseNagleAlgorithm`: مقدار `false` الگوریتم بافرینگ بسته TCP نِیگل را خاموش می‌کند تا پیام‌های سبک بلافاصله در شبکه ارسال شوند.

```
<add key="DefaultConnectionLimit" value="200" />
<add key="Expect100Continue" value="false" />
<add key="UseNagleAlgorithm" value="false" />
```

TrustedProxies و کنترل دسترسی فرم‌های ناشناس

**نوع:** IP Security | **حساسیت:** بحرانی

- `TrustedProxies`: لیست IP لودبالانسرها، Reverse Proxyها و WAFهای معتبر سازمان (مانند Nginx، HAProxy، F5 یا Cloudflare) جداشده با کاما. اگر درخواست از این آدرس‌ها وارد شود، آخرین IP غیر پروکسی در هدر `X-Forwarded-For` استخراج می‌شود؛ در غیر این صورت هدر نادیده گرفته می‌شود تا مهاجم نتواند IP واقعی خود را جعل کند.

- `AnonymousFormAccess`: لیست سفید IPهای مجاز برای ثبت فرم‌های ناشناس فرآیندی. مقدار `*` دسترسی عمومی را آزاد می‌گذارد، اما می‌توان آن را به رنج داخلی سازمان محدود کرد.

- `CustomJsDenyRegexes`: الگوهای منظم جداشده با ` or ` برای شناسایی و مسدودسازی کدهای مخرب نظیر `fetch(`، `$.ajax(` و `XMLHttpRequest` در فرم‌های پویا.

- `secureSettings / JwtSecret`: کلید امضای متقارن توکن‌های احراز هویت JWT (حداقل ۳۲ کاراکتر پیچیده تصادفی).

```
<!-- نمونه در استقرار پشت لودبالانسر Nginx یا F5 -->
<add key="TrustedProxies" value="192.168.10.1,192.168.10.2,10.0.0.1" />
<add key="AnonymousFormAccess" value="*" />

<!-- در تگ secureSettings -->
<secureSettings>
    <add key="JwtSecret" value="YOUR_RANDOM_SECRET_MIN_32_CHARS" />
</secureSettings>
```

### احراز هویت، کنترل نشست‌ها و مدیریت کاربران

کنترل نشست‌ها، ورود همزمان و زمان انقضای کوکی

**حساسیت:** متوسط

نام کلیدنوع دادهپیش‌فرضشرح جامع رفتار در سامانه

`LoginTimeoutHours`Int3248طول عمر کوکی احراز هویت OWIN ApplicationCookie بر حسب ساعت. پس از این مدت کاربر موظف به ورود مجدد است.
`IdleTime`Int32200مدت زمان عدم فعالیت به دقیقه؛ در صورت بی‌استفاده ماندن صفحه، سیستم کاربر را قفل یا لاگ‌اوت می‌کند.
`SessionMultiple`Booleantrueاجازه باز کردن چند تب همزمان در یک مرورگر برای انجام امور کارتابل.
`IsLoginPersistent`Booleantrueماندگاری کوکی لاگین در صورت بستن و باز کردن مجدد مرورگر.
`EnableSingleUserModeLogin`Booleanfalse**ممنوعیت ورود همزمان با یک اکانت:** در صورت `true` بودن، اگر کاربری در سیستم لاگین باشد، ورود مجدد با همان نام کاربری از سیستم یا مرورگر دیگر مسدود شده یا نشست قبلی باطل می‌شود.
`ShowCaptcha`Booleanfalseفعال‌سازی تصویر امنیتی (کپچا) در فرم ورود جهت پیشگیری از حملات خودکار حدس کلمه عبور.
`PasswordDifficultyScore`EnumStrongحداقل دشواری رمز عبور (مقادیر: `Blank`، `Weak`، `Medium`، `Strong`، `VeryStrong`). در پروداکشن مقدار `Strong` الزامی است.

```
<add key="LoginTimeoutHours" value="48" />
<add key="IdleTime" value="200" />
<add key="SessionMultiple" value="true" />
<add key="IsLoginPersistent" value="true" />
<add key="EnableSingleUserModeLogin" value="false" />
<add key="ShowCaptcha" value="false" />
<add key="PasswordDifficultyScore" value="Strong" />
```

تنظیمات سامانه احراز هویت متمرکز (SSO) شهرداری تهران

**نوع:** SSO / OAuth2 | **حساسیت:** بحرانی

در پروژه‌هایی که ورود کاربران از طریق درگاه احراز هویت شهرداری تهران انجام می‌شود، کلیدهای زیر مشخصات پروتکل OAuth2 را هدایت می‌کنند:

- `SSOTehranWebConfigSettingEnabled`: اگر `true` باشد، اطلاعات از این کلیدها خوانده می‌شود؛ در غیر این صورت از جدول `SystemConfiguration` واکشی می‌شود.

- `AppClientID_tehran`: شناسه ثبت‌شده اپلیکیشن در IdentityServer تهران.

- `AppSecretKey_tehran`: رمز عبور محرمانه کلاینت.

- `Authority_URL_Tehran`: آدرس سرور اصلی صدور توکن SSO.

- `Redirect_URL_Tehran`: نشانی بازگشتی ریدایرکت سامانه پس از ورود موفق.

```
<add key="SSOTehranWebConfigSettingEnabled" value="false" />
<add key="AppClientID_tehran" value="YOUR_CLIENT_ID" />
<add key="AppSecretKey_tehran" value="YOUR_CLIENT_SECRET" />
<add key="Authority_URL_Tehran" value="https://sso.tehran.ir" />
<add key="Redirect_URL_Tehran" value="https://bpms.tehran.ir/Login/SSOCallback" />
```

### کنترل نرخ ترافیک و سهمیه‌بندی (Rate Limiting Engine)

RateLimitEndpoints (کلید یگانه سهمیه‌بندی در Web.config)

**نوع:** Delimited / JSON | **حساسیت:** بحرانی (Security و Performance)

**نکته مهم:** در `Web.config` منحصراً همین یک کلید به نام `RateLimitEndpoints` وجود دارد و هیچ کلید اضافی مانند `RateLimitEnabled` نباید درج شود. شرح کامل در بخش «[محدودیت نرخ درخواست (Rate Limiting)](#rate-limit)» همین صفحه آمده است.

این کلید سقف درخواست‌های مجاز بر حسب زمان را برای اندپوینت‌های سامانه مدیریت می‌کند. فرمت رشته‌ای هر رول:

```
[METHOD ]PATH:LIMIT:WINDOW_IN_SECONDS[:LIMIT_BY_USER[:MESSAGE[:CONDITION]]]
```

خطوط با کاراکتر `#` کامنت تلقی شده و نادیده گرفته می‌شوند. قوانین با کاراکتر `;` یا شکست خط از هم تفکیک می‌شوند.

```
<add key="RateLimitEndpoints" value="
    # ورود به سیستم: حداکثر ۵ تلاش در ۶۰ ثانیه به ازای هر IP
    POST /Login/Authenticate:5:60:false:تعداد ورود بیش از حد مجاز است. لطفاً {0} ثانیه دیگر تلاش فرمایید.;

    # ثبت فرم ناشناس: ۱۰ درخواست در ۶۰ ثانیه
    /AnonymousForm/Submit:10:60:false;

    # کارتابل پرسنلی: ۶۰ درخواست در ۶۰ ثانیه به ازای هر کاربر واردشده
    /Process/SubmitContinueProcess:60:60:true;

    # وب‌سرویس عمومی: ۱۲۰ درخواست در ۶۰ ثانیه
    /SharedApi/RunProcess:120:60:false;

    # اندپوینت شرطی با پیام اختصاصی
    POST /SharedApi/ThrowProcessSignal:5:60:false:ارسال سیگنال‌های حیاتی محدود است:Content.IsUrgent == true;
    POST /SharedApi/ThrowProcessSignal:60:60:false
"/>
```

### موتور فرآیند، اسکریپت‌ها و زمان‌بند پویا (BPMN Script Task و Timer)

موتور تسک‌های اسکریپتی (Script Tasks) و زمان‌بند Quartz.NET

**حساسیت:** بحرانی

- `ScriptTask:EnableCSharpLanguage`: مجوز اجرای زبان C# در تسک اسکریپتی فرآیند توسط موتور کامپایلر Roslyn دات‌نت (پیش‌فرض: `true`). در صورت تنظیم روی `false`، فقط زبان جاوااسکریپت اجازه اجرا دارد.

- `ScriptTask:MaximumTimeout`: سقف مجاز زمان اجرای اسکریپت بر حسب ثانیه (پیش‌فرض: `90`). مانع از قفل شدن ریسه به دلیل محاسبات سنگین کدهای کاربر می‌شود.

- `ScriptTask:MaximumStatements`: حداکثر تعداد دستورات و تکرار حلقه‌های مجاز در اسکریپت (پیش‌فرض: `20000`). مانع از حلقه‌های بی‌نهایت ناخواسته می‌شود.

- `EnableTimer`: راه‌اندازی سرویس زمان‌بندی پویا (Quartz.NET). **هشدار وب‌فارم:** در کلاسترهای چند سروری، این کلید فقط روی یک سرور باید `true` باشد تا از اجرای موازی و تکراری تایمرهای فرآیندی جلوگیری شود.

```
<add key="ScriptTask:EnableCSharpLanguage" value="true" />
<add key="ScriptTask:MaximumTimeout" value="90" />
<add key="ScriptTask:MaximumStatements" value="20000" />

<!-- فعال‌سازی زمان‌بند فرآیندها -->
<add key="EnableTimer" value="true" />
```

### ارائه‌دهندگان داده و لیست‌های سفید وب‌سرویس (SSRF Whitelisting)

کنترل دسترسی دیتابیس و لیست‌های سفید REST و SOAP

**نوع:** Provider Security | **حساسیت:** الزام افتا

- `ProviderLimitations`: فیلتر کردن اشیاء پایگاه‌داده در ماژول منبع داده. مقادیر مجاز شامل `*` (همه)، `MaximumLimitation` (مسدودسازی کلیه جداول زیرساختی سامانه) و الگوهای سفارشی نظیر `Operation_SqlDeleteStatement` است.

- `RestServiceAllowedHosts` و `WsdlServiceAllowedHosts`: لیست سفید دامنه‌ها و هاست‌های مجاز در فراخوانی‌های وب‌سرویس جهت ممانعت از حملات جعل درخواست سمت سرور (SSRF). مقدار `*` دسترسی عمومی را آزاد می‌گذارد، اما در محیط‌های حساس باید با دامنه‌های مشخص پر شود.

- `DatabaseProvider:CommandTimeoutSeconds`: سقف ثانیه‌ای تایم‌اوت اجرای کوئری در منابع داده (مقدار ۰ معادل تایم‌اوت نامحدود است).

```
<add key="ProviderLimitations" value="MaximumLimitation" />
<add key="RestServiceAllowedHosts" value="api.myorg.ir,services.bank.ir,*.gov.ir" />
<add key="WsdlServiceAllowedHosts" value="ws.myorg.ir,*.bank.ir" />
<add key="DatabaseProvider:CommandTimeoutSeconds" value="60" />
```

### مهلت‌های زمانی و تایم‌اوت‌های ارتباطی (Timeouts)

مهلت‌های زمانی ارتباطات کلاینت و کلاینت HTTP

**واحد:** میلی‌ثانیه | **حساسیت:** متوسط

نام کلیدمقدار پیش‌فرضمعادل زمانیتوضیحات و کاربرد

`AjaxScriptTimeOut``1000000`۱۶.۶ دقیقهمهلت زمانی اجرای اسکریپت‌های آژاکس معمولی در مرورگر کلاینت
`LargeDataAjaxScriptTimeOut``180000`۳ دقیقهمهلت فراخوانی‌های داده‌ای سنگین و گزارشات بزرگ آژاکس
`WebApiRequestTimeOut``600000`۱۰ دقیقهتایم‌اوت پایه فراخوانی‌های شیء سراسری HttpClient در ارسال درخواست‌های خروجی سامانه

```
<add key="AjaxScriptTimeOut" value="1000000" />
<add key="LargeDataAjaxScriptTimeOut" value="180000" />
<add key="WebApiRequestTimeOut" value="600000" />
```

### سیستم ممیزی دیتابیس، لاگینگ و سطح نمایش خطاها

تنظیمات ممیزی تراکنش‌ها و ثبت وقایع ORM

**حساسیت:** بحرانی کارایی

نام کلیدپیش‌فرضتوصیه پروداکشنشرح کارکرد و اثر بر دیتابیس

`SaveSelectLog`true**false**ثبت کلیه کوئری‌های SELECT در لاگ. در پروداکشن برای جلوگیری از اشباع دیسک و افت IOPS حتماً خاموش شود.
`SaveInsertLog`truetrueثبت ممیزی عملیات درج رکوردها در جدول SystemLogAsync
`SaveUpdateLog`truetrueثبت ممیزی عملیات به‌روزرسانی داده‌ها در لاگ
`SaveDeleteLog`truetrueثبت ممیزی عملیات حذف داده‌ها جهت ردیابی‌های امنیتی
`SaveControllerLog`truetrueثبت فراخوانی اکشن‌های کنترلرهای سیستم
`ShouldLogAgent`truetrueثبت مشخصات User-Agent دستگاه کلاینت در لاگ
`isProviderParamLogActive`falsefalseثبت جزئیات پارامترهای ارسالی به منابع داده (صرفاً برای دیباگ)
`DbDurationLogs`falsefalseردگیری فرامین کند دیتابیس در جدول `dbo.DbDurationLogs`
`isCacheLogActive`falsefalseثبت وقایع ثبت و ابطال در کش حافظه سیستم
`MaximumLogRecordCount`21474836402147483640سقف مجاز بازیابی رکوردهای لاگ در مانیتورینگ

**سطح نمایش خطاها (LogVerbosityLevel):**

- `JustError` (مقدار ۱): بالاترین امنیت؛ کاربر فقط خطای عمومی می‌بیند (توصیه پروداکشن‌های حساس).

- `JustExceptionMessage` (مقدار ۲): فقط پیام اصلی خطا را نمایش می‌دهد.

- `AllInnerExceptions` (مقدار ۳، پیش‌فرض): کلیه خطاهای لایه‌های زیرین را نمایش می‌دهد.

- `Professional` (مقدار ۴): تمام جزئیات فنی و خطوط کد فایل‌ها را برای دیباگ کامل نمایش می‌دهد.

```
<!-- توصیه سخت‌سازی کارایی و امنیت برای پروداکشن -->
<add key="SaveSelectLog" value="false" />
<add key="SaveInsertLog" value="true" />
<add key="SaveUpdateLog" value="true" />
<add key="SaveDeleteLog" value="true" />
<add key="SaveControllerLog" value="true" />
<add key="ShouldLogAgent" value="true" />
<add key="isProviderParamLogActive" value="false" />
<add key="DbDurationLogs" value="false" />
<add key="LogVerbosityLevel" value="3" />
<add key="MaximumLogRecordCount" value="2147483640" />
```

### سامانه اعلان‌های آنی، استریم SSE و وب‌پوش (Push Architecture)

تنظیمات پیشرفته کانال‌های ارسال اعلان (Push Options)

**حساسیت:** متوسط

نام کلیدپیش‌فرضواحدشرح مکانیزم در سامانه

`EnableNotification`trueBooleanکلید سراسری فعال یا غیرفعال بودن ماژول اعلان‌ها
`Push:SSEEnabled`trueBooleanفعال‌سازی کانال استریم زنده رویدادهای سرور (Server-Sent Events)
`Push:WebPushEnabled`trueBooleanارسال اعلان‌های وب‌پوش استاندارد مرورگر (VAPID) حتی هنگام بسته بودن تب
`Push:HeartbeatSeconds`15ثانیهفاصله زمانی ارسال پینگ‌های نگه‌دارنده اتصال SSE در شبکه
`Push:MaxTabsPerBrowser`12عددسقف تب‌های همزمان متصل در یک مرورگر برای ممانعت از اشباع پورت
`Push:MaxTabsPerUser`48عددسقف کل تب‌های باز یک کاربر در کلیه دستگاه‌ها
`Push:WebPushFailureThreshold`10شکست متوالیحد آستانه Circuit Breaker: در صورت ۱۰ شکست متوالی ارسال به سرویس خارجی متوقف می‌شود
`Push:WebPushCooldownSeconds`120ثانیهدوره تنفس مدارشکن پیش از تلاش مجدد
`Push:RoleParallelism`8Threadدرجه موازی‌سازی تردها در ارسال گروهی اعلان به نقش‌ها
`Push:SseActiveTabOnly`falseBooleanتحویل رویداد فقط به تبی که کاربر روی آن فوکوس دارد
`Push:SendLegacySubscriptionsWhenSseDelivered`falseBooleanارسال پوش به اشتراک‌های قدیمی حتی پس از دریافت موفق در SSE

```
<add key="EnableNotification" value="true" />
<add key="Push:HeartbeatSeconds" value="15" />
<add key="Push:WebPushEnabled" value="true" />
<add key="Push:SSEEnabled" value="true" />
<add key="Push:SendLegacySubscriptionsWhenSseDelivered" value="false" />
<add key="Push:MaxTabsPerBrowser" value="12" />
<add key="Push:MaxTabsPerUser" value="48" />
<add key="Push:WebPushFailureThreshold" value="10" />
<add key="Push:WebPushCooldownSeconds" value="120" />
<add key="Push:RoleParallelism" value="8" />
<add key="Push:SseActiveTabOnly" value="false" />
```

### کش حافظه، کش‌باستینگ دارایی‌ها و باندلینگ فرانت‌اند

ابطال خودکار کش کلاینت‌ها (Cache Busting) و بهینه‌سازی دارایی‌ها

**نوع:** Release Management | **حساسیت:** الزامی در انتشار

- `BpmsVersionFiles`: **مهم‌ترین کلید هنگام انتشار هر نسخه جدید.** با تغییر این شماره نسخه (مثلاً از `2.14.128` به `2.14.129`)، تمام تگ‌های `<script>` و `<link>` با کوئری‌استرینگ جدید (مانند `?v=2.14.129`) رندر می‌شوند و کش مرورگر تمام کاربران آنلاین بدون نیاز به Ctrl+F5 باطل شده و فایل‌های جاوااسکریپت و استایل جدید دانلود می‌شوند.

- `ShouldUseMinifyBundle`: در محیط عملیاتی باید `true` باشد تا فایل‌های کوچک‌شده و باندل بارگذاری شوند و زمان لود صفحه کاهش یابد. در محیط توسعه می‌توان آن را `false` گذاشت تا سورس اصلی اسکریپت‌ها در دسترس باشد.

- `CacheSlidingExpirationInterval`: بازه انقضای شناور کش داده‌های هویتی بر حسب ساعت (پیش‌فرض: `24`).

- `CacheAbsoluteExpirationInterval`: سقف مطلق انقضای کش داده‌های هویتی بر حسب ساعت (پیش‌فرض: `48`).

```
<!-- پس از هر بار انتشار نسخه جدید این مقدار را افزایش دهید -->
<add key="BpmsVersionFiles" value="2.14.129" />

<add key="ShouldUseMinifyBundle" value="true" />
<add key="CacheSlidingExpirationInterval" value="24" />
<add key="CacheAbsoluteExpirationInterval" value="48" />
<add key="DirectoryJS" value="~/ActiveQueryBuilderSource/js/release/" />
<add key="DirectoryCSS" value="~/ActiveQueryBuilderSource/css/" />
<add key="HttpCompressionEnabled" value="false" />
<add key="MultipleQueriesPerSession" value="true" />
```

### محیط موبایل، کامپایل APK و ماژول همگام‌سازی بین‌سیستمی (Sync Module)

تنظیمات کلاینت‌های مجاز موبایل و ماژول همگام‌سازی فرآیندها

**حساسیت:** متوسط

- `AllowedMobilesSoftwares`: تعیین نوع کلاینت‌های موبایلی مجاز به فراخوانی سرویس‌های اشتراکی:

`*` (پیش‌فرض): اجازه همزمان به اپلیکیشن بومی APK و وب‌اپلیکیشن پیش‌رونده PWA.

- `APK`: فقط اپلیکیشن‌های بومی اندروید و iOS.

- `PWA`: فقط وب‌اپلیکیشن پیش‌رونده در مرورگر موبایل.

- `AndroidSDKPath`: مسیر فیزیکی نصب Android SDK روی سرور بیلد (مثال: `C:\Program Files\Android\android-sdk`) جهت امضا و صدور فایل APK بومی سامانه به‌صورت خودکار.

- `Sync:RequireCentralOrganization`: در ماژول همگام‌سازی فرآیندها، با مقدار `true` (پیش‌فرض) فقط کاربر فعال در سازمان مرکزی مجاز به صدور پکیج انتقال داده است.

- `Sync:SkipMissingPermissionOperations`: در زمان ایمپورت دسترسی‌ها در محیط هدف، با مقدار `true` از خطای ناشی از عدم وجود اکشن مربوطه صرف‌نظر می‌شود تا تراکنش متوقف نشود.

- `Sync:ImportTransactionTimeoutMinutes`: سقف دقیقه زمان تراکنش ایمپورت پکیج‌های همگام‌سازی فرآیندها در پایگاه‌داده مقصد.

```
<add key="AllowedMobilesSoftwares" value="*" />
<add key="AndroidSDKPath" value="C:\Program Files\Android\android-sdk" />
<add key="Sync:RequireCentralOrganization" value="true" />
<add key="Sync:SkipMissingPermissionOperations" value="true" />
<add key="Sync:ImportTransactionTimeoutMinutes" value="15" />
```

### راهنمای عملیاتی و چک‌لیست انتشار (Publishing و Deployment Checklist)

تیم‌های استقرار، انتشار و مهندسان DevOps قبل از هرگونه استقرار نهایی در محیط‌های کارفرما باید موارد جدول زیر را ارزیابی و تایید کنند:

عنوان کنترلتنظیم سرور پروداکشنتنظیم سرور تست / توسعهچک‌لیست نهایی

**ابطال کش کاربران**افزایش `BpmsVersionFiles`-شماره نسخه فایل‌ها در وب‌کانفیگ افزایش داده شد.
**فشرده‌سازی باندل‌ها**`ShouldUseMinifyBundle = true``false`بسته‌های فشرده فعال و لود اسکریپت‌ها تست شد.
**لاگ کوئری‌های SELECT**`SaveSelectLog = false``true`جهت جلوگیری از اشباع دیسک، لاگ سلکت خاموش است.
**سطح نمایش خطاها**`LogVerbosityLevel = 1 یا 3``4 (Professional)`جزئیات کد و استک‌ترس به کاربران نمایش داده نمی‌شود.
**زمان‌بند رویدادها (تایمر)**`EnableTimer = true` (فقط روی ۱ نود)`true`در محیط کلاستر، تایمر فقط روی یک سرور فعال است.
**گواهی‌های SSL/TLS**فاقد کلیدواژه `none`مجاز به استفاده از `none`هیچ‌گونه دور زدن اعتبارسنجی گواهی SSL فعال نیست.
**فیلتر فایل‌های خطرناک**شامل لیست کامل پسوندها و امضاهاپیش‌فرضآپلود فایل‌های PE و اسکریپت‌های سیستمی مسدود است.
**سقف سوکت‌های خروجی**`DefaultConnectionLimit = 200``200`گلوگاه خروجی اتصالات شبکه دات‌نت رفع شده است.
**پروکسی‌های مورد اعتماد**ثبت IP لودبالانسر در `TrustedProxies`خالی یا لوکالآی‌پی‌های پروکسی جهت ممانعت از جعل هدر XFF ثبت شد.
**سهمیه‌بندی Rate Limit**کلید `RateLimitEndpoints` فعالفعال یا دیباگ۳۰ اندپوینت حساس سامانه در برابر حملات محافظت شدند.

**جمع‌بندی:** با رعایت دقیق پارامترهای مستندشده در این مرجع، سامانه GraphBPMS در بالاترین سطوح کارایی، امنیت شبکه، انطباق با چک‌لیست‌های مرکز مدیریت راهبردی افتا و پایداری در مقیاس سازمانی عملیاتی خواهد شد.
