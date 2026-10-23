---
id: 1629
graphDocId: 1482
title: "انتشار و فراخوانی Process در ESB"
subTitle: null
module: "02-process"
wikiPath: "Process/1627/1629"
wikiUrl: "https://wiki.graphbpms.ir/Process/1627/1629"
parentPageUrl: "https://wiki.graphbpms.ir/Process/1627/1629"
breadcrumbs: ["مدیریت و مدل‌سازی فرآیندها","توسعه و یکپارچه‌سازی","انتشار و فراخوانی Process در ESB"]
parentId: 1627
parentTitle: "توسعه و یکپارچه‌سازی"
url: "1629"
priority: 2
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت فرآیند > مدیریت و مدل‌سازی فرآیندها > توسعه و یکپارچه‌سازی. راهنمای انتشار یک Process به‌عنوان سرویس در ESB است: انتخاب نسخه مناسب و سازگاری مدل، تعیین نام API و HTTP Method و نوع پاسخ، نگاشت Variableهای داخلی به ورودی‌های عمومی، تعیین خروجی از End Event، ذخیره و مدیریت سرویس نسخه، خواندن مستند تولیدشده، احراز هویت و Token و خطاهای HTTP. نمونه فراخوانی با cURL، Fetch، jQuery و C# و روش آزمون و رفع اشکال قرارداد نیز ارائه شده است."
updatedAt: "2026-10-18T16:08:51"
---

