---
id: 1002594
title: "راهنمای جامع پیکربندی امن Web.config"
englishTitle: "راهنمای جامع پیکربندی امن Web.config"
category: "09-custom-articles"
wikiPath: "webconfig"
wikiUrl: "https://wiki.graphbpms.ir/webconfig"
breadcrumbs: ["راهنمای جامع پیکربندی امن Web.config"]
priority: 5
isCanonical: true
updatedAt: "2026-11-07T14:21:08"
---

# راهنمای جامع پیکربندی امن Web.config

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `راهنمای جامع پیکربندی امن Web.config`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/webconfig](https://wiki.graphbpms.ir/webconfig)

نسخه 2.14.132 | کامیت 12b9323e
    
# 

# راهنمای جامع پیکربندی امن Web.config، استقرار و ارتقای سامانه GraphBPMS

    

مرجع سخت‌سازی امنیتی سرور، تنظیمات IIS، ماتریس دسترسی پوشه‌های NTFS، کاربر کمترین دسترسی پایگاه داده و تشریح ۹۸ کلید پیکربندی

  

  
  
    

      این سند، راهنمای رسمی و الزامی همراه با بسته انتشار نسخه **2.14.132** سامانه GraphBPMS ویژه کارشناسان استقرار، مدیران سرور، تیم پشتیبانی فنی و ممیزان امنیت سایبری است. این راهنما بر پایه بررسی دقیق و خط‌به‌خط سورس‌کد سامانه تدوین گردیده و کلیه کلیدهای تنظیماتی، مقادیر پیش‌فرض بسته و مقادیر امن عملیاتی (Production) را جهت پیشگیری از نفوذ، ارتقای عملکرد و اطمینان از پایداری تبیین می‌نماید.
    

  

  
  
    **📌 سرفصل‌های این راهنما:**
    

      - [۱. اقدامات الزامی و حیاتی پیش از راه‌اندازی سرور عملیاتی](#mandatory-prelaunch-actions)

      - [۲. پیش‌نیازهای نرم‌افزاری و نقش‌های IIS](#server-prerequisites)

      - [۳. مراحل استقرار گام‌به‌گام](#step-by-step-deployment)

      - [۴. تشریح کلیدهای Web.config به تفکیک حوزه‌های ۱۵گانه](#webconfig-keys-taxonomy)

      - [۵. قالب نمونه Web.config سخت‌سازی‌شده (Production Ready)](#recommended-secure-config)

      - [۶. پایگاه داده: تفکیک دسترسی و جایگزینی کاربر sa](#database-least-privilege)

      - [۷. مدیریت اسرار: تولید کلید تصادفی، JWT_SECRET و رمزنگاری IIS](#secrets-management)

      - [۸. پیکربندی امنیتی پیشرفته IIS (احراز هویت، HSTS و Headers)](#iis-security-configuration)

      - [۹. ساختار پوشه‌ها، مجوزهای NTFS و لغو دسترسی اجرا](#folder-structure-ntfs-acls)

      - [۱۰. مسدودسازی اجرای فایل‌های بارگذاری‌شده و لیست سفید Files/web.config](#files-whitelist-webconfig)

      - [۱۱. چک‌لیست آزمون‌های صحت‌سنجی پس از استقرار](#post-deployment-testing)

      - [۱۲. راهنمای ارتقا از نسخه‌های پیشین و پویش بدافزارها](#rolling-upgrade-guide)

      - [۱۳. جدول اولویت‌بندی اقدامات امنیتی (پیوست نهایی)](#priority-action-matrix)

    

  

  
  
    
## ۱. اقدامات الزامی و حیاتی پیش از راه‌اندازی سرور عملیاتی

    

      ۱۰ مورد زیر بیشترین اثر حفاظتی را دارند و بدون اجرای مو‌به‌موی آن‌ها، سرور سامانه به هیچ عنوان نباید در دسترس کاربران قرار گیرد:
    

    
      

        - **متغیر محیطی JWT_SECRET:** روی Application Pool سرور تعریف شود. کلید `JwtSecret` در فایل Web.config باید خالی بماند تا مقدار پیش‌فرض داخلی برنامه استفاده نشود.

        - **قطع وابستگی به کاربر sa:** سامانه هرگز نباید با حساب مدیر ارشد پایگاه داده (`sa`) اجرا شود؛ یک لاگین اختصاصی با کمترین دسترسی ایجاد و رمز `sa` غیرفعال گردد.

        - **غیرفعال‌سازی اشکال‌زدایی:** ویژگی `compilation debug="false"` و `customErrors mode="On"` تنظیم شوند.

        - **اجبار سراسری پروتکل امن HTTPS:** سایت فقط روی پورت 443 و HTTPS در دسترس باشد، ریدایرکت خودکار فعال و مکانیزم HSTS از طریق IIS تنظیم گردد.

        - **تفکیک دسترسی NTFS ریشه وب:** هویت Application Pool روی ریشه سامانه منحصراً مجوز خواندن و اجرا (RX) داشته باشد و دسترسی نوشتن (Modify) فقط به پوشه‌های ذخیره‌سازی داده محدود گردد.

        - **سلب مجوز اجرای اسکریپت از پوشه‌های بارگذاری:** در کلیه پوشه‌های قابل نوشتن، مجوزهای Script و Execute از بخش Handler Mappings حذف شوند تا اجرای فایل‌های وب‌شل یا پسوند aspx غیرممکن گردد.

        - **قفل‌گذاری ابزارهای اجرایی WSDL:** فایل‌های اجرایی `wsdl.exe` و `SvcUtil.exe` در برابر نوشتن کاملاً قفل (Read-Only) گردند.

        - **محدودسازی جزئیات خطا (LogVerbosityLevel):** مقدار کلید روی عدد 1 تنظیم شود تا متن خطاهای SQL و لاگ‌های حساس به کاربر نمایش داده نشود.

        - **فهرست مجاز سرویس‌های وب (SSRF Hardening):** کلیدهای `RestServiceAllowedHosts` و `WsdlServiceAllowedHosts` از علامت ستاره (`*`) به آدرس‌های دقیق و صریح میزبان‌های مجاز تغییر یابند.

        - **پویش امنیتی پیش از ارتقا:** در هر بار بروزرسانی، هرگز پوشه Files نسخه قبل بدون پویش امنیتی ضداسکریپت به نسخه جدید منتقل نشود.

      

    
  

  
  
    
## ۲. پیش‌نیازهای نرم‌افزاری و نقش‌های IIS

    
    
      
        
          
            مؤلفه
            نیازمندی حداقلی
            نکات فنی و امنیتی
          
        
        
          
            سیستم‌عامل
            Windows Server 2016 یا جدیدتر (ترجیحاً 2019 یا 2022)
            جهت بهره‌مندی از HSTS بومی و لغو هدر Server در IIS 10 (نسخه 1709 به بالا).
          
          
            سرویس‌دهنده وب IIS
            Web Server با ASP.NET 4.8 و قابلیت‌های فشرده‌سازی و فیلترینگ
            نقش‌های مازاد WebDAV، ASP کلاسیک، CGI، مرور دایرکتوری (Directory Browsing) و FTP نباید نصب شوند.
          
          
            دات‌نت فریم‌ورک
            .NET Framework 4.8
            فایل پیکربندی دارای هدف 4.7.2 بوده و بر بستر 4.8 اجرا می‌گردد.
          
          
            پایگاه داده
            Microsoft SQL Server 2016 یا جدیدتر
            فعال‌سازی قابلیت Force Encryption و گواهی معتبر روی سرویس دیتابیس.
          
          
            پروتکل‌های ارتباطی
            TLS 1.2 و TLS 1.3
            پروتکل‌های ناامن SSL 3.0، TLS 1.0 و TLS 1.1 در سیستم‌عامل کاملاً غیرفعال گردند.
          
        
      
    

    
### اسکریپت PowerShell نصب نقش‌های لازم و حذف اجزای مازاد IIS:

    
# نصب بسته‌ها و ویژگی‌های مورد نیاز GraphBPMS
Install-WindowsFeature Web-Server, Web-Default-Doc, Web-Static-Content, Web-Http-Errors, `
Web-Http-Logging, Web-Request-Monitor, Web-Http-Tracing, Web-Stat-Compression, Web-Dyn-Compression, `
Web-Filtering, Web-Net-Ext45, Web-Asp-Net45, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Mgmt-Console

# حذف کامپوننت‌های خطرزا و بلااستفاده
Uninstall-WindowsFeature Web-DAV-Publishing, Web-Dir-Browsing, Web-CGI, Web-ASP, Web-Ftp-Server
    
  

  
  
    
## ۳. مراحل استقرار گام‌به‌گام سامانه

    
    
      
۱**جداسازی مسیر نصب از درایو سیستم‌عامل:** پوشه سایت را در درایوی غیر از C (مانند `E:\GraphBPMS\2.14.132`) مستقر کرده و پوشه داده‌ها و آرشیو را بیرون از ریشه وب بسازید (مانند `E:\GraphBPMSData`).

      
۲**استخراج بسته:** فایل‌های بسته رسمی را در پوشه سایت استخراج نمایید. اطمینان یابید پوشه پیش‌فرض آرشیو فاقد فایل‌های تست و دمو است.

      
۳**پیکربندی Login و رشته اتصال دیتابیس:** کاربری اختصاصی پایگاه داده با کمترین دسترسی ایجاد کرده و در بخش `connectionStrings` وارد کنید.

      
۴**ایجاد Application Pool اختصاصی:** اپ‌پول مجزا با هویت `ApplicationPoolIdentity` ایجاد و متغیر محیطی `JWT_SECRET` را بر آن ست کنید.

      
۵**تنظیم وب‌سایت در IIS:** بایندینگ منحصراً HTTPS با گواهی معتبر اضافه و HSTS فعال شود.

      
۶**سخت‌سازی سطح دسترسی NTFS و IIS:** اجرای اسکریپت‌های حذف ارث‌بری و سلب مجوز Script/Execute از پوشه‌های آپلود.

      
۷**جایگزینی Files/web.config با لیست سفید:** قرار دادن قوانین مجاز پسوندها در تمامی پوشه‌های آپلود فایل.

      
۸**تنظیم نهایی کلیدهای Web.config:** تطبیق مقادیر امنیتی و در صورت نیاز، رمزنگاری بخش‌های حساس با `aspnet_regiis`.

      
۹**راه‌اندازی مجدد و تست:** ریسایکل کردن اپ‌پول و اجرای چک‌لیست تست‌های امنیتی.

      
۱۰**پشتیبان‌گیری از ACL و پیکربندی نهایی:** مستندسازی و ذخیره فایل نسخه پشتیبان دسترسی‌ها.

    
  

  
  
    
## ۴. تشریح کلیدهای Web.config به تفکیک حوزه‌های ۱۵گانه

    

      مهم‌ترین کلیدهای امنیتی که مستقیماً رفتار سرور و نفوذپذیری را کنترل می‌کنند در جدول زیر دسته‌بندی شده‌اند:
    

    
      
        
          
            حوزه / کلید
            مقدار در بسته انتشار
            مقدار امن پیشنهادی (Production)
            کاربرد و علت فنی سخت‌سازی
          
        
        
          
          
            JwtSecret
            خالی
            خالی بماند (مقدار در AppPool ست شود)
            کلید امضای توکن‌های JWT؛ در صورت خالی بودن هر دو، کلید پیش‌فرض داخلی ناامن فعال می‌شود. مقدار تصادفی ۶۴ کاراکتری در محیط Application Pool تعریف گردد.
          
          
          
            ShowCaptcha
            false
            true
            فعال‌سازی بررسی کپچا در صفحه لاگین جهت جلوگیری از حملات Brute-Force و حدس کلمه عبور.
          
          
            IsLoginPersistent
            true
            false
            جلوگیری از ماندگاری کوکی لاگین پس از بستن مرورگر (حیاتی برای سامانه‌های سازمانی و رایانه‌های اشتراکی).
          
          
            LoginTimeoutHours
            48
            8 تا 12 (یک روز کاری)
            طول عمر توکن نشست کاربر به ساعت بدون تمدید خودکار؛ پس از آن نیاز به لاگین مجدد است.
          
          
          
            LogVerbosityLevel
            3
            1
            سطح ۳ متن خطاهای داخلی و SQL را به کلاینت نشان می‌دهد! در سطح ۱ فقط پیام عمومی خطا به کاربر ارسال شده و جزئیات صرفاً در پایگاه داده ثبت می‌شود.
          
          
          
            ArchiveNotAllowedFileTypes
            exe,msi
            فهرست جامع اسکریپت‌ها (بخش ۱۰)
            فهرست مسدودی پسوندهای اجرایی آرشیو؛ باید تمامی پسوندهای aspx، ashx، php، dll، bat و غیره در آن قید شوند.
          
          
          
            RestServiceAllowedHosts
            *
            فهرست صریح میزبان‌های مجاز
            علامت ستاره امکان سوءاستفاده از سرور جهت حمله جعل درخواست سمت سرور (SSRF) به شبکه‌های داخلی را فراهم می‌سازد.
          
          
            SystemSecurityProtocols
            شامل SSL3 و TLS1.0
            Tls12,Tls13
            محدودسازی درخواست‌های خروجی وب‌سرویس به پروتکل‌های رمزنگاری امن نسل جدید.
          
          
          
            ScriptTask:EnableCSharpLanguage
            true
            false (در صورت عدم استفاده)
            کد C# تسک فرآیند با سطح دسترسی پروسس سرور اجرا می‌شود. در صورت عدم نیاز، جهت ارتقای امنیت خاموش گردد.
          
          
          
            TrustedProxies
            تعریف نشده
            IP لودبالانسر / WAF یا 127.0.0.1
            تشخیص صحیح هدر X-Forwarded-For و آدرس IP واقعی کاربر جهت عملکرد درست محدودیت نرخ (Rate Limit).
          
          
          
            MVCEncryptSalt
            تعریف نشده (رشته عمومی ثابت)
            رشته تصادفی اختصاصی ۳۲+ کاراکتر
            نمک رمزنگاری پارامترهای آدرس؛ در نصب‌های جدید حتماً رشته‌ای یکتا وارد شود.
          
        
      
    

    

> [!WARNING]
> ⚠️ کلیدهای حساس حین اجرای اولیه (Bootstrap Keys): کلیدهای TempFilePath ، NewsFilePath ، ImagesGalleryFilePath ، DynamicPagesPath ، IdleTime و ProviderLimitations هنگام لود سامانه مستقیماً خوانده می‌شوند؛ هرگز این کلیدها را حذف نکنید بلکه مقدار آن‌ها را در صورت نیاز اصلاح نمایید.

  

  
  
    
## ۵. قالب نمونه Web.config سخت‌سازی‌شده (Production Ready)

    

      قطعه‌کدهای زیر بخش‌های کلیدی اصلاح‌شده فایل `Web.config` را جهت استقرار امن در سرور عملیاتی نمایش می‌دهند:
    

    
### الف) تنظیمات appSettings و secureSettings:

    
<secureSettings>
  <!-- کلید در متغیر محیطی JWT_SECRET اپ‌پول نگهداری می‌شود و اینجا خالی است -->
  <add key="JwtSecret" value="" />
</secureSettings>

<appSettings>
  <!-- محدودیت‌های حجم و درخواست JSON و صف نشست -->
  <add key="aspnet:MaxJsonLength" value="104857600" />
  <add key="aspnet:MaxJsonDeserializerMembers" value="150000" />
  <add key="aspnet:RequestQueueLimitPerSession" value="200" />

  <!-- مسدودسازی پسوندهای پرخطر در آرشیو -->
  <add key="ArchiveNotAllowedFileTypes" value="exe,msi,dll,bat,cmd,ps1,vbs,js,jse,wsf,hta,scr,com,asp,aspx,ashx,asmx,asa,cer,cshtml,vbhtml,config,svc,php,jsp,jspx,shtml,html,htm,svg" />

  <!-- کنترل نشست و ورود کاربر -->
  <add key="ShowCaptcha" value="true" />
  <add key="IsLoginPersistent" value="false" />
  <add key="LoginTimeoutHours" value="10" />
  <add key="CacheSlidingExpirationInterval" value="6" />
  <add key="CacheAbsoluteExpirationInterval" value="24" />

  <!-- لاگ و نمایش خطا -->
  <add key="LogVerbosityLevel" value="1" />
  <add key="MaximumLogRecordCount" value="100000" />
  <add key="DatabaseProvider:CommandTimeoutSeconds" value="300" />

  <!-- امنیت شبکه، SSRF و TLS خروجی -->
  <add key="RestServiceAllowedHosts" value="api.example.ir,sms.example.ir" />
  <add key="WsdlServiceAllowedHosts" value="service.example.ir" />
  <add key="SystemSecurityProtocols" value="Tls12,Tls13" />
  <add key="IgnoreSystemSecurityProtocolsForSelfRequests" value="false" />
  <add key="TrustedProxies" value="127.0.0.1" />
  <add key="MVCEncryptSalt" value="<رشته تصادفی حداقل ۳۲ کاراکتری اختصاصی>" />

  <!-- دسترسی سرویس‌های آرشیو بدون لاگین -->
  <add key="AllowAnonymousReadFileServices" value="false" />
  <add key="AllowAnonymousWriteFileServices" value="false" />
</appSettings>
    

    
### ب) تنظیمات system.web و system.webServer:

    
<system.web>
  <sessionState mode="InProc" timeout="60" cookieless="false" />
  <httpCookies sameSite="Lax" httpOnlyCookies="true" requireSSL="true" />
  <compilation targetFramework="4.7.2" debug="false" />
  <httpRuntime maxRequestLength="204800" executionTimeout="360" maxUrlLength="10999" maxQueryStringLength="90000" enableVersionHeader="false" />
  <customErrors mode="On" defaultRedirect="/ErrorPage/DefaultErrorPage" />
  <webServices>
    <protocols>
      <remove name="HttpGet" />
      <add name="HttpPost" />
    </protocols>
  </webServices>
</system.web>

<system.webServer>
  <httpErrors errorMode="Custom">
    <remove statusCode="401" /><remove statusCode="403" /><remove statusCode="404" /><remove statusCode="500" />
    <error statusCode="401" path="/ErrorPage/Http401UnAuthorize" responseMode="ExecuteURL" />
    <error statusCode="403" path="/ErrorPage/Http403Forbidden" responseMode="ExecuteURL" />
    <error statusCode="404" path="/ErrorPage/Http404NotFound" responseMode="ExecuteURL" />
    <error statusCode="500" path="/ErrorPage/DefaultErrorPage" responseMode="ExecuteURL" />
  </httpErrors>
  <security>
    <requestFiltering removeServerHeader="true">
      <requestLimits maxQueryString="90000" maxAllowedContentLength="209715200" />
      <verbs allowUnlisted="true">
        <add verb="TRACE" allowed="false" />
        <add verb="TRACK" allowed="false" />
      </verbs>
    </requestFiltering>
  </security>
  <directoryBrowse enabled="false" />
  <httpProtocol>
    <customHeaders>
      <remove name="X-Powered-By" />
      <add name="X-Frame-Options" value="SAMEORIGIN" />
      <add name="X-Content-Type-Options" value="nosniff" />
      <add name="X-XSS-Protection" value="1; mode=block" />
      <add name="Referrer-Policy" value="no-referrer" />
    </customHeaders>
  </httpProtocol>
</system.webServer>
    
  

  
  
    
## ۶. پایگاه داده: تفکیک دسترسی و جایگزینی کاربر sa

    

      کاربر برنامه هرگز نباید دارای دسترسی `sysadmin` باشد. سامانه در کارکرد عادی نیازمند ساخت/تغییر جدول فرم‌ها (`ALTER TABLE`)، اجرای رویه‌ها و خواندن/نوشتن داده است که منحصراً در محدوده دیتابیس BPMS انجام می‌پذیرد.
    

    
### اسکریپت T-SQL ساخت کاربر کمترین دسترسی برای سامانه:

    
-- اجرا توسط مدیر ارشد دیتابیس (SA)
USE [master];
CREATE LOGIN [graphbpms_app] WITH PASSWORD = N'<کلمه عبور پیچیده حداقل ۲۰ کاراکتر>',
CHECK_POLICY = ON, CHECK_EXPIRATION = OFF, DEFAULT_DATABASE = [<BPMS_DB>];
GO

USE [<BPMS_DB>];
CREATE USER [graphbpms_app] FOR LOGIN [graphbpms_app];
ALTER ROLE db_datareader ADD MEMBER [graphbpms_app];
ALTER ROLE db_datawriter ADD MEMBER [graphbpms_app];
ALTER ROLE db_ddladmin ADD MEMBER [graphbpms_app]; -- جهت ساخت و تغییر ساختار جداول فرم‌ساز
GRANT EXECUTE TO [graphbpms_app];
GRANT VIEW DEFINITION TO [graphbpms_app];
GRANT SHOWPLAN TO [graphbpms_app];
GO
    

    
### سخت‌سازی حساب sa و بستن خطرات جانبی:

    
USE [master];
ALTER LOGIN [sa] WITH PASSWORD = N'<رمز عبور تصادفی جدید>';
ALTER LOGIN [sa] DISABLE; -- غیرفعال‌سازی پس از اطمینان از وجود لاگین ادمین دیگر

-- غیرفعال‌سازی قطعی رویه خطرناک خط فرمان
EXEC sp_configure 'show advanced options', 1; RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 0; RECONFIGURE;
    
  

  
  
    
## ۷. مدیریت اسرار: تولید کلید تصادفی، JWT_SECRET و رمزنگاری IIS

    
### الف) دستور PowerShell تولید کلید رمزنگاری تصادفی امن:

    
$b = New-Object byte[] 48
[System.Security.Cryptography.RandomNumberGenerator]::Create().GetBytes($b)
[Convert]::ToBase64String($b)
    

    
### ب) تعریف JWT_SECRET روی Application Pool در خط فرمان ادمین:

    
set APPCMD=%windir%\system32\inetsrv\appcmd.exe
set POOL=GraphBPMS

%APPCMD% set config -section:system.applicationHost/applicationPools ^
/+"[name='%POOL%'].environmentVariables.[name='JWT_SECRET',value='<رشته تولیدشده در مرحله قبل>']" /commit:apphost

%APPCMD% recycle apppool /apppool.name:%POOL%
    

    
### ج) رمزنگاری بخش‌های connectionStrings و appSettings با DPAPI:

    
set REGIIS=%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe
set SITE=E:\GraphBPMS\2.14.132

%REGIIS% -pef "connectionStrings" "%SITE%" -prov "DataProtectionConfigurationProvider"
%REGIIS% -pef "appSettings" "%SITE%" -prov "DataProtectionConfigurationProvider"
    
  

  
  
    
## ۸. پیکربندی امنیتی پیشرفته IIS (احراز هویت، HSTS و Headers)

    
      
        
          
            شاخص امنیتی IIS
            تنظیم الزامی
            دلیل و تأثیر امنیتی
          
        
        
          
            احراز هویت IIS
            منحصراً Anonymous فعال؛ بقیه غیرفعال
            احراز هویت با کوکی OWIN صورت می‌گیرد؛ هویت کاربر ناشناس روی ApplicationPoolIdentity تنظیم شود.
          
          
            HSTS در IIS
            فعال با max-age=31536000 و ریدایرکت
            اجبار مرورگرها به برقراری ارتباط صرفاً با HTTPS و مسدودسازی حملات دان‌گرید پروتکل (SSL Stripping).
          
          
            لغو هدرهای افشاکننده
            removeServerHeader و لغو X-Powered-By
            جلوگیری از شناسایی نگارش IIS و ASP.NET توسط پویشگرهای امنیتی مهاجمین.
          
        
      
    

    
### دستورات فعال‌سازی HSTS روی وب‌سایت:

    
%APPCMD% set config -section:system.applicationHost/sites "/[name='%SITE%'].hsts.enabled:True" /commit:apphost
%APPCMD% set config -section:system.applicationHost/sites "/[name='%SITE%'].hsts.max-age:31536000" /commit:apphost
%APPCMD% set config -section:system.applicationHost/sites "/[name='%SITE%'].hsts.redirectHttpToHttps:True" /commit:apphost
    
  

  
  
    
## ۹. ساختار پوشه‌ها، مجوزهای NTFS و لغو دسترسی اجرا

    

      **اصل کلیدی:** حساب Application Pool روی کل سایت صرفاً دسترسی **خواندن و اجرا (RX)** دارد. دسترسی نوشتن (Modify) فقط و فقط به پوشه‌های ذخیره‌سازی فایل‌های کاربر داده می‌شود. در همان پوشه‌ها، مجوز اجرای کد در IIS کاملاً سلب می‌گردد.
    

    
      
        
          
            پوشه یا مسیر فایل
            دسترسی NTFS (AppPool)
            وضعیت در IIS
            توضیحات امنیتی
          
        
        
          
            ریشه سایت (به‌جز موارد زیر)
            فقط Read & Execute (RX)
            پیش‌فرض
            حذف دسترسی Users, Everyone, IUSR و IIS_IUSRS از ریشه.
          
          
            Web.config و Global.asax
            فقط Read (R)
            مسدود پیش‌فرض
            هیچ حساب برنامه‌ای نباید امکان دستکاری پیکربندی وب را داشته باشد.
          
          
            bin و Views
            فقط RX
            مسدود / بدون نوشتن
            تضمین عدم امکان تزریق DLL یا دستکاری کدهای نمایشی cshtml.
          
          
            Files و زیرپوشه‌ها
            Modify
            فقط Read (بدون Script/Execute)
            مهم‌ترین پوشه قابل نوشتن؛ مسدودسازی قطعی اجرای وب‌شل و اعمال لیست سفید.
          
          
            Files/WsdlConfig/wsdl.exe
            فقط Read & Execute (RX)
            Hidden Segment
            فایل‌های اجرایی ابزار WSDL نباید توسط هیچ پردازشی قابل بازنویسی باشند.
          
        
      
    

    
### لغو مجوز Script و Execute در پوشه‌های قابل نوشتن از طریق appcmd:

    
set RO=/section:system.webServer/handlers /accessPolicy:Read /commit:apphost
%APPCMD% set config "%SITE%/Files" %RO%
%APPCMD% set config "%SITE%/Content/images" %RO%
%APPCMD% set config "%SITE%/ArchiveLocations" %RO%
%APPCMD% set config "%SITE%/App_Data" %RO%
    
  

  
  
    
## ۱۰. مسدودسازی اجرای فایل‌های بارگذاری‌شده و لیست سفید Files/web.config

    

      پوشه `/Files` مقصد اصلی ذخیره فایل‌های موقت، مدارک و تصاویر است. جهت جلوگیری از هرگونه نفوذ و اجرای اسکریپت در صورت دور زدن فرم‌ها، فایل `web.config` زیر با **لیست سفید صریح پسوندها (Whitelist)** در پوشه Files قرار می‌گیرد:
    

    
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <security>
      <requestFiltering>
        <fileExtensions allowUnlisted="false">
          <add fileExtension=".jpg" allowed="true" />
          <add fileExtension=".jpeg" allowed="true" />
          <add fileExtension=".png" allowed="true" />
          <add fileExtension=".gif" allowed="true" />
          <add fileExtension=".bmp" allowed="true" />
          <add fileExtension=".webp" allowed="true" />
          <add fileExtension=".ico" allowed="true" />
          <add fileExtension=".pdf" allowed="true" />
        </fileExtensions>
        <hiddenSegments>
          <add segment="WsdlConfig" />
          <add segment="largeFileSavingTempDirectory" />
        </hiddenSegments>
      </requestFiltering>
    </security>
    <staticContent>
      <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="1.00:00:00" />
    </staticContent>
    <httpProtocol>
      <customHeaders>
        <add name="X-Content-Type-Options" value="nosniff" />
        <add name="Content-Security-Policy" value="default-src 'none'; img-src 'self'; sandbox" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
    
  

  
  
    
## ۱۱. چک‌لیست آزمون‌های صحت‌سنجی پس از استقرار

    
      
        
          
            عنوان آزمون امنیتی
            دستور یا روش آزمایش
            نتیجه مورد انتظار
          
        
        
          
            هدرهای امنیتی پاسخ
            curl -sI https://bpms.example.ir/Login
            حضور HSTS، CSP، X-Frame؛ عدم وجود Server و X-Powered-By
          
          
            هدایت به HTTPS
            curl -sI http://bpms.example.ir/
            کد ۳۰۱ یا ۳۰۷ به آدرس https
          
          
            تست اجرای فایل در Files
            قرار دادن موقت یک فایل test.aspx در Files/Temp و درخواست مرورگر
            خطای قطعی 403 یا 404 (عدم اجرا)
          
          
            بررسی نقش لاگین SQL
            SELECT IS_SRVROLEMEMBER('sysadmin')
            مقدار دقیق 0 (عدم دسترسی سیستمی)
          
          
            محدودیت نرخ (Rate Limit)
            ارسال ۶ تلاش متوالی لاگین اشتباه در یک دقیقه
            مسدود شدن درخواست ششم
          
        
      
    
  

  
  
    
## ۱۲. راهنمای ارتقا از نسخه‌های پیشین و پویش بدافزارها

    

      هنگام ارتقای سامانه به نگارش 2.14.132، رعایت چرخه زیر ضامن انتقال امن و بدون تخریب داده‌ها است:
    

    

      - تهیه پشتیبان کامل از دیتابیس، فایل Web.config و کل پوشه‌های داده.

      - بسته جدید در یک مسیر مجزا باز شود؛ هرگز روی پوشه نسخه قبلی Copy-Paste نکنید.

      - کلیدهای اختصاصی مشتری (رشته اتصال، ایمیل و تنظیمات خاص) از فایل قدیمی به Web.config نسخه جدید منتقل شوند.

      - **پویش امنیتی پوشه‌های داده:** پیش از جابجایی پوشه Files به نسخه جدید، حتماً اسکریپت زیر جهت کشف اسکریپت‌ها یا فایل‌های ناشناخته احتمالی اجرا گردد:

    

    
$root = 'E:\GraphBPMS\2.14.132'
$dirs = 'Files','Content\images','Mobile_PWA\app\assets\images','ArchiveLocations','App_Data' | ForEach-Object { Join-Path $root $_ } | Where-Object { Test-Path $_ }
$ext = '.aspx','.ashx','.asmx','.asp','.asa','.cer','.cshtml','.vbhtml','.svc','.soap','.rem','.xamlx','.config','.php','.jsp','.exe','.dll','.bat','.cmd','.ps1','.vbs','.shtml'
$known = 'web.config','wsdl.exe','wsdl.exe.config','SvcUtil.exe'

Get-ChildItem -Path $dirs -Recurse -File -Force -ErrorAction SilentlyContinue |
Where-Object { $ext -contains $_.Extension.ToLower() -and $known -notcontains $_.Name } |
Select-Object FullName, Length, CreationTime, LastWriteTime | Format-Table -AutoSize
    
  

  
  
    
## ۱۳. جدول اولویت‌بندی اقدامات امنیتی (پیوست نهایی)

    
      
        
          
            سطح اولویت
            مورد کنترلی
            مقدار بسته
            اقدام امن الزامی
          
        
        
          
            فوری (Critical)
            متغیر محیطی JWT_SECRET
            تعریف نشده
            تولید کلید تصادفی امن و ثبت روی Application Pool
          
          
            فوری (Critical)
            کاربر پایگاه داده
            sa
            ساخت کاربر graphbpms_app با کمترین دسترسی
          
          
            فوری (Critical)
            مجوز اجرای پوشه Files
            فعال
            لغو Script/Execute و تنظیم منحصراً Read در IIS
          
          
            فوری (Critical)
            دستور compilation debug
            true
            تغییر به false و اطمینان از حذف نمادهای دیباگ
          
          
            بالا (High)
            سیاست‌های مجاز SSRF
            *
            تعریف فهرست صریح میزبان‌های وب‌سرویس مجاز
          
          
            بالا (High)
            فهرست سفید Files/web.config
            فهرست سیاه ۱۷ پسوند
            جایگزینی با لیست سفید سخت‌گیرانه
          
          
            متوسط (Medium)
            کپچا و ماندگاری نشست
            false / true
            تنظیم ShowCaptcha=true و IsLoginPersistent=false
