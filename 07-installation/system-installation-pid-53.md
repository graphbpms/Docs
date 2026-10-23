---
id: 53
title: "نصب سامانه گراف"
englishTitle: "Enterprise Installation & Deployment"
category: "07-installation"
wikiPath: "53"
wikiUrl: "https://wiki.graphbpms.ir/53"
breadcrumbs: ["نصب و بروزرسانی سامانه","نصب سامانه گراف"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-10-04T11:31:13"
---

# نصب سامانه گراف

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `نصب و بروزرسانی سامانه` ❯ `نصب سامانه گراف`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/53](https://wiki.graphbpms.ir/53)

ابتدا سامانه را دریافت می کنیم و سپس آن را در مسیر مشخص استخراج می کنیم

![install2.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=04C79D87-6501-4A6C-996B-441EDFBF1197.png)

استخراج فایل‌های سامانه در مسیر مشخص

حالا باید در Sql Server، پایگاه داده مربوط به سامانه را ایجاد کرد. برای این کار می توانید از لینک زیر Backup تهیه شده را دانلود کنیم و آن را بر روی پایگاه داده Sql Server خود Restore کنیم.

* * *

پایگاه داده خالی را از حالت فشرده خارج می کنیم

![installnulldb.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=81487BF6-137C-401D-A323-6541FBF8A64F.png)

خارج کردن پایگاه داده خالی از حالت فشرده

* * *

سپس باید پایگاه داده در Sql Server که می خواهیم سامانه را بر روی آن نصب کنیم Restore کنیم

![restoredb.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AB3A9211-552F-4CD2-9623-DC8168D42ED6.png)

Restore کردن پایگاه داده در Sql Server

پس از Restore کردن باید تمام جدول های مربوط به سامانه نمایش داده شود

* * *

در Internet Information Service باید تنظیمات مربوط به اضافه کردن سامانه را انجام داد

![internetis1.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=DBEA6321-D1AA-4C6D-810A-B9244A240201.png)

افزودن سامانه در IIS

در قسمت `Physical Path` باید آدرس پوشه مربوط به فایل های سامانه را انتخاب کرد تا در محتوا اضافه شود

* * *

در مرحله بعد باید در فایل `web.config` تنظیمات اتصال به پایگاه داده را انجام داد. برای این کار باید به پوشه سامانه مراجعه کنید و سپس `web.config` را باز کنید

![webconfig1.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=51AAB472-77F3-4664-92F0-4BF3F09B6625.png)

باز کردن فایل web.config

اگر می خواهید هم زمان چند کاربر با یک نام کاربری در سامانه وارد شوند باید `SessionMultiple` را برابر با `true` قرار دهید

```
key="SessionMultiple" value="true"
```

* * *

برای اتصال پایگاه داده به بانک اطلاعاتی باید `ConnectionString` مورد نظر در این قسمت درج شود

![webconfigimage.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=CD40A8AA-A47B-433D-8D0B-22C124DC9D53.png)

درج ConnectionString در web.config

**نمونه ConnectionString :**

```
<add name="BPMS" connectionString="Data Source=(localhost);Initial Catalog=bpms; user id=sa;password=123; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
```

```
Data Source : آدرس سرور پایگاه داده 
Initial Catalog : نام پایگاه داده
user id : شناسه
password : رمز عبور
```

* * *

![sqlscriptdirectoryimage.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9302ABEF-807F-4905-BBEE-E153DADC4226.png)

پوشه SQLScript در سورس کد پروژه

بعد از ویرایش و ذخیره کردن `web.config` باید اسکریپت‌های مربوط به نسخه همان سامانه در بانک اطلاعاتی ایجاد شده اجرا شود

> [!WARNING]
> **توجه**
> 
> توجه داشته باشید که باید نسخه سورس پروژه با نسخه اسکریپت یکسان باشد

> [!NOTE]
> **نکته**
> 
> نسخه هر اسکریپت در اول کد های آن درج شده و نام فایل اسکریپت شامل نسخه اسکریپت است

> [!NOTE]
> **نکته**
> 
> اسکریپت های مربوط به سامانه در سورس کد پروژه و در شاخه `SQLScript` قرار دارد

![script2.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8324B48D-BF62-455A-BBB5-03EEB82D951C.png)

اجرای اسکریپت‌های SQL در بانک اطلاعاتی

* * *

در Internet Information Service و در قسمت Permission فایل های سامانه باید جهت آپلود فایل ها و عکس کاربران سامانه، اخبار، گالری دسترسی را مطابق مراحل زیر اضافه نمود

ابتدا باید بر روی `Edit Permission` کلیک کرد

![permissioniis.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8A2A4581-C949-4F13-8FA1-58DCFB3C50FD.png)

کلیک بر روی Edit Permission در IIS

سپس باید در قسمت Security و در `Group or UserNames` مجوز های `Full control` و `Modify` را داد

![permissioniis2.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D88106D6-0530-4937-AF55-A2C9AC665C58.png)

تنظیم دسترسی Full control و Modify

![permissioniis3.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8A2A4581-C949-4F13-8FA1-58DCFB3C50FD.png)

تایید مجوزهای دسترسی

* * *

در قسمت `Manage Website` باید `Browse` را انتخاب کنید تا وارد سامانه شوید

![managewebsiteiis.png](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=CC668C71-8E3D-4B9C-AAA0-26885C55D0D8.png)

انتخاب Browse برای ورود به سامانه