# انتشار و فراخوانی Process در ESB

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت و مدل‌سازی فرآیندها` ❯ `توسعه و یکپارچه‌سازی` ❯ `انتشار و فراخوانی Process در ESB`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/Process/1627/1629](https://wiki.graphbpms.ir/Process/1627/1629)

ESB یک قرارداد سرویس با نام، روش درخواست، ورودی‌ها و خروجی‌های مشخص روی فرآیند ایجاد می‌کند. این مسیر برای اجرای سرویس‌مانند تا پایان طراحی می‌شود. بستهٔ «انتشار فرآیند» برای انتقال طراحی به محیط دیگر، مفهوم دیگری است؛ تعریف ESB از آیکن مربوط به همان نسخه در لیست فرآیندها آغاز می‌شود.

## ورود از نسخهٔ درست

در لیست فرآیندها شناسهٔ نسخه و خانواده را کنترل کنید و آیکن ESB ردیف مورد نظر را بزنید. جدول سرویس‌های آن نسخه، شناسه، نام لاتین و فعال‌بودن را نشان می‌دهد. تعداد «وب‌سرویس‌ها» در لیست فرآیندها تعداد تعریف‌های متصل است، نه شمار فراخوانی‌ها.

[![آیکن ESB در عملیات یک نسخهٔ فرآیند](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=1B58E39A-4947-4C51-99A8-062E9DBB6406)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=1B58E39A-4947-4C51-99A8-062E9DBB6406 "بازکردن تصویر در اندازه اصلی")

آیکن ESB در عملیات یک نسخهٔ فرآیند

[![فهرست ESBهای متصل به نسخه و دکمهٔ افزودن](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=7FC31B93-BEC9-42E1-BEB7-7459F28529A5)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=7FC31B93-BEC9-42E1-BEB7-7459F28529A5 "بازکردن تصویر در اندازه اصلی")

فهرست ESBهای متصل به نسخه و دکمهٔ افزودن

## بررسی سازگاری مدل

اگر ساختار مدل برای این مسیر مناسب نباشد، جدول سازگاری نام Node، تعداد فعلی و تعداد مورد نیاز را نشان می‌دهد. از «ویرایش فرآیند» وارد مدل شوید، مسئله را اصلاح و نسخه را ذخیره کنید، سپس ESB را دوباره باز کنید.

| عنصر | شرط نمایش‌داده‌شده |
| --- | --- |
| MessageStartEvent | دقیقاً یک عدد |
| EndEvent | دقیقاً یک عدد |
| UserTask | نباید داشته باشد |
| GraphMobileTask | نباید داشته باشد |
| ReceiveTask | نباید داشته باشد |
| TimerEvent | نباید داشته باشد |
| SignalEvent | نباید داشته باشد |
| CallActivityTask | نباید داشته باشد |

[![نمونهٔ رد سازگاری: دو آغاز و دو پایان و چند گام بازدارنده؛ ستون تعداد مورد نیاز ملاک اصلاح است](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4555C078-D0DA-44CA-B974-3EE577A91719)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4555C078-D0DA-44CA-B974-3EE577A91719 "بازکردن تصویر در اندازه اصلی")

نمونهٔ رد سازگاری: دو آغاز و دو پایان و چند گام بازدارنده؛ ستون تعداد مورد نیاز ملاک اصلاح است

این محدودیت‌ها متعلق به ساخت ESB از این پنجره‌اند. وجود یک نماد در BPMN یا کاربرد آن در فرآیند معمولی، سازگاری‌اش با اجرای سرویس‌مانند را ثابت نمی‌کند. به‌ویژه نام کلی TimerEvent و SignalEvent در گزارش اعتبارسنجی را با پشتیبانی/عدم پشتیبانی تمام انواع رویدادها در کل محصول یکی نگیرید.

## مدل مناسب برای ESB

ESB از Process یک API قابل فراخوانی می‌سازد. فرآیند مقصد باید **دقیقاً یک Message Start Event** و **دقیقاً یک End Event** داشته باشد و بدون توقف انسانی یا انتظار پیام، از ابتدا تا پایان اجرا شود. User Task، Graph Mobile Task، Receive Task، Timer Intermediate Catch، Signal Intermediate Catch و Call Activity از Nodeهای منتظرند و در این مدل مجاز نیستند.

[![اعتبارسنجی پیش‌نیازهای Process پیش از ورود به مدیریت ESB.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9EABA31E-F491-474A-9491-E84A4FDB8871)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9EABA31E-F491-474A-9491-E84A4FDB8871 "بازکردن تصویر در اندازه اصلی")

اعتبارسنجی پیش‌نیازهای Process پیش از ورود به مدیریت ESB.

اگر پیام پیش‌نیاز نمایش داده شد، مدل و نسخهٔ فرآیند را اصلاح و سپس به صفحهٔ ESB برگردید. پس از اعتبارسنجی، فهرست ESBها و فرمان افزودن در دسترس قرار می‌گیرد. این قابلیت با دکمهٔ غیرفعال «انتشار فرآیند» در فهرست یکی نیست.

## نام، روش درخواست و نوع پاسخ

1.  «افزودن» را انتخاب و نام لاتین سرویس را وارد کنید. همین نام در مسیر فراخوانی دیده می‌شود.
2.  متد درخواست را از GET یا POST مطابق قرارداد مصرف‌کننده انتخاب کنید.
3.  نوع محتوای پاسخ را text/plain یا application/json تعیین کنید.
4.  فعال‌بودن و «مجوز استفاده ناشناس» را با سیاست سرویس تنظیم کنید.
5.  توضیح سرویس، ورودی مورد انتظار و معنی نتیجه را بنویسید؛ این توضیحات بخشی از مستند مصرف‌کننده است.

[![تعریف ESB و انتخاب GET یا POST در متد درخواست](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=705DF9E2-CA38-4DF0-ABCA-C501F6AC86C8)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=705DF9E2-CA38-4DF0-ABCA-C501F6AC86C8 "بازکردن تصویر در اندازه اصلی")

تعریف ESB و انتخاب GET یا POST در متد درخواست

[![انتخاب نوع محتوای پاسخ؛ این کنترل به پاسخ سرویس مربوط است](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4AEA5206-6988-4BBB-8B01-9B99924844B7)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4AEA5206-6988-4BBB-8B01-9B99924844B7 "بازکردن تصویر در اندازه اصلی")

انتخاب نوع محتوای پاسخ؛ این کنترل به پاسخ سرویس مربوط است

فعال‌بودن سرویس و مجوز کاربر دو کنترل جدا هستند. فعال‌کردن ناشناس را راهی برای حل خطای مجوز کاربر ندانید. وقتی روش، نام یا نوع خروجی را عوض می‌کنید، مصرف‌کننده باید قرارداد تازه را دریافت کند.

## نام API، متد، پاسخ و هویت فراخوان

«نام لاتین» ESB در آدرس API اثر دارد؛ فقط نامی برای شناسایی داخلی مدیر نیست. برای قرارداد مصرف‌کننده، نام مناسب و پایدار انتخاب و متد `GET` یا `POST` را تعیین کنید. «نوع محتوای پاسخ» نیز می‌تواند `text/plain` یا `application/json` باشد.

[![نام لاتین، متد درخواست، وضعیت فعال و مجوز استفاده ناشناس در پنل ESB.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8E41CB64-3F9A-4EE4-A2B7-3B8C441643E0)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8E41CB64-3F9A-4EE4-A2B7-3B8C441643E0 "بازکردن تصویر در اندازه اصلی")

نام لاتین، متد درخواست، وضعیت فعال و مجوز استفاده ناشناس در پنل ESB.

[![انتخاب نوع محتوای پاسخ بین text/plain و application/json.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=18C7C753-F9C7-4693-B69E-633240E01EA4)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=18C7C753-F9C7-4693-B69E-633240E01EA4 "بازکردن تصویر در اندازه اصلی")

انتخاب نوع محتوای پاسخ بین text/plain و application/json.

اگر «مجوز استفاده ناشناس» فعال نباشد، فراخوان باید Token معتبر را مطابق مستند تولیدشده، از مسیر `Authorization` با قالب Bearer یا `privateToken` پشتیبانی‌شده بفرستد. هویت Token علاوه بر اعتبار احراز هویت باید در Laneِ همان Message Start مجاز باشد؛ Token یک کاربر دیگر صرفاً به دلیل معتبر بودن، دسترسی شروع این Process را ندارد.

با فعال‌کردن مجوز ناشناس، فراخوان بدون Token کاربر ممکن می‌شود و هویت ناشناس در اجرای فرآیند و لاگ آن ثبت می‌شود. این حالت را آگاهانه متناسب با قرارداد عمومی سرویس انتخاب کنید. متن «توضیحات» نیز در مستندات تولیدشدهٔ ESB قرار می‌گیرد؛ هدف API، معنای ورودی‌ها و خروجی را برای مصرف‌کننده روشن بنویسید.

## تعریف و نگاشت ورودی‌ها

متغیرهای مورد نیاز را در فرآیند تعریف کنید و مدل را ذخیره کنید. سپس در بخش پارامترهای ESB از فهرست «افزودن پارامتر» متغیر مقصد را انتخاب کنید. نام داخلی متغیر ثابت نمایش داده می‌شود؛ «تغییر نام به» نام بیرونی آن در درخواست سرویس است.

[![دو متغیر فرآیند var1 و var2 که می‌توانند ورودی ESB شوند](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=165DCB5F-7315-4AEF-8A19-07244A2AD846)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=165DCB5F-7315-4AEF-8A19-07244A2AD846 "بازکردن تصویر در اندازه اصلی")

دو متغیر فرآیند var1 و var2 که می‌توانند ورودی ESB شوند

[![افزودن متغیر فرآیند به فهرست پارامترهای قرارداد ESB](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=E17F2ADD-EF43-4EAD-BED2-64355C377F0E)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=E17F2ADD-EF43-4EAD-BED2-64355C377F0E "بازکردن تصویر در اندازه اصلی")

افزودن متغیر فرآیند به فهرست پارامترهای قرارداد ESB

| ستون | کاربرد |
| --- | --- |
| نام پارامتر | متغیر داخلی انتخاب‌شده در فرآیند |
| تغییر نام به | کلید قابل ارسال توسط مصرف‌کننده؛ مثلاً var2 در قرارداد به Variable2 تبدیل می‌شود. |
| مقدار پیش‌فرض | مقدار پیش‌فرض تعریف‌شده برای ورودی؛ رفتار حذف کلید یا null را جدا آزمون کنید. |
| نوع مقدار | نوع تبدیل/انتظار قرارداد سرویس |
| حذف ردیف | برداشتن همین نگاشت از قرارداد، نه حذف متغیر فرآیند |

نوع‌های دیده‌شده در انتخاب‌گر عبارت‌اند از Boolean، Byte، SByte، Char، Decimal، Double، Single، Int32، UInt32، Int64، UInt64، Int16، UInt16، String، Object، Guid و DateTime. نوع را با مقدار مصرف‌شده در فرآیند تطبیق دهید؛ انتخاب Object به معنی پذیرش هر متن نامعتبر به‌عنوان JSON نیست.

[![نام بیرونی، مقدار پیش‌فرض و فهرست نوع‌های پارامتر؛ var2 با نام Variable2 منتشر شده است](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8CD0AEA3-656C-40D7-B642-7ADE3E6981DD)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8CD0AEA3-656C-40D7-B642-7ADE3E6981DD "بازکردن تصویر در اندازه اصلی")

نام بیرونی، مقدار پیش‌فرض و فهرست نوع‌های پارامتر؛ var2 با نام Variable2 منتشر شده است

هر پارامتر را یک بار به نگاشت اضافه کنید. تغییر نام بیرونی، نام متغیر داخل طراحی را عوض نمی‌کند. برای چند ورودی هم‌نام یا نام بیرونی تکراری، نتیجهٔ اعتبارسنجی ذخیره را کنترل کنید و قرارداد مبهم منتشر نکنید.

## انتخاب ورودی عمومی از Variableهای داخلی

فهرست «افزودن پارامتر» از Variableهای تعریف‌شدهٔ همان Process ساخته می‌شود. فقط Variableهایی را اضافه کنید که باید از بیرون مقدار بگیرند؛ لازم نیست تمام متغیرهای داخلی در API منتشر شوند. برای نمونه، از ده Variable می‌توانید فقط دو ورودی عمومی بسازید و متغیرهای داخلی را در همان فرآیند نگه دارید.

[![انتخاب Variableهای Process برای پارامترهای ورودی ESB.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=39F764C9-3115-448A-B716-CD1D0D6C77F8)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=39F764C9-3115-448A-B716-CD1D0D6C77F8 "بازکردن تصویر در اندازه اصلی")

انتخاب Variableهای Process برای پارامترهای ورودی ESB.

برای هر پارامتر، «نام پارامتر» به Variable داخلی متصل است و «تغییر نام به» نام بیرونی قرارداد را تعیین می‌کند. در تصویر، `var2` با نام عمومی `Variable2` عرضه شده است. مقدار پیش‌فرض برای نبود ورودی و نوع مقدار برای قرارداد و تبدیل داده تنظیم می‌شود. نام بیرونی، نام Variable داخلی را تغییر نمی‌دهد.

[![نام عمومی Variable2، مقدار پیش‌فرض و نوع داده برای ورودی‌های منتخب.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=41C18615-0550-49D2-8207-66D25A3C5B7D)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=41C18615-0550-49D2-8207-66D25A3C5B7D "بازکردن تصویر در اندازه اصلی")

نام عمومی Variable2، مقدار پیش‌فرض و نوع داده برای ورودی‌های منتخب.

نوع را مطابق مقدار واقعی انتخاب کنید؛ برای مثال، دادهٔ ساختاریافته‌ای که باید به Object تبدیل شود با رشتهٔ ساده یکسان نیست. در مستندات و نمونهٔ درخواست، همان نام عمومی و نوع انتخاب‌شده باید استفاده شود.

## از End Event تا خروجی سرویس

خروجی ESB از خروجی‌های تعریف‌شده در End Event انتخاب می‌شود. ابتدا در رویداد پایان کلید خروجی و مقدار آن را تنظیم کنید؛ مقدار می‌تواند از متغیرهای فرآیند بیاید. سپس در ESB آن کلید را به فهرست خروجی‌ها اضافه و نام بیرونی، مقدار پیش‌فرض و نوع را تعیین کنید.

[![تعریف OutOf1 و OutOf2 در پایان و اتصال آن‌ها به متغیرهای فرآیند](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=54751680-E187-4570-B8B8-3A0D540005C8)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=54751680-E187-4570-B8B8-3A0D540005C8 "بازکردن تصویر در اندازه اصلی")

تعریف OutOf1 و OutOf2 در پایان و اتصال آن‌ها به متغیرهای فرآیند

[![نمایش خروجی‌های پایان در فهرست افزودن خروجی ESB](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D6FB7AA0-02D7-465A-989E-B44A2969F298)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D6FB7AA0-02D7-465A-989E-B44A2969F298 "بازکردن تصویر در اندازه اصلی")

نمایش خروجی‌های پایان در فهرست افزودن خروجی ESB

[![قرارداد کامل: ورودی Object و String و خروجی Int32 با نام بیرونی O2](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AE94A465-DE3D-464C-AF26-248A0B6DF43C)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AE94A465-DE3D-464C-AF26-248A0B6DF43C "بازکردن تصویر در اندازه اصلی")

قرارداد کامل: ورودی Object و String و خروجی Int32 با نام بیرونی O2

در مثال، کلید داخلی `OutOf2` با نام بیرونی `O2` و نوع Int32 منتشر شده است. مقدار 555 در کنترل پیش‌فرض دیده می‌شود؛ این تصویر به‌تنهایی نشان نمی‌دهد null، رشتهٔ خالی یا خطای تبدیل همگی به همین مقدار تبدیل می‌شوند. این سه حالت باید مستقل آزمون شوند.

## انتخاب خروجی عمومی از End واحد

فهرست خروجی‌های ESB از متغیرهای خروجی همان End واحد گرفته می‌شود. هنگام فراخوان، پارامترهای ورودی Variableهای متناظر را مقداردهی می‌کنند، Process اجرا می‌شود و نتیجهٔ End بر پایهٔ نگاشت خروجی ESB بازگردانده می‌شود. می‌توانید از میان بیست خروجی End فقط یک خروجی را برای این API منتشر کنید.

[![انتخاب خروجی عمومی ESB از خروجی‌های تعریف‌شده در End Event.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=561F360B-412E-4099-8F68-B8EFE98F1BE9)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=561F360B-412E-4099-8F68-B8EFE98F1BE9 "بازکردن تصویر در اندازه اصلی")

انتخاب خروجی عمومی ESB از خروجی‌های تعریف‌شده در End Event.

در نگاشت خروجی ESB، مقدار End به‌صورت رشته دریافت و با «نوع مقدار» منتخب تبدیل می‌شود؛ این شرحِ نگاشت ESB را به همهٔ ساختارهای پاسخ API و سطح‌های Verbosity تعمیم ندهید. نام عمومی و مقدار پیش‌فرض نیز قابل تنظیم‌اند؛ در مثال، اگر خروجی End خالی یا بدون مقدار باشد، مقدار پیش‌فرض خروجی مصرف می‌شود. تبدیل نوع را متناسب با مقدار واقعی بررسی کنید؛ نوشتن نوع عددی، یک رشتهٔ غیرقابل تبدیل را خودبه‌خود به عدد معنادار تبدیل نمی‌کند. نمونهٔ کامل زیر ورودی ساختاریافتهٔ `var1` از نوع Object، ورودی `var2` با نام عمومی `Variable2` و یک خروجی منتخب از نوع `Int32` با مقدار پیش‌فرض `555` را نشان می‌دهد.

[![قرارداد کامل یک ESB با ورودی‌های منتخب و تنها یک خروجی منتخب از End.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=EF45D15E-461E-402E-9947-E0FBD2FB87AA)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=EF45D15E-461E-402E-9947-E0FBD2FB87AA "بازکردن تصویر در اندازه اصلی")

قرارداد کامل یک ESB با ورودی‌های منتخب و تنها یک خروجی منتخب از End.

یک Process می‌تواند چند ESB داشته باشد. هر ESB می‌تواند نام، متد و زیرمجموعهٔ ورودی/خروجی متناسب با مصرف‌کنندهٔ خود داشته باشد؛ تفاوت قرارداد عمومی به معنی نیاز به کپی‌کردن کل مدل Process نیست.

## ذخیره و مدیریت سرویس‌های نسخه

پس از کنترل نگاشت‌ها، «ذخیره» را بزنید و بازگشت سرویس به فهرست را بررسی کنید. هر نسخه می‌تواند چند تعریف ESB داشته باشد. سرویس هدف را از نام و شناسهٔ خود پیدا کنید و از عملیات ردیف، ویرایش، حذف یا مستند را انتخاب کنید.

[![دو سرویس مستقل در فهرست همان نسخه](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=62D0C993-01C7-441C-A249-E00E295C12EE)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=62D0C993-01C7-441C-A249-E00E295C12EE "بازکردن تصویر در اندازه اصلی")

دو سرویس مستقل در فهرست همان نسخه

[![ویرایش قرارداد ESB از عملیات ردیف](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=3A0BD36D-5244-4CA5-880D-B689245305D5)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=3A0BD36D-5244-4CA5-880D-B689245305D5 "بازکردن تصویر در اندازه اصلی")

ویرایش قرارداد ESB از عملیات ردیف

[![حذف تعریف سرویس انتخاب‌شده؛ قبل از اجرا مصرف‌کنندگان آن را بررسی کنید](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C5709238-9C70-4D81-9B2E-059BC2194A5D)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C5709238-9C70-4D81-9B2E-059BC2194A5D "بازکردن تصویر در اندازه اصلی")

حذف تعریف سرویس انتخاب‌شده؛ قبل از اجرا مصرف‌کنندگان آن را بررسی کنید

[![بازکردن سند مصرف سرویس از دکمهٔ مستند](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=90D4F52C-4DF5-4853-A651-2DF32CAA3B0C)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=90D4F52C-4DF5-4853-A651-2DF32CAA3B0C "بازکردن تصویر در اندازه اصلی")

بازکردن سند مصرف سرویس از دکمهٔ مستند

## خواندن سند تولیدشده

سند سرویس وضعیت فعال، اجازهٔ ناشناس، گیرندگان مجاز، URL، Method، Content-Type، ساختار درخواست و پاسخ، خطاها و نمونه‌های فراخوانی را کنار هم نشان می‌دهد. قبل از ارسال این سند به مصرف‌کننده، نام کاربران و سازمان را متناسب با مخاطب حذف کنید و نشانی استقرار مقصد را جایگزین نشانی نمونه کنید.

[![سند تولیدشدهٔ ESB: قرارداد HTTP، ورودی/خروجی، خطاها، ورود و نمونه‌های زبان‌های مختلف](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=74EDECD1-A614-46A9-95D6-84DDE35B1A3F)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=74EDECD1-A614-46A9-95D6-84DDE35B1A3F "بازکردن تصویر در اندازه اصلی")

سند تولیدشدهٔ ESB: قرارداد HTTP، ورودی/خروجی، خطاها، ورود و نمونه‌های زبان‌های مختلف

نمونهٔ این سند آدرس `/esb/run/CreatingAnEsb` و روش POST دارد. نام نهایی به تعریف شما وابسته است؛ آدرس و درگاه localhost نمونه را به محیط عملیاتی منتقل نکنید. پاسخ application/json این نمونه شیئی مانند `{"O2":0}` است، نه لزوماً پوستهٔ پاسخ RunProcess عمومی.

### ورودی و خروجی نمونه با نوع درست

```json
{
  "var1": { "RequestCode": "DEMO-001", "Quantity": 2 },
  "Variable2": "نمونه آموزشی"
}
```

```json
{ "O2": 0 }
```

برای var1 که Object تنظیم شده، Object یا Array معتبر بفرستید. بعضی نمونه‌های تولیدشده به‌جای آن رشتهٔ «???» نشان می‌دهند؛ آن placeholder کد آمادهٔ اجرا نیست. String بودن Variable2 نیز با عددی بودن خروجی O2 مستقل است.

## مستندات اختصاصی هر ESB

پس از ذخیره، هر ESB در فهرست سه اقدام «مستندات»، «ویرایش» و «حذف» دارد. مستندات را برای همان ESB باز کنید تا آدرس، متد، ورودی‌ها، خروجی‌ها و مجوزهای قرارداد انتخاب‌شده را ببینید؛ اگر دو ESB روی یک Process ساخته‌اید، یکسان‌بودن Process به معنی یکسان‌بودن قرارداد عمومی آن‌ها نیست.

[![چند ESB متعلق به یک Process و عملیات جداگانهٔ هر ردیف.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9FDBC558-2BB9-4AB2-9F8E-3DFF23485FB0)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9FDBC558-2BB9-4AB2-9F8E-3DFF23485FB0 "بازکردن تصویر در اندازه اصلی")

چند ESB متعلق به یک Process و عملیات جداگانهٔ هر ردیف.

[![مستند تولیدشدهٔ ESB شامل مشخصات قرارداد و احراز هویت.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C037C83B-284E-4177-A652-0CF3DE8ECE62)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C037C83B-284E-4177-A652-0CF3DE8ECE62 "بازکردن تصویر در اندازه اصلی")

مستند تولیدشدهٔ ESB شامل مشخصات قرارداد و احراز هویت.

در حالت غیرناشناس، «دسترسی‌های مجاز» از Laneِ Message Start خوانده می‌شود و بخش دریافت Token/ورود در مستندات در دسترس است. با مجوز ناشناس، این نمایش متناسب با قرارداد ناشناس تغییر می‌کند و راهنمای دریافت Token کاربر لازم نیست. نمونه‌های `curl`، `C#`، `fetch` و `jQuery` در تب‌های مستندات، شیوهٔ درخواست همان قرارداد را نشان می‌دهند. برای کد جدید Custom JS در این راهنما، نمونهٔ `fetch` مبنای پیشنهادی است؛ وجود تب jQuery به معنی الزام استفاده از آن نیست.

