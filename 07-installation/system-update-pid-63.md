---
id: 63
title: "بروزرسانی سامانه گراف"
englishTitle: "Rolling Upgrades & Migration Guide"
category: "07-installation"
wikiPath: "63"
wikiUrl: "https://wiki.graphbpms.ir/63"
breadcrumbs: ["نصب و بروزرسانی سامانه","بروزرسانی سامانه گراف"]
priority: 2
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# بروزرسانی سامانه گراف

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `نصب و بروزرسانی سامانه` ❯ `بروزرسانی سامانه گراف`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/63](https://wiki.graphbpms.ir/63)

# بروزرسانی سامانه گراف

ابتدا سامانه را از طریق Internet Information Service متوقف می کنیم

توقف سامانه از طریق IIS

پس از دریافت نسخه جدید سامانه باید آن را بر روی سرور و یا سیستم مد نظر خود بارگذاری کنیم و سپس وارد پوشه نسخه قدیمی سامانه بشویم. برای اطمینان از محل پوشه نسخه قدیمی سامانه می توانیم از قسمت explore وارد آن شویم

یافتن پوشه نسخه قدیمی از طریق Explore

مشاهده پوشه نسخه قدیمی

برای بروز کردن سامانه باید پوشه های `bin`، `content`، `Mobile_PWA` و فایل های `notification-sw`، `service-worker` و `Web.Config` را از نسخه جدید جایگزین نسخه قدیمی کرد

> [!WARNING]
> توجه قبل از جایگزین کردن web.config بهتر است نسخه قدیمی را تغییر نام داد تا اطلاعات connection string را از نسخه قدیمی به نسخه جدید منتقل کنیم

جایگزینی پوشه‌ها و فایل‌های نسخه جدید

نمای پوشه‌های جایگزین‌شده

پس از جایگزین کردن فایل ها باید connection string را از web.config نسخه قدیمی در نسخه جدید کپی کرد

کپی ConnectionString از web.config قدیمی به جدید

**نمونه ConnectionString :**

```
<add name="BPMS" connectionString="Data Source=(localhost);Initial Catalog=bpms; user id=sa;password=123; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
```

> [!NOTE]
> نکته در نسخه های جدید سامانه باید در ConnectionString عبارت MultipleActiveResultSets=True باشد

```
Data Source : آدرس سرور پایگاه داده 
Initial Catalog : نام پایگاه داده
user id : شناسه
password : رمز عبور
```

سپس باید کوئری که در پوشه sql script نسخه جدید می باشد را اجرا کرد

پوشه SQL Script نسخه جدید

باید دقت داشته باشید که در قسمت message نباید خطایی باشد

اجرای اسکریپت SQL بدون خطا

> [!WARNING]
> توجه توجه داشته باشید که باید نسخه سورس پروژه با نسخه اسکریپ یکسان باشد

> [!NOTE]
> نکته نسخه هر اسکریت در اول کد های آن درج شده و نام فایل اسکریت شامل نسخه اسکریت است

> [!NOTE]
> نکته اسکریپت های مروبط به سامانه در سورس کد پروژه و در شاخه SQLScript قرار دارد

پس از اجرای کوئری ها می توانید سامانه را start کنید

راه‌اندازی مجدد سامانه
