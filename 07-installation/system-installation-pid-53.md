---
id: 53
title: "نصب سامانه گراف"
englishTitle: "Enterprise Installation & Deployment"
category: "07-installation"
wikiPath: "53"
wikiUrl: "https://wiki.graphbpms.ir/53"
breadcrumbs: ["نصب سامانه گراف"]
priority: 1
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# نصب سامانه گراف

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `نصب سامانه گراف`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/53](https://wiki.graphbpms.ir/53)

# نصب سامانه گراف

ابتدا سامانه را دریافت می کنیم و سپس آن را در مسیر مشخص استخراج می کنیم

استخراج فایل‌های سامانه در مسیر مشخص

حالا باید در Sql Server، پایگاه داده مربوط به سامانه را ایجاد کرد. برای این کار می توانید از لینک زیر Backup تهیه شده را دانلود کنیم و آن را بر روی پایگاه داده Sql Server خود Restore کنیم.

پایگاه داده خالی را از حالت فشرده خارج می کنیم

خارج کردن پایگاه داده خالی از حالت فشرده

سپس باید پایگاه داده در Sql Server که می خواهیم سامانه را بر روی آن نصب کنیم Restore کنیم

Restore کردن پایگاه داده در Sql Server

پس از Restore کردن باید تمام جدول های مربوط به سامانه نمایش داده شود

در Internet Information Service باید تنظیمات مربوط به اضافه کردن سامانه را انجام داد

افزودن سامانه در IIS

در قسمت `Physical Path` باید آدرس پوشه مربوط به فایل های سامانه را انتخاب کرد تا در محتوا اضافه شود

در مرحله بعد باید در فایل `web.config` تنظیمات اتصال به پایگاه داده را انجام داد. برای این کار باید به پوشه سامانه مراجعه کنید و سپس `web.config` را باز کنید

باز کردن فایل web.config

اگر می خواهید هم زمان چند کاربر با یک نام کاربری در سامانه وارد شوند باید `SessionMultiple` را برابر با `true` قرار دهید

```
key="SessionMultiple" value="true"
```

برای اتصال پایگاه داده به بانک اطلاعاتی باید `ConnectionString` مورد نظر در این قسمت درج شود

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

پوشه SQLScript در سورس کد پروژه

بعد از ویرایش و ذخیره کردن `web.config` باید اسکریپت‌های مربوط به نسخه همان سامانه در بانک اطلاعاتی ایجاد شده اجرا شود

> [!WARNING]
> توجه توجه داشته باشید که باید نسخه سورس پروژه با نسخه اسکریپت یکسان باشد

> [!NOTE]
> نکته نسخه هر اسکریپت در اول کد های آن درج شده و نام فایل اسکریپت شامل نسخه اسکریپت است

> [!NOTE]
> نکته اسکریپت های مربوط به سامانه در سورس کد پروژه و در شاخه SQLScript قرار دارد

اجرای اسکریپت‌های SQL در بانک اطلاعاتی

در Internet Information Service و در قسمت Permission فایل های سامانه باید جهت آپلود فایل ها و عکس کاربران سامانه، اخبار، گالری دسترسی را مطابق مراحل زیر اضافه نمود

ابتدا باید بر روی `Edit Permission` کلیک کرد

کلیک بر روی Edit Permission در IIS

سپس باید در قسمت Security و در `Group or UserNames` مجوز های `Full control` و `Modify` را داد

تنظیم دسترسی Full control و Modify

تایید مجوزهای دسترسی

در قسمت `Manage Website` باید `Browse` را انتخاب کنید تا وارد سامانه شوید

انتخاب Browse برای ورود به سامانه