برای کپی نمونه، نام پارامترهای عمومی، متد و نوع محتوا را با آخرین تنظیمات ESB هماهنگ نگه دارید و مقدارهای نمونهٔ آدرس یا Token را با اطلاعات محیط خود جایگزین کنید. پس از تغییر نگاشت، مستند همان ESB را دوباره باز کنید؛ نمونهٔ یک قرارداد قدیمی مبنای مطمئنی برای ورودی تازه نیست.

## توکن، مجوز و خطاهای HTTP

برای سرویس بدون مجوز ناشناس، ابتدا `POST /SharedApi/Login` را با اطلاعات حساب مجاز فراخوانی کنید. نتیجه باید HasError=false داشته باشد و توکن غیرخالی برگرداند. سند ساختاری، توکن را در `Content.Token.PrivateToken` نشان می‌دهد. سپس در درخواست ESB یکی از هدرهای `privateToken` یا `Authorization: Bearer <PRIVATE_TOKEN>` را مطابق قرارداد به کار ببرید.

| کد | وضعیت | بررسی |
| --- | --- | --- |
| 401 | Unauthorized | سرویس ناشناس نیست و احراز هویت معتبر انجام نشده است. |
| 403 | Forbidden | هویت احراز شده ولی مجوز اجرای همین سرویس ندارد. |
| 500 | خطای اجرای سرویس | متن بدنهٔ خطا را بخوانید؛ تبدیل نوع، مقدار ورودی و اجرای فرآیند را بررسی کنید. |

متن فارسی خطا را از بدنهٔ پاسخ بخوانید و تحلیل را به StatusDescription محدود نکنید. در Login نیز HasError، Message و MessageException را بررسی کنید. موفقیت دریافت توکن، مجوز همهٔ ESBها را تضمین نمی‌کند.

**اختلاف مستند و نمونهٔ تولیدشده:** تصویر Fetch و jQuery به `authResult.PrivateToken` اشاره می‌کند، در حالی که ساختار Login داخل همان سند `Content.Token.PrivateToken` است. پیش از استفاده، مسیر را با پاسخ واقعی محیط تطبیق دهید. نمونه‌های زیر توکن معتبر دریافت‌شده را ورودی می‌گیرند و گذرواژه‌ای داخل کد قرار نمی‌دهند.

## نمونه‌های فراخوانی

نام‌ها، نشانی‌ها و مقادیر زیر آموزشی‌اند. متغیر privateToken باید از جریان احراز هویت مجاز برنامه تأمین شود. نمونه‌های تصویری، محل دریافت الگو در محصول را نشان می‌دهند؛ کد تایپی اصلاح‌شدهٔ زیر، نوع Object و مسیر توکن را روشن می‌کند.

### cURL

[![پنجرهٔ نمونهٔ cURL و امکان کپی متن تولیدشده](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4E2B9709-FBD4-45A8-A4B4-6D1FC9AD8FCD)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4E2B9709-FBD4-45A8-A4B4-6D1FC9AD8FCD "بازکردن تصویر در اندازه اصلی")

پنجرهٔ نمونهٔ cURL و امکان کپی متن تولیدشده

```bash
curl -X POST "https://bpms.example/esb/run/CreatingAnEsb"   -H "Authorization: Bearer <PRIVATE_TOKEN>"   -H "Content-Type: application/json; charset=utf-8"   --data '{"var1":{"RequestCode":"DEMO-001","Quantity":2},"Variable2":"demo"}'   --max-time 60
```

### Fetch با توکن موجود

[![نمونهٔ تولیدشدهٔ Fetch؛ مسیر توکن Login و placeholder نوع Object باید پیش از اجرا اصلاح شوند](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C8674EAE-4E18-4B27-87AB-C30FC57B4026)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C8674EAE-4E18-4B27-87AB-C30FC57B4026 "بازکردن تصویر در اندازه اصلی")

نمونهٔ تولیدشدهٔ Fetch؛ مسیر توکن Login و placeholder نوع Object باید پیش از اجرا اصلاح شوند

```javascript
async function callProcessEsb(privateToken) {
  const response = await fetch("https://bpms.example/esb/run/CreatingAnEsb", {
    method: "POST",
    headers: {
      "Content-Type": "application/json; charset=utf-8",
      "Authorization": "Bearer " + privateToken
    },
    body: JSON.stringify({
      var1: { RequestCode: "DEMO-001", Quantity: 2 },
      Variable2: "demo"
    })
  });
  const body = await response.text();
  if (!response.ok) throw new Error(`HTTP ${response.status}: ${body}`);
  return JSON.parse(body); // برای قرارداد application/json این سرویس
}
```

### jQuery با توکن موجود

[![نمونهٔ تولیدشدهٔ jQuery؛ بررسی خطای Login و کلید توکن را با قرارداد پاسخ یکسان کنید](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=16BC54F0-A5A9-41A3-88FE-A28F045CBC5C)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=16BC54F0-A5A9-41A3-88FE-A28F045CBC5C "بازکردن تصویر در اندازه اصلی")

نمونهٔ تولیدشدهٔ jQuery؛ بررسی خطای Login و کلید توکن را با قرارداد پاسخ یکسان کنید

```javascript
function callProcessEsbWithJQuery(privateToken) {
  return $.ajax({
    url: "https://bpms.example/esb/run/CreatingAnEsb",
    type: "POST",
    headers: { Authorization: "Bearer " + privateToken },
    contentType: "application/json; charset=utf-8",
    dataType: "json",
    data: JSON.stringify({
      var1: { RequestCode: "DEMO-001", Quantity: 2 },
      Variable2: "demo"
    })
  });
}
```

### C# در کلاینت مستقل

[![نمونهٔ تولیدشدهٔ C#؛ به‌جای ساخت دستی JSON با escapeهای رشته، بدنه را serialize کنید](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=CA395DB3-89E4-417B-B827-97290D8271AC)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=CA395DB3-89E4-417B-B827-97290D8271AC "بازکردن تصویر در اندازه اصلی")

نمونهٔ تولیدشدهٔ C#؛ به‌جای ساخت دستی JSON با escapeهای رشته، بدنه را serialize کنید

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

public static async Task<string> CallProcessEsbAsync(string privateToken)
{
    using var client = new HttpClient { Timeout = TimeSpan.FromSeconds(60) };
    client.DefaultRequestHeaders.Authorization =
        new AuthenticationHeaderValue("Bearer", privateToken);
    var body = new {
        var1 = new { RequestCode = "DEMO-001", Quantity = 2 },
        Variable2 = "demo"
    };
    using var content = new StringContent(
        JsonSerializer.Serialize(body), Encoding.UTF8, "application/json");
    using var response = await client.PostAsync(
        "https://bpms.example/esb/run/CreatingAnEsb", content);
    var text = await response.Content.ReadAsStringAsync();
    if (!response.IsSuccessStatusCode)
        throw new Exception($"HTTP {(int)response.StatusCode}: {text}");
    return text;
}
```

این نمونهٔ C# برای کلاینت مستقل با HttpClient و System.Text.Json است؛ API جدیدی برای Script Task معرفی نمی‌کند. سازگاری کتابخانه‌ها و نسخهٔ .NET مصرف‌کننده را در پروژهٔ مقصد کنترل کنید.

## آزمون و رفع اشکال قرارداد

| نشانه | بررسی و اقدام |
| --- | --- |
| افزودن ESB ممکن نیست | جدول سازگاری را اصلاح کنید؛ شمار آغاز/پایان و گام‌های بازدارنده را کنترل کنید. |
| متغیر در فهرست ورودی نیست | تعریف و ذخیرهٔ متغیر در همان نسخهٔ فرآیند را بررسی کنید. |
| خروجی در فهرست نیست | کلید خروجی را در End Event تعریف و ذخیره کنید. |
| ورودی می‌رسد ولی فرآیند مقدار را نمی‌خواند | نگاشت نام بیرونی به متغیر داخلی و نوع داده را کنترل کنید. |
| خروجی خالی یا نامنتظر است | رسیدن به پایان، مقدار کلید End Event، نگاشت خروجی، نوع و پیش‌فرض را جدا بررسی کنید. |
| 401 یا 403 | اعتبار توکن را از مجوز اجرای سرویس جدا کنید. |
| 500 | بدنهٔ خطا، نوع داده و مسیر اجرای فرآیند را بررسی کنید. |
| کد تولیدشده اجرا نمی‌شود | placeholderها، JSON escapeها، مسیر توکن و نوع Object را اصلاح کنید. |
| تغییر طراحی در سرویس دیده نمی‌شود | نسخهٔ متصل به ESB و ذخیرهٔ قرارداد را با نسخهٔ مورد انتظار تطبیق دهید. |

در آزمون پذیرش، ورودی معتبر، حذف پارامتر اختیاری، null، نوع ناسازگار، توکن نامعتبر و حساب فاقد مجوز را مستقل ثبت کنید. GET و POST و دو نوع محتوای پاسخ را فقط برای قراردادهایی که واقعاً تعریف کرده‌اید آزمایش کنید. نتیجهٔ موفق HTTP را با نتیجهٔ مورد انتظار فرآیند و خروجی End Event تطبیق دهید.
