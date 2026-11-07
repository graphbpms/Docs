---
id: 1626
title: "مرجع فنی داده و سرویس‌های BAM"
englishTitle: "BAM Data & Technical Reference"
category: "02-process/bam-monitoring"
wikiPath: "1626"
wikiUrl: "https://wiki.graphbpms.ir/1626"
breadcrumbs: ["مرجع فنی داده و سرویس‌های BAM"]
priority: 2
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# مرجع فنی داده و سرویس‌های BAM

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `مرجع فنی داده و سرویس‌های BAM`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1626](https://wiki.graphbpms.ir/1626)

# BAM — مرجع فنی داده و سرویس‌ها

این مقاله برای توسعه‌دهنده و تحلیل‌گر داده، قرارداد مسیرهای BAM، نگاشت ورودی و خروجی گزارش‌ها، ۱۹ رویه SQL و نکات یکپارچگی رابط را توضیح می‌دهد. راهنمای کاربری در مقاله «PROCESS-04 پایش فرآیند — BAM» قرار دارد.

## دامنه مرجع فنی
**نیازمند تأیید فنی:** انطباق قراردادها، Providerهای ثبت‌شده، نسخه SQL و فایل‌های رابط با محیط مستقر باید پیش از اتکا به نتایج محاسباتی تأیید شود. امضاهای زیر مرجع خواندن و عیب‌یابی‌اند؛ اجرای مستقیم یا تغییر رویه در پایگاه عملیاتی جزو این آموزش نیست.

## از یک انتخاب در صفحه تا داده‌ای که در گزارش دیده می‌شود

صفحهٔ `ProcessBAM` از یک کنترلر عمومی گزارش استفاده می‌کند. برای هر گزارش متد جداگانه‌ای در کنترلر نوشته نشده است؛ نام گزارش به یک Provider ثبت‌شده نگاشت می‌شود و Provider با پارامترهای همان گزارش اجرا می‌شود. بنابراین ساختار ستون‌های گزارش از نتیجهٔ Provider و رویهٔ مربوط می‌آید؛ کنترلر BAM در مسیر عمومی، ستون‌ها را دوباره محاسبه نمی‌کند.

**زنجیرهٔ اجرا:** کنترل ورودی در صفحه ← تابع متناظر در `bam.js` ← مسیر `/ProcessBAM/Report/{reportName}` ← `LoadReportAsync` ← نگاشت نام به شناسهٔ Provider ← `RunProviderAsync` ← جدول نتیجه ← `ResponseModel.Content` ← کارت، نمودار یا گرید.

ورود به کنترلر نیازمند احراز هویت است و کلاس با `[Authorize]` مشخص می‌شود. مسیر `/ProcessBAM` نمای اولیه را باز می‌کند. ثبت Providerهای محیط مستقر مرجع نگاشت اجرایی است و انطباق آن با قرارداد این مقاله نیازمند تأیید فنی است.

### واژه‌نامهٔ شناسه‌ها؛ سه عددی که نباید با هم جابه‌جا شوند

مفهومکلید فنیمعنی و کاربرد
گروه فرآیند`ProcessGroupId`هویت مشترک یک فرآیند در میان نسخه‌های مختلف آن. انتخاب `dqwdwqd` با شناسهٔ گروه `24273` یک تب BAM با دامنهٔ آن گروه می‌سازد. این شناسه، شناسهٔ یک بار اجرای فرآیند نیست.
فرآیند / نسخهٔ فرآیند`ProcessId`رکورد یک نسخهٔ مشخص از فرآیند در گروه. انتخاب «نسخهٔ فرآیند» در گزارش گام‌ها یا خطاهای گام، این شناسه را محدود می‌کند. API نسخه‌ها، `Id` و `Version` برمی‌گرداند.
نمونهٔ فرآیند`ProcessCreationId` یا `CreationId`یک اجرای واقعی از یک نسخهٔ فرآیند؛ دارای عنوان، شروع، وضعیت و احتمالاً پایان، لغو، ارجاع و خطا. بازکردن جزئیات، لاگ متغیرها و خروجی نمونه با این شناسه انجام می‌شود.
شناسهٔ عددی گام`ElementId`شناسهٔ رکورد المان در پایگاه داده. در بعضی خروجی‌ها همراه نام و نوع گام دیده می‌شود.
شناسهٔ BPMN گام`BpmnElementId`، `ElementBpmnId` یا `ElementID`شناسه رشته‌ای مانند `Activity_...` در نمودار BPMN با شناسه عددی رکورد گام تفاوت دارد. کلیدهای `BpmnElementId` و `ElementBpmnId` این مفهوم را بیان می‌کنند. `ElementID` نامی وابسته به زمینه است؛ در پارامتر جزئیات یک خطا می‌تواند عددی باشد. نوع و محل استفاده را کنترل کنید.
ارجاع`ReferencesID` / `ReferenceId`رکورد قرارگیری یا گردش کار در یک گام و نزد گیرنده. یک نمونه می‌تواند چند ارجاع داشته باشد؛ تعداد ارجاع الزاماً برابر تعداد نمونه نیست.
خطا`ExceptionId`یک رخداد خطا؛ یک نمونه ممکن است صفر، یک یا چند رخداد خطا داشته باشد. شمارندهٔ «خطا» را با «نمونهٔ دارای خطا» یکی نگیرید.
Lane`LaneId` / `LaneName`مسیر مسئولیت در مدل BPMN که گام در آن قرار دارد. ستون شناسه و نام Lane به تشخیص محل سازمانی یا منطقی گام کمک می‌کند.

### قرارداد پاسخ مشترک

پاسخ موفق کنترلر، شیء JSON زیر را برمی‌گرداند. مثال زیر شکل قرارداد را نشان می‌دهد؛ اعداد و متن گزارش واقعی نیستند. در گزارش‌های جدولی، `Content` آرایهٔ ردیف‌هاست. حتی گزارش تجمیعی تک‌ردیفی نیز ابتدا جدول است و فرانت در مواردی مثل اطلاعات عمومی و پراکندگی تجمیعی ردیف اول آن را مصرف می‌کند.

{
  "HasError": false,
  "ErrorCode": -1,
  "Message": "متن محلی‌سازی‌شدهٔ عملیات موفق",
  "MessageException": null,
  "Content": [ { "ColumnName": "value" } ]
}
عضو پاسخمعنی عملی
`HasError`نشانگر موفقیت یا خطای برنامه. مصرف‌کننده باید این مقدار را بررسی کند.
`ErrorCode`در سازندهٔ پاسخ موفق عمومی، مقدار `-1` ثبت می‌شود.
`Message`پیام قابل نمایش که در پاسخ موفق، از ترجمهٔ `OperationSuccess` گرفته می‌شود؛ متن آن می‌تواند با زبان سامانه تغییر کند.
`MessageException`جزئیات خطای پاسخ در مسیر خطا؛ با ستون «شرح خطا» در دادهٔ یک گزارش یکی نیست.
`Content`دادهٔ اصلی. نوع آن با endpoint تغییر می‌کند: جدول ردیف‌ها، لیست رشته‌ها، دیکشنری، یا نتیجهٔ صفحه‌بندی.

در `BaseController.OnException` خطاهای کنترلر به پاسخ JSON تبدیل می‌شوند و وضعیت HTTP روی `200` قرار می‌گیرد. بنابراین دریافت HTTP 200 به‌تنهایی به معنی موفقیت گزارش نیست؛ وضعیت `HasError` نیز باید بررسی شود. خالی‌بودن جدول، یک حالت داده‌ای مستقل از خطای اجرای گزارش است.

### مسیرهای اصلی BAM و کاربرد آن‌ها

درخواست GETورودیخروجی و نقش
`/ProcessBAM`نشست کاربرصفحهٔ HTML اولیه؛ دادهٔ کارت‌ها و گزارش‌ها سپس جداگانه خوانده می‌شود.
`/ProcessBAM/Report/GetReports`بدون پارامتر گزارش`Content` یک نگاشت نام گزارش به شناسهٔ Provider است؛ برای مثال نام بدون پیشوند `ProcessBAM_` کلید نگاشت می‌شود.
`/ProcessBAM/Report/{reportName}`نام گزارش در مسیر و دیکشنری `parameters` در Query Stringجدول خروجی Provider در `Content`. مسیر مشترک بیشتر گزارش‌های این صفحه.
`/ProcessBAM/GetExceptionsTypes`بدون ورودیلیست رشته‌های نوع خطا از `ProcessBAM_GetExceptionsTypes`؛ منبع انتخابگر «نوع خطا».
`/ProcessBAM/GetProcessOutputVariables``processId` و `processCreationId`دیکشنری نام → مقدار خروجی آخرین EndEvent اجراشده. فرانت آن را به ردیف‌های `Name` و `Value` تبدیل می‌کند.

### شکل پارامترهای گزارش

کلیدهای گزارش به شکل `parameters[نام‌پارامتر]` ارسال می‌شوند. نمونهٔ درخواست زیر فقط برای خواندن روند زمانی گروه منتخب است؛ مقادیر تاریخ در Query String باید مطابق قرارداد Provider و با URL encoding ارسال شوند. این نمونه، بازهٔ پنج سالهٔ میلادی را برای توضیح قرارداد نشان می‌دهد.

GET /ProcessBAM/Report/GetSummaryPerDayByProcessGroupId
  ?parameters[ProcessGroupId]=24273
  &parameters[FromDateTime]=2021-09-30T00%3A00%3A00
  &parameters[ToDateTime]=2026-09-30T23%3A59%3A59
  &parameters[IncludeCopies]=false
  &parameters[BasedOnCreationDate]=true
  &parameters[BasedOnCreationCount]=true
خانوادهٔ گزارشورودی‌های ارسال‌شده توسط فرانتنوع نتیجه و نکتهٔ کاربردی
روند عملکرد`FromDateTime`، `ToDateTime`، `IncludeCopies`، `BasedOnCreationDate`، `BasedOnCreationCount`؛ در تب گروهی: `ProcessGroupId`چند ردیف سری زمانی. مبنای تاریخ و مبنای شمارش دو ورودی مستقل‌اند؛ سوئیچ رونوشت نیز ورودی مستقل است.
پراکندگی`FromDateTime`، `ToDateTime` و در نسخهٔ گروهی `ProcessGroupId`ردیف‌های گروه‌ها در نمای کلی یا نسخه‌ها در تب گروه. دادهٔ نمودار و جدول از این خانواده تأمین می‌شود.
سرجمع پراکندگیورودی‌های پراکندگی به‌علاوهٔ `ExcludedIds`یک ردیف تجمیعی. مقدار خالی `ExcludedIds` از پارامتر ارسالی حذف می‌شود؛ شناسه‌های مستثناشده روی سرجمع و میانگین اثر دارند.
اطلاعات عمومیبرای هر شاخص یک زوج تاریخ مستقل، `ProcessGroupId` در تب گروه و چهار پارامتر ترجمهیک ردیف با شمارنده‌ها، مخرج‌ها و درصدها. تغییر بازهٔ یک کارت لزوماً بازهٔ تمام کارت‌های دیگر را عوض نمی‌کند.
ریز خطاهاگروه، نسخه، گام، نوع گام، بازهٔ وقوع، شناسه/عنوان/شرح/نوع خطا، نقش یا کاربر مالک خطا، عنوان/شناسه/وضعیت نمونه، والد و دستهٔ خطاهر ردیف یک رخداد خطای واجد شرایط. کلید فرانت `TypeId` به پارامتر بکند `ExceptionCategory` تبدیل می‌شود.
فهرست نمونه‌هاگروه، نسخه، عنوان/شناسهٔ نمونه، فعال/معلق بودن فرآیند، گروه ماژول، کاربر شروع/پایان/لغو، بازهٔ شروع/پایان/لغو، وضعیت پایان/لغو، توضیح لغو و مشخصات والدهر ردیف یک نمونه. فیلترهای وضعیت فرآیند با وضعیت پایان/لغو نمونه متفاوت‌اند.
مدت گام‌ها`FromDateTime`، `ToDateTime`، `ProcessGroupId`، `IncludeCopies` و در صورت انتخاب `StepTypeId` و `ProcessId`ردیف‌های تجمیع‌شدهٔ گام. رابط فعلی نسخهٔ `ByProcessGroupId` را در تب فرآیند صدا می‌زند.
خطاهای گام‌ها`FromDateTime`، `ToDateTime`، `ProcessGroupId` و در صورت انتخاب `StepTypeId` و `ProcessId`تعداد خطای منتسب به گام؛ انتخابگر نوع گام فرانت با نام داخلی `ExeptionTypeId` به `StepTypeId` نگاشت می‌شود، نه به رشتهٔ `ExceptionType`.
جزئیات نمونه`ProcessCreationId`جدول جزئیات گام‌ها/ارجاع‌ها از `GetProcessCreationDetails` برای یک نمونه.
تاریخچهٔ متغیرها`ProcessesCreationsIds`لاگ متغیرهای نمونه یا فهرستی از شناسه‌ها. پنجرهٔ فعلی یک شناسهٔ نمونه ارسال می‌کند؛ مسیر Contributor می‌تواند لیست شناسه‌ها را به رشتهٔ جداشده با ویرگول تبدیل کند.

نام دقیق ورودی‌های ریز خطاها و فهرست نمونه‌ها برای تطبیق با درخواست شبکه
ریز خطاها`ProcessGroupId, ProcessId, ElementBpmnId, ExceptionOccurredDateTimeFrom, ExceptionOccurredDateTimeTo, ExceptionId, ExceptionTitle, ExceptionMessage, ExceptionType, ExceptionOwnerRoleId, ExceptionOwnerRoleName, ExceptionOwnerUserId, CreationTitle, CreationId, HasCreationFinished, HasCreationDiscarded, ParentProcessGroupId, ParentStepBpmnId, ParentStepTypeId, ParentCreationTitle, ElementTypeId, ExceptionCategory`

فهرست نمونه‌ها
`ProcessGroupId, CreationTitle, CreationId, ProcessId, IsProcessActive, IsProcessSuspended, ProcessGroupModuleGroupId, StarterUserId, StartRangeOfStartDateTime, EndRangeOfStartDateTime, Finish, FinisherUserId, StartRangeOfEndDateTime, EndRangeOfEndDateTime, Discard, DiscarderUserId, DiscardDescription, StartRangeOfDiscardDate, EndRangeOfDiscardDate, ParentProcessGroupId, ParentStepBpmnId, ParentCreationTitle, ParentStepTypeId`

مقادیر `null` یا `undefined` در سازندهٔ درخواست‌های این دو خانواده عموماً ارسال نمی‌شوند. در حالت کلی، فرانت برای تشخیص نمای کل سامانه از شناسهٔ داخلی `-1` استفاده می‌کند و در آن حالت پسوند گروهی و پارامتر گروه حذف می‌شوند.

### Provider و ترجمه؛ دو مرحلهٔ پنهان در اجرای گزارش

`GetProcessBAMProviders` شناسه و نام Providerهایی را می‌خواند که نامشان با الگوی `ProcessBAM_%` تطبیق دارد. Contributor برای کلید دیکشنری، یازده نویسهٔ نخست نام یعنی پیشوند `ProcessBAM_` را حذف می‌کند. برای نمونه، مسیر `GetAggregatedGeneralInfo` به Provider هم‌نام با پیشوند BAM نگاشت می‌شود. خود Controller تضمین نمی‌کند هر نام مستقیماً هم‌نام یک Stored Procedure باشد؛ این اتصال را پیکربندی Provider مشخص می‌کند.

نگاشت Providerها در بکند با کلید `AllProcessBAMProviders` و `ICacheManagerContributor.GetOrCreateAsync` کش می‌شود. این کش، **کش فهرست Providerها** است؛ با کش پاسخ گزارش در مرورگر متفاوت است. در مسیر عمومی `LoadReportAsync` پس از یافتن شناسه، پارامترها به `ParametersModel` تبدیل و Provider اجرا می‌شود.

اگر نام یک پارامتر شامل `_Translate` باشد، مقدار ارسالی مرورگر مستقیماً به Provider داده نمی‌شود. Contributor بخش پیش از `_Translate` را کلید ترجمه می‌گیرد، مقدار ترجمه‌شده را از `LanguageService` می‌خواند و با همان نام پارامتر به Provider می‌دهد. چهار کلید زیر در گزارش اطلاعات عمومی و خطاها برای تطبیق برخی دسته‌ها استفاده می‌شوند:

`InvalidOutputServiceData_TranslateErrorInGetValVar_TranslateAnExceptionOccurredDuringStartingOrContinuingProcess_TranslateThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate`

### APIهای کمکی انتخابگرها

مسیرکاربرد و شکل داده
`/Process/GetAllProcessesGroups`انتخاب گروه فرآیند. ورودی‌های `PageSize` و `PageNumber` را می‌پذیرد؛ `Content` شامل `TotalItemsCount`، `TotalPagesCount` و `Models` است. ردیف‌های گروه شامل `Id`، `Name`، `CreateDate` و `ModuleGroupId` هستند.
`/Process/GetProcessGroupVersions`ورودی `processGroupId`؛ خروجی لیست `{ Id, Version }`. برای محدودکردن گزارش به نسخهٔ مشخص استفاده می‌شود.
`/Process/GetElementsByProcessGroupId`ورودی `processGroupId`؛ فهرست المان‌های گروه، برای انتخاب گام و فیلترهای مربوط به گام یا والد.
`/Process/GetElementsTypes`انواع المان BPMN برای انتخابگر نوع گام؛ مانند `UserTask` یا `ScriptTask`.
`/Process/GetRolesList`ردیف‌های `RoleId`، `RoleName`، `OrganizationId` و `OrganizationTitle` برای فیلتر نقش.
`/Process/GetAllUsersList`ردیف‌های `UserId`، `FullName`، `OrganizationId`، `OrganizationTitle`، `UserName` و `IsSystemUser` برای انتخاب کاربران مرتبط.

### خروجی نهایی نمونه با تاریخچهٔ متغیرها چه تفاوتی دارد؟

در پنجرهٔ تاریخچهٔ متغیرها، هر ردیف یک ثبت از مقدار متغیر در زمان مشخص است و ممکن است یک نام متغیر چند بار تکرار شود. اما پنجرهٔ خروجی نهایی، نام و مقدار خروجی‌های تعریف‌شده برای **آخرین EndEvent اجراشده** را نمایش می‌دهد؛ همهٔ متغیرهای نمونه یا همهٔ ثبت‌های تاریخچه را فهرست نمی‌کند.

- بکند با `Process_GetLastExecutedEndEvent` آخرین EndEvent نمونه را پیدا می‌کند.

- اگر EndEvent پیدا نشود، دیکشنری خالی بازمی‌گردد؛ در نتیجه نبود ردیف خروجی می‌تواند ناشی از نرسیدن نمونه به رویداد پایان باشد.

- بکند زمینهٔ اجرای آن رخداد، شامل نسخه، نمونه، گام، گیرنده، نقش، مسیر و آخرین ارجاع را می‌سازد.

- خروجی‌های تنظیم‌شدهٔ EndEvent خوانده می‌شوند. نوع ثابت، مقدار متغیر، حالت پیشرفته/ساده و تابع طبق تنظیمات خودشان محاسبه می‌شوند. برای خواندن مقادیر تاریخی، زمان `ExitCartableDateTime` همان EndEvent به عنوان `asOfDate` ارسال می‌شود.

- پاسخ به صورت دیکشنری نام → مقدار بازمی‌گردد؛ فرانت آن را برای گرید دو ستونی تبدیل می‌کند.

فیلد لاگ متغیرمعنی
`Name`نام متغیری که مقدار آن ثبت شده است.
`Value`مقدار ثبت‌شده در همان رخداد؛ ممکن است متن، مقدار ساده یا محتوای ساختاری باشد.
`CreateDate`زمان ثبت مقدار.
`ProcessCreationId`نمونه‌ای که این ثبت به آن تعلق دارد.
`CreatorUserId` و `CreatorUserName`شناسه و نام کاربر ثبت‌کننده.
`Reference`اطلاعات مرجع ثبت متغیر، مطابق خروجی رویه.
`UserTaskFormId`شناسهٔ اختیاری فرم مرتبط با UserTask.

### مرز دقیق کش گزارش و درخواست مستقیم جزئیات

گزارش‌های اصلی صفحه و انتخابگرهای کمکی از مسیر `CachedAjax` عبور می‌کنند. در کنار آن‌ها، سه پنجرهٔ **جزئیات نمونه، تاریخچهٔ متغیرها و خروجی نهایی نمونه** در قرارداد رابط از `$.ajax` مستقیم استفاده می‌کنند؛ هنگام بازشدن آن‌ها درخواست به بکند ارسال می‌شود. بنابراین تعبیر «تمام پاسخ‌های بکند بدون استثنا در کش فرانت‌اند هستند» با قرارداد رابط دقیق نیست. توضیح کش بخش کاربری این سند ناظر به گزارش‌هایی است که از مسیر کش عبور می‌کنند.

همچنین کش نتیجهٔ گزارش در مرورگر، ذخیرهٔ تنظیمات تب‌ها و تاریخ‌ها در `localStorage`، و کش نگاشت Providerها در سرور سه نقش جدا دارند. رفرش گزارش برای خواندن دادهٔ تازه است؛ باز و بسته‌کردن یک تب یا بازیابی تنظیمات قبلی به معنی تغییر دادهٔ پایگاه داده نیست.

مدل داخلی وضعیت نمونه، ارجاع‌ها و استثناها

قرارداد `IProcessBAMContributor` علاوه بر گزارش‌های صفحه، امکانات مشترکی برای وضعیت نمونه و ردگیری ارجاع دارد. این قرارداد داخلی را با پاسخ جدولی `GetProcessCreationDetails` در BAM یکی نگیرید.

عضو مدل ترکیبیمحتوا و کاربرد
`CreationInfo``Id, ProcessId, Title, CreateDateTime, FinishDateTime, ExceptionOccurred, ShouldDeleteReferencesAfterFinish`؛ اطلاعات اصلی و نشانگر خطای نمونه.
`ReferencesDetails`ارجاع‌ها همراه مشخصات گام، گیرنده، فرم و خطای منتسب به همان ارجاع.
`OtherExceptions`خطاهای دیگری که در خروجی اختصاصی خطاها برگردانده شده‌اند.
`VariablesLogs`فهرست ثبت مقادیر متغیرها با مدل معرفی‌شده در بالا.
`OutputVariables`دیکشنری خروجی آخرین EndEvent.

اگر مقدار `ExceptionOccurred` از اطلاعات اصلی تعیین نشده باشد، سازندهٔ مدل آن را از وجود `OtherExceptions` یا خطای دارای شناسه در `ReferencesDetails` استنتاج می‌کند. این نشانگر فقط وجود خطا را بیان می‌کند؛ شمارندهٔ تعداد خطا نیست.

دستهٔ فیلدهای ارجاعفیلدهای اصلی
هویت و گام`ReferencesID, ELementTitle, ElementTypeId, ElementId, BpmnElementId, ReferencesRead, ReferencesCreateDateTime`
گیرنده`FKRecieverUserID, RecieverUserFullName, FKRecieverRoleID, RecieverRoleName`
خواندن و پیش‌نویس`DraftFormState, IsView, ViewedAt, ReferenceNoteExplain`
موجودیت و فرم`Url, EntityId, EntityTitle, EntityTypeId, ElementConfigId, EntityMode, UserTaskFormRecordId`
خروجی و مسیر`OutputVariableLogId, OutputVariableName, OutputVariableValue, LastReferenceId, IsUserTaskCopied`
فرآیند فرزند`ChildProcessId, ChildProcessTestEnviromentId, ChildProcessCreationId`
خطای همراه`ExceptionId, ExceptionOccurredDateTime, ExceptionTitle, ExceptionMessage, ExceptionType, ExceptionOwner, ExceptionParameters, ExceptionErrorType`

متد `GetProcessCreationStatusReferencesAndExceptionsAsync` رویه `ProcessBAM_GetProcessCreationStatusReferencesAndExceptions` را با چند مجموعه نتیجه می‌خواند. `GetReferencesTraceAsync` نیز `ProcessBAM_GetProcessCreationReferencesTrace` را اجرا می‌کند. این دو قرارداد کمکی علاوه بر ۱۹ رویه گزارش این مقاله هستند.

### نکات سازگاری قرارداد و رابط

موضوعنتیجهٔ تطبیق و شیوهٔ استفاده در این سند
تعداد رویه‌هااین مرجع ۱۹ رویه را پوشش می‌دهد: ۱۸ گزارش اصلی و `ProcessBAM_GetProcessesCreationsVariablesLogs` برای تاریخچه متغیرها.
گام‌ها در نمای فعلیقرارداد مدت گام‌ها و خطاهای گام‌ها هر دو نسخه عمومی و گروهی دارد؛ رابط دو جدول جزییات آماری را در تب گروه با مسیرهای گروهی فراخوانی می‌کند. وجود رویه عمومی به معنی وجود جدول مشابه در نمای کلی نیست.
تعبیر نام «نمونه» در عنوان‌هاتعداد نمونه دارای خطا با تعداد رخداد خطا متفاوت است. واحد شمارش را از منطق محاسبه بخوانید؛ عنوان نمایشی و نام قدیمی یک کلید واحد شمارش را تغییر نمی‌دهد.
«براساس رونوشت‌ها»`IncludeCopies` نوع ارجاع را انتخاب می‌کند: اصلی یا رونوشت. از نام انگلیسی آن نباید نتیجه گرفت که حالت روشن «اصلی به‌اضافه رونوشت» است.
مجموع درصدهاتعبیر مجموع وضعیت‌های جاری، پایان‌یافته و لغوشده برای یک دامنه و بازهٔ همسان معتبر است. چون کارت‌ها ورودی تاریخ مستقل دارند، پس از تغییر جداگانهٔ بازهٔ کارت‌ها نباید انتظار داشت مجموع درصدهای دیده‌شده الزاماً ۱۰۰ باشد.

## پیوست فنی: قرارداد واقعی ۱۹ رویه پایش فرآیند

### واژه‌ها و قواعد مشترک خواندن اعداد

- **گروه فرآیند** خانواده یک فرآیند در ProcessGroup است. **نسخه فرآیند** یک ProcessList.Id متعلق به آن گروه است. **نمونه** اجرای یک فرآیند در ProcessCreation و **ارجاع** رکورد کارتابل/اجرای گام در ProcessReferences است. این شناسه‌ها قابل جایگزینی با یکدیگر نیستند.

- تاریخ‌های SQL میلادی و از نوع DATETIME یا DATETIME2 هستند. SQL در این رویه‌ها تبدیل منطقه زمانی یا تقویم جلالی انجام نمی‌دهد. تبدیل/نمایش جلالی وظیفه لایه برنامه است. مرزهای بازه عموماً فراگیرند؛ پایان امروز باید همراه ساعت پایان روز باشد.

- NULL در ورودی اختیاری معمولاً یعنی نبود آن فیلتر. با صفر، رشته خالی یا تاریخ صفر تفاوت دارد. بازه وارونه در بیشتر رویه‌ها اعتبارسنجی اختصاصی ندارد و معمولاً خروجی خالی می‌دهد.

- بخش زیادی از آمار و فهرست‌ها WITH (NOLOCK) دارند. در سامانه در حال تغییر، چند پرس‌وجو الزاماً تصویر تراکنشی واحد ندارند و اختلاف موقت اعداد ممکن است رخ دهد. جداول موقت # در SQL کش مرورگر نیستند و در پایان رویه حذف می‌شوند.

- بیشتر آمار نمونه‌ها TestEnvironmentId IS NULL را اعمال می‌کند؛ استثناهای مهم در بخش جزئیات یک نمونه، لاگ متغیرها و فهرست نوع خطا صریحاً ذکر شده‌اند.

- هیچ‌یک از این ۱۹ رویه در امضای خود پارامتر صفحه‌بندی ندارند؛ صفحه‌بندی، ستون‌بندی، خروجی فایل و بازشدن مدال رفتار لایه برنامه هستند. اکثر رویه‌ها یک مجموعه نتیجه دارند؛ خلاصه زمانی یک مجموعه نتیجه با دو شکل ستون زمان تولید می‌کند.

### دسته‌بندی دقیق خطاها و کد ریزگزارش
کددستهنام شمارنده در تجمیعقاعده عضویت1کل خطاها`OccurredExceptionCount`همه رکوردهای خطای نمونه‌های غیرآزمایشی؛ فیلترهای عمومی همچنان اعمال می‌شوند.2مدیریت‌شده توسط Boundary`AutoHandledOccurredExceptionCreationsCount`برای المان خطا Boundary متصل در همان نسخه فرآیند وجود داشته باشد؛ اتصال با AttachedTo و FK_ProcessID بررسی می‌شود.3مدیریت‌نشده توسط Boundary`NotAutoHandledOccurredExceptionCreationsCount`برای المان خطا چنین Boundaryای پیدا نشود.4خطای دسترسی`AccessViolationOccurredExceptionCreationsCount`ExceptionType = 'AccessViolationException'.5سرویس خارجی`ExternalOccurredExceptionCreationsCount`نوع المان ServiceTask (۲)، دارای ProcessServiceTaskConfig و Provider با DataType برابر ۶ (WSDL) یا ۷ (REST).6نمونه آغازشده با سرویس`StartedAsServiceOccurredExceptionCreationsCount`وجود نام متغیر @@StartFromService در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.7نمونه آغازشده با ESB`StartedAsESBOccurredExceptionCreationsCount`وجود نام متغیر @@StartFromESB در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.8مهلت‌زمان / Timeout`TimeoutOccurredExceptionCreationsCount`ExceptionType = 'TimeoutException' یا تطبیق دقیق پیام SQL timeout تعریف‌شده در کد.9نبود مسیر پیش‌فرض Gateway`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsCount`ExceptionMessage دقیقاً 'Gateway has not default sequence flow'.10خطا در نمونه خاتمه‌یافته`OccurredExceptionFinishedCreationsCount`وضعیت فعلی Finish نمونه برابر ۱؛ الزام نمی‌کند خطا پس از خاتمه رخ داده باشد.11خروجی نامعتبر سرویس`InvalidOutputServiceDataOccurredExceptionCreationsCount`ExceptionTitle برابر متن پارامتر InvalidOutputServiceData_Translate.12دریافت مقدار متغیر`ErrorInGetVariableValueOccurredExceptionCreationsCount`ExceptionTitle برابر متن پارامتر ErrorInGetValVar_Translate.13شروع با تایمر`AnExceptionOccurredDuringStartingProcessCreationsCount`عنوان برابر ترجمه شروع/ادامه و نوع المان ۱۸ (Start Timer).14ادامه با تایمر`AnExceptionOccurredDuringContinuingProcessCreationsCount`عنوان برابر ترجمه شروع/ادامه و نوع المان ۲۳ (Intermediate Timer).15نبود مسیر ادامه / نمونه لغوشده`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsCount`برابری دقیق عنوان با ترجمه مربوط؛ نام دسته به‌تنهایی اثبات لغوشدن وضعیت نمونه نیست.16سایر خطاها`OtherExceptionCreationsCount`خطای بدون Boundary که هیچ‌یک از شرط‌های دسته‌های ۴ تا ۱۵ را ندارد. بازه آن در تجمیع از NotAutoHandledOccurredException گرفته می‌شود.
به‌جز نمونه یکتای دارای خطا، ستون‌های دسته‌بندی فوق تعداد رکورد خطا هستند. دسته‌های موضوعی می‌توانند هم‌پوشانی داشته باشند؛ برای مثال خطای Timeout یک سرویس خارجی می‌تواند در هر دو دسته شمرده شود. برچسب مدیریت‌شده بر پایه تنظیم Boundary است، نه شاهد اجرای موفق آن.

### مبنای زمان در خلاصه عملکرد
خانواده دادهBasedOnCreationDate=1BasedOnCreationDate=0جاریCreateDate نمونهCreateDate نمونهخاتمهCreateDate نمونهEndCreateDate و در نبود آن CreateDateلغوCreateDate نمونهDiscardProcessDate و در نبود آن CreateDateکارتابل/رونوشتCreateDate نمونهReferencesCreateDate در Process_InterimInboxDataارسال فرمCreateDate نمونه؛ میانگین مدت در سطح نمونهExitCartableDateTime هر ارجاع؛ مدت همان ارجاعخطاCreateDate نمونهCreateDate خطا در ProcessErrorLogs

### 1. تجمیع پراکندگی در کل سامانه

`ProcessBAM_GetAggregatedDispersions`

یک ردیف شامل میانگین ایجاد روزانه، تعداد جاری و تعداد خاتمه‌یافته بازمی‌گرداند. داده از ProcessCreation و ProcessList و با حذف محیط آزمایشی نمونه خوانده می‌شود. فیلتر تاریخ روی CreateDate نمونه است.

ExcludedIds در این نسخه شناسه گروه فرآیند است. STRING_SPLIT و TRY_CAST ورودی‌های غیرقابل‌تبدیل به INT را حذف می‌کنند و DISTINCT شناسه تکراری را حذف می‌کند. پس از حذف گروه‌ها، برای تاریخ‌های NULL کمینه/بیشینه CreateDate همین دامنه انتخاب می‌شود.

تعداد روز = DATEDIFF(DAY, شروع مؤثر, پایان مؤثر)+1. بازه وارونه به تعداد روز ۱ محدود می‌شود، اما شرط تاریخ معمولاً نمونه‌ای برنمی‌گرداند. اگر تاریخ مؤثر یا مجموعه نمونه‌ها خالی باشد میانگین ۰٫۰ است.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@ExcludedIds``VARCHAR(MAX)``NULL`رشته شناسه‌های جداشده با ویرگول؛ این شناسه‌ها از تجمیع کنار گذاشته می‌شوند. در حالت کل، شناسه گروه و در حالت گروه، شناسه نسخه فرآیند است.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`DailyAverageCreations`تعداد کل نمونه‌های باقی‌مانده در بازه تقسیم بر تعداد روزهای تقویمی فراگیر بازه؛ نوع محاسبه FLOAT. لغوشده‌ها نیز در صورت کسر هستند.`ActiveCreationsCount`تعداد نمونه جاری (Finish=0 و Discard≠1)، COUNT_BIG.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- صورت میانگین همه نمونه‌ها، شامل لغوشده‌هاست؛ اما دو شمارنده جاری و خاتمه لغوشده‌ها را حذف می‌کنند. بنابراین مجموع این دو الزاماً صورت میانگین نیست.
- ردیف‌های ProcessList با INNER JOIN لازم‌اند؛ برخلاف بعضی مخرج‌های پراکندگی، نمونه بدون نسخه معتبر در این تجمیع قرار نمی‌گیرد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessList`

CREATE OR ALTER PROCEDURE ProcessBAM_GetAggregatedDispersions
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @ExcludedIds VARCHAR(MAX) = NULL

### 2. تجمیع پراکندگی یک گروه فرآیند
`ProcessBAM_GetAggregatedDispersionsByProcessGroupId`

همان سه خروجی و فرمول رویه عمومی را فقط برای گروه اجباری ProcessGroupId تولید می‌کند. محدودسازی از ProcessList.FK_ProcessGroupID انجام می‌شود و بازه روی تاریخ ایجاد نمونه است.

تفاوت تعیین‌کننده: ExcludedIds در این رویه شناسه نسخه فرآیند (ProcessCreation.ProcessID / ProcessList.Id) است. حذف یک نسخه از جدول پراکندگی گروه، تجمیع را بدون نمونه‌های آن نسخه بازمی‌سازد.

تاریخ‌های مؤثر و تعداد روز پس از اعمال گروه و حذف نسخه‌ها محاسبه می‌شوند. تعداد روز فراگیر است و میانگین کل نمونه‌ها را، شامل لغوشده‌ها، در نظر می‌گیرد.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@ExcludedIds``VARCHAR(MAX)``NULL`رشته شناسه‌های جداشده با ویرگول؛ این شناسه‌ها از تجمیع کنار گذاشته می‌شوند. در حالت کل، شناسه گروه و در حالت گروه، شناسه نسخه فرآیند است.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`DailyAverageCreations`تعداد کل نمونه‌های باقی‌مانده در بازه تقسیم بر تعداد روزهای تقویمی فراگیر بازه؛ نوع محاسبه FLOAT. لغوشده‌ها نیز در صورت کسر هستند.`ActiveCreationsCount`تعداد نمونه جاری (Finish=0 و Discard≠1)، COUNT_BIG.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- ProcessGroupId گروه است؛ مقدار Version یا شناسه نمونه را نباید به آن فرستاد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessList`

CREATE OR ALTER PROCEDURE ProcessBAM_GetAggregatedDispersionsByProcessGroupId
    @ProcessGroupId INT,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @ExcludedIds VARCHAR(MAX) = NULL

### 3. پراکندگی نمونه‌ها میان گروه‌های فرآیند
`ProcessBAM_GetDispersions`

برای هر گروه دارای نسخه غیرآزمایشی در ProcessList یک ردیف بازمی‌گرداند؛ LEFT JOIN نمونه‌ها باعث می‌شود گروه بدون نمونه در بازه نیز با صفر دیده شود. تاریخ بر CreateDate نمونه اعمال می‌شود.

جاری: Finish NULL→0 و Discard NULL→0، سپس Finish=0 و Discard≠1. خاتمه: Finish=1 و Discard≠1. ترتیب SQL ابتدا تعداد خاتمه نزولی و سپس تعداد جاری نزولی است.

مخرج هر دو درصد، تعداد تمام نمونه‌های غیرآزمایشی در بازه است؛ نه تعداد جاری‌ها یا خاتمه‌یافته‌ها و نه صرفاً گروه‌های نمایان نمودار.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`Id`شناسه گروه فرآیند در گزارش عمومی؛ شناسه نسخه ProcessList در گزارش یک گروه.`Title`نام گروه فرآیند در گزارش عمومی؛ شماره Version در گزارش یک گروه.`InProcessCreationsCount`تعداد نمونه جاری این ردیف، بدون لغوشده‌ها.`InProcessCreationsPercentage`تعداد جاری این ردیف ×۱۰۰ تقسیم بر کل نمونه‌های غیرآزمایشی بازه در دامنه گزارش، نه فقط مجموع نمونه‌های جاری.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.`FinishedCreationsPercentage`تعداد خاتمه‌یافته ×۱۰۰ تقسیم بر مخرج گزارش؛ در پراکندگی، کل نمونه‌های بازه در دامنه گزارش است.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- مخرج از ProcessCreation مستقل محاسبه می‌شود؛ صورت، نسخه غیرآزمایشی و پیوند معتبر گروه را لازم دارد. لغوشده‌ها در مخرج هستند اما در دو صورت نیستند؛ مجموع درصدها الزاماً ۱۰۰ نیست.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessGroup`، `dbo.ProcessList`

CREATE OR ALTER PROCEDURE ProcessBAM_GetDispersions
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL

### 4. پراکندگی نمونه‌ها میان نسخه‌های یک گروه
`ProcessBAM_GetDispersionsByProcessGroupId`

برای هر نسخه غیرآزمایشی ProcessList در گروه یک ردیف می‌دهد. Id شناسه نسخه و Title مقدار Version است؛ در حالی‌که نسخه عمومی Id گروه و Title نام گروه می‌دهد.

صورت‌ها همان جاری/خاتمه بدون لغو و مخرج تمام نمونه‌های غیرآزمایشی بازه مربوط به گروه انتخابی است. بازه روی CreateDate اعمال می‌شود. نسخه بدون نمونه نیز با صفر بازمی‌گردد.

مرتب‌سازی: FinishedCreationsCount نزولی، سپس InProcessCreationsCount نزولی.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`Id`شناسه گروه فرآیند در گزارش عمومی؛ شناسه نسخه ProcessList در گزارش یک گروه.`Title`نام گروه فرآیند در گزارش عمومی؛ شماره Version در گزارش یک گروه.`InProcessCreationsCount`تعداد نمونه جاری این ردیف، بدون لغوشده‌ها.`InProcessCreationsPercentage`تعداد جاری این ردیف ×۱۰۰ تقسیم بر کل نمونه‌های غیرآزمایشی بازه در دامنه گزارش، نه فقط مجموع نمونه‌های جاری.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.`FinishedCreationsPercentage`تعداد خاتمه‌یافته ×۱۰۰ تقسیم بر مخرج گزارش؛ در پراکندگی، کل نمونه‌های بازه در دامنه گزارش است.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- این رویه ورودی حذف نسخه ندارد؛ حذف انتخابی در تجمیع مکمل با ExcludedIds انجام می‌شود.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessList`

CREATE OR ALTER PROCEDURE ProcessBAM_GetDispersionsByProcessGroupId
    @ProcessGroupId INT,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL

### 5. شاخص‌های جامع کل سامانه
`ProcessBAM_GetAggregatedGeneralInfo`

این رویه یک ردیف آماری جامع برای کارت‌های وضعیت نمونه و کارت‌های خطا تولید می‌کند. هر کارت زوج تاریخ اختصاصی دارد؛ تغییر یک زوج تاریخ، مخرج همان کارت را نیز تغییر می‌دهد. چهار پارامتر ترجمه پیش‌فرض ندارند و باید هنگام فراخوانی تأمین شوند؛ امکان ارسال صریح NULL به SQL همچنان وجود دارد.

ابتدا همه نمونه‌های غیرآزمایشی در #CreationsDates، خطاهای مرتبط با همین نمونه‌ها در #Exceptions و نام متغیرهای @@StartFromService و @@StartFromESB در #VariablesLogs گردآوری می‌شوند. وضعیت‌های NULL نمونه به صفر تبدیل می‌شوند. جدول موقت Boundary از ProcessBoundaryEventConfig و ProcessElements ساخته می‌شود.

کارت‌های جاری، خاتمه، لغو، کارتابل/رونوشت و نمونه دارای خطا همگی با تاریخ ایجاد نمونه فیلتر می‌شوند. کارت‌های تعداد خطا و دسته‌های خطا با تاریخ وقوع خطا فیلتر می‌شوند. مثلاً نمونه ایجادشده در بازه که بعداً خطا داده است، در شمارنده نمونه دارای خطا می‌ماند.

نسبت هر کارت Count×100.0/TotalCount است و در مخرج صفر، مقدار صفر بازمی‌گردد. برای کارت‌های وضعیت، TotalCount کل نمونه‌های همان بازه است. برای دسته خطا، TotalCount تمام خطاهای همان بازه اختصاصی است.

کارتابل دو سطح دارد: شمارش نمونه یکتا با حداقل یک ارجاع اصلی یا رونوشت، و شمارش خود ردیف‌های ارجاع. یک نمونه می‌تواند هم در اصلی و هم در رونوشت قرار گیرد. کارتابل از Process_InterimInboxData فعلی خوانده می‌شود و تاریخ آن تاریخ ایجاد نمونه است.

ProcessesGroupsCount تعداد گروه‌های متمایز در نسخه‌های غیرآزمایشی است و با فیلتر تاریخ تغییر نمی‌کند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@InvalidOutputServiceData_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان «خروجی نامعتبر سرویس» برای تشخیص دسته ۱۱؛ برابری عنوان خطا ملاک است.`@ErrorInGetValVar_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان «خطا در دریافت مقدار متغیر» برای دسته ۱۲.`@AnExceptionOccurredDuringStartingOrContinuingProcess_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان خطای شروع/ادامه؛ همراه نوع المان ۱۸ یا ۲۳ برای دسته ۱۳ یا ۱۴.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان نبود مسیر ادامه/لغو نمونه برای دسته ۱۵.`@FinishedStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه خاتمه‌یافته»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@FinishedEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه خاتمه‌یافته»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NewStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه جاری»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NewEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه جاری»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@DiscardStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه لغوشده»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@DiscardEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه لغوشده»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InCartableStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «کارتابل و رونوشت»؛ مبنا: تاریخ ایجاد نمونه، نه تاریخ ورود ارجاع. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InCartableEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «کارتابل و رونوشت»؛ مبنا: تاریخ ایجاد نمونه، نه تاریخ ورود ارجاع. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionCreationStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه دارای خطا»؛ مبنا: تاریخ ایجاد نمونه؛ خود خطا می‌تواند خارج بازه رخ داده باشد. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionCreationEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه دارای خطا»؛ مبنا: تاریخ ایجاد نمونه؛ خود خطا می‌تواند خارج بازه رخ داده باشد. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «کل خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «کل خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AutoHandledOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دارای Boundary»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AutoHandledOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دارای Boundary»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NotAutoHandledOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای بدون Boundary و سایر خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NotAutoHandledOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای بدون Boundary و سایر خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AccessViolationOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دسترسی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AccessViolationOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دسترسی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ExternalOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای سرویس خارجی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ExternalOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای سرویس خارجی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsServiceOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نمونه آغازشده با سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsServiceOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نمونه آغازشده با سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsESBOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نمونه آغازشده با ESB»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsESBOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نمونه آغازشده با ESB»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@TimeoutOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای مهلت‌زمان»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@TimeoutOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای مهلت‌زمان»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@GatewayHasNoDefaultSequenceFlowOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نبود مسیر پیش‌فرض Gateway»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@GatewayHasNoDefaultSequenceFlowOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نبود مسیر پیش‌فرض Gateway»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionFinishedStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطا در نمونه خاتمه‌یافته»؛ مبنا: زمان وقوع خطا؛ وضعیت خاتمه در زمان خواندن. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionFinishedEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطا در نمونه خاتمه‌یافته»؛ مبنا: زمان وقوع خطا؛ وضعیت خاتمه در زمان خواندن. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InvalidOutputServiceDataOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خروجی نامعتبر سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InvalidOutputServiceDataOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خروجی نامعتبر سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ErrorInGetValVarOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دریافت مقدار متغیر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ErrorInGetValVarOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دریافت مقدار متغیر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringStartingProcessStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای شروع با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringStartingProcessEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای شروع با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringContinuingProcessStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای ادامه با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringContinuingProcessEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای ادامه با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نبود مسیر ادامه یا لغو نمونه»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نبود مسیر ادامه یا لغو نمونه»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ProcessesGroupsCount`تعداد گروه‌های متمایز دارای نسخه غیرآزمایشی در ProcessList؛ مستقل از فیلتر تاریخ و الزاماً تعداد تمام ردیف‌های ProcessGroup نیست.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.`FinishedCreationsTotalCount`تمام نمونه‌های بازه ایجاد اختصاصی کارت خاتمه‌یافته، شامل وضعیت‌های دیگر.`FinishedCreationsPercentage`تعداد خاتمه‌یافته ×۱۰۰ تقسیم بر مخرج گزارش؛ در پراکندگی، کل نمونه‌های بازه در دامنه گزارش است.`NewCreationsCount`تعداد نمونه جاری: Finish=0 و Discard≠1؛ عنوان New به معنی تمام نمونه‌های تازه ایجادشده نیست.`NewCreationsTotalCount`تمام نمونه‌های غیرآزمایشی بازه اختصاصی کارت جاری، شامل خاتمه و لغو.`NewCreationsPercentage`NewCreationsCount ×۱۰۰ / NewCreationsTotalCount؛ مخرج صفر، نتیجه صفر.`DiscardedCreationsCount`تعداد نمونه‌هایی که Discard=1؛ مستقل از مقدار Finish.`DiscardedCreationsTotalCount`تمام نمونه‌های بازه ایجاد اختصاصی کارت لغوشده.`DiscardedCreationsPercentage`DiscardedCreationsCount ×۱۰۰ / DiscardedCreationsTotalCount؛ مخرج صفر، نتیجه صفر.`InCartableCreationsCount`تعداد نمونه‌های یکتایی که دست‌کم یک ارجاع اصلی در Process_InterimInboxData دارند.`CopiesInCartableCreationsCount`تعداد نمونه‌های یکتایی که دست‌کم یک رونوشت در کارتابل دارند؛ ممکن است با نمونه‌های کارت اصلی هم‌پوشانی داشته باشد.`InCartableAndCopiesCreationsTotalCount`کل نمونه‌های بازه ایجاد کارتابل؛ تعداد نمونه‌های دارای ارجاع یا رونوشت نیست.`InCartableCreationsPercentage`درصد نمونه‌های دارای ارجاع اصلی از کل نمونه‌های بازه کارتابل.`CopiesInCartableCreationsPercentage`درصد نمونه‌های دارای رونوشت از کل نمونه‌های بازه کارتابل.`InCartableRefsCount`تعداد ردیف ارجاع اصلی کارتابل برای نمونه‌های بازه؛ SQL با SUM محاسبه می‌کند و در نبود ارجاع ممکن است NULL باشد.`CopiesInCartableRefsCount`تعداد ردیف رونوشت کارتابل برای نمونه‌های بازه؛ در مجموعه خالی ممکن است NULL باشد.`OccurredExceptionCount`تعداد رکورد خطا در بازه وقوع خطا؛ چند خطا در یک نمونه جداگانه شمرده می‌شوند.`OccurredExceptionCreationsCount`تعداد نمونه یکتای دارای حداقل یک خطا؛ بازه روی تاریخ ایجاد نمونه است و زمان خود خطا محدود نمی‌شود.`OccurredExceptionCreationsTotalCount`تعداد کل نمونه‌ها در بازه ایجاد اختصاصی شاخص نمونه دارای خطا.`OccurredExceptionCreationsPercentage`درصد نمونه یکتای دارای خطا از کل نمونه‌های همان بازه ایجاد.`AutoHandledOccurredExceptionCreationsCount`تعداد رکورد خطای «مدیریت‌شده توسط Boundary»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برای المان خطا Boundary متصل در همان نسخه فرآیند وجود داشته باشد؛ اتصال با AttachedTo و FK_ProcessID بررسی می‌شود.`AutoHandledOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AutoHandledOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`NotAutoHandledOccurredExceptionCreationsCount`تعداد رکورد خطای «مدیریت‌نشده توسط Boundary»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برای المان خطا چنین Boundaryای پیدا نشود.`NotAutoHandledOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`NotAutoHandledOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AccessViolationOccurredExceptionCreationsCount`تعداد رکورد خطای «خطای دسترسی»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionType = 'AccessViolationException'.`AccessViolationOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AccessViolationOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ExternalOccurredExceptionCreationsCount`تعداد رکورد خطای «سرویس خارجی»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. نوع المان ServiceTask (۲)، دارای ProcessServiceTaskConfig و Provider با DataType برابر ۶ (WSDL) یا ۷ (REST).`ExternalOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ExternalOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`StartedAsServiceOccurredExceptionCreationsCount`تعداد رکورد خطای «نمونه آغازشده با سرویس»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وجود نام متغیر @@StartFromService در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.`StartedAsServiceOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`StartedAsServiceOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`StartedAsESBOccurredExceptionCreationsCount`تعداد رکورد خطای «نمونه آغازشده با ESB»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وجود نام متغیر @@StartFromESB در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.`StartedAsESBOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`StartedAsESBOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`TimeoutOccurredExceptionCreationsCount`تعداد رکورد خطای «مهلت‌زمان / Timeout»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionType = 'TimeoutException' یا تطبیق دقیق پیام SQL timeout تعریف‌شده در کد.`TimeoutOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`TimeoutOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsCount`تعداد رکورد خطای «نبود مسیر پیش‌فرض Gateway»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionMessage دقیقاً 'Gateway has not default sequence flow'.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`OccurredExceptionFinishedCreationsCount`تعداد رکورد خطای «خطا در نمونه خاتمه‌یافته»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وضعیت فعلی Finish نمونه برابر ۱؛ الزام نمی‌کند خطا پس از خاتمه رخ داده باشد.`OccurredExceptionFinishedCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`OccurredExceptionFinishedCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`InvalidOutputServiceDataOccurredExceptionCreationsCount`تعداد رکورد خطای «خروجی نامعتبر سرویس»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionTitle برابر متن پارامتر InvalidOutputServiceData_Translate.`InvalidOutputServiceDataOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`InvalidOutputServiceDataOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ErrorInGetVariableValueOccurredExceptionCreationsCount`تعداد رکورد خطای «دریافت مقدار متغیر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionTitle برابر متن پارامتر ErrorInGetValVar_Translate.`ErrorInGetVariableValueOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ErrorInGetVariableValueOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AnExceptionOccurredDuringStartingProcessCreationsCount`تعداد رکورد خطای «شروع با تایمر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. عنوان برابر ترجمه شروع/ادامه و نوع المان ۱۸ (Start Timer).`AnExceptionOccurredDuringStartingProcessCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AnExceptionOccurredDuringStartingProcessCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AnExceptionOccurredDuringContinuingProcessCreationsCount`تعداد رکورد خطای «ادامه با تایمر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. عنوان برابر ترجمه شروع/ادامه و نوع المان ۲۳ (Intermediate Timer).`AnExceptionOccurredDuringContinuingProcessCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AnExceptionOccurredDuringContinuingProcessCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsCount`تعداد رکورد خطای «نبود مسیر ادامه / نمونه لغوشده»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برابری دقیق عنوان با ترجمه مربوط؛ نام دسته به‌تنهایی اثبات لغوشدن وضعیت نمونه نیست.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`OtherExceptionCreationsCount`تعداد رکورد خطای «سایر خطاها»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. خطای بدون Boundary که هیچ‌یک از شرط‌های دسته‌های ۴ تا ۱۵ را ندارد. بازه آن در تجمیع از NotAutoHandledOccurredException گرفته می‌شود.`OtherExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`OtherExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- وضعیت جاری/خاتمه/لغو وضعیت فعلی نمونه در زمان خواندن است، نه بازسازی وضعیت تاریخی آن در روز انتخاب‌شده. فقط این سه دسته با بازه یکسان، دسته‌های ناسازگار با یکدیگرند؛ کارتابل و خطا با آن‌ها هم‌پوشانی دارند.
- با وجود نام ...ExceptionCreationsCount، دسته‌های ۲ تا ۱۶ رکوردهای خطا را می‌شمارند. تنها OccurredExceptionCreationsCount نمونه یکتا می‌شمارد. یک نمونه با چند خطا می‌تواند چند بار در دسته‌های خطا شمرده شود.
- دسته‌های موضوعی خطا مستقل‌اند و ممکن است هم‌پوشانی داشته باشند؛ جمع اعداد آن‌ها الزاماً کل خطاها نیست. Other از «بدون Boundary» و خارج همه دسته‌های ۴ تا ۱۵ ساخته می‌شود، نه از تفریق ساده اعداد کارت‌ها.
- «مدیریت‌شده» در SQL به معنی وجود Boundary متصل در تعریف فرآیند است؛ این محاسبه لاگ موفقیت اجرای مسیر مدیریت خطا را بررسی نمی‌کند. خود جدول Boundary بر نوع رویداد خطا شرط صریح ندارد.
- اگر متن ترجمه لازم NULL یا با متن ذخیره‌شده متفاوت باشد، دسته‌های ۱۱ تا ۱۵ ممکن است صفر شوند و بخشی از خطاها در Other بمانند. تشخیص سرویس/ESB صرفاً با وجود نام متغیر است.
- InCartableRefsCount و CopiesInCartableRefsCount به‌علت SUM بدون ISNULL ممکن است در مجموعه خالی NULL باشند؛ سایر شمارنده‌های COUNT_BIG صفر هستند.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessList`، `dbo.ProcessServiceTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.Process_InterimInboxData`، `dbo.Provider`

CREATE OR ALTER PROCEDURE ProcessBAM_GetAggregatedGeneralInfo
    @InvalidOutputServiceData_Translate NVARCHAR(500),
    @ErrorInGetValVar_Translate NVARCHAR(500),
    @AnExceptionOccurredDuringStartingOrContinuingProcess_Translate NVARCHAR(500),
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate NVARCHAR(500),
    @FinishedStartDate DATETIME = NULL,
    @FinishedEndDate DATETIME = NULL,
    @NewStartDate DATETIME = NULL,
    @NewEndDate DATETIME = NULL,
    @DiscardStartDate DATETIME = NULL,
    @DiscardEndDate DATETIME = NULL,
    @InCartableStartDate DATETIME = NULL,
    @InCartableEndDate DATETIME = NULL,
    @OccurredExceptionCreationStartDate DATETIME = NULL,
    @OccurredExceptionCreationEndDate DATETIME = NULL,
    @OccurredExceptionStartDate DATETIME = NULL,
    @OccurredExceptionEndDate DATETIME = NULL,
    @AutoHandledOccurredExceptionStartDate DATETIME = NULL,
    @AutoHandledOccurredExceptionEndDate DATETIME = NULL,
    @NotAutoHandledOccurredExceptionStartDate DATETIME = NULL,
    @NotAutoHandledOccurredExceptionEndDate DATETIME = NULL,
    @AccessViolationOccurredExceptionStartDate DATETIME = NULL,
    @AccessViolationOccurredExceptionEndDate DATETIME = NULL,
    @ExternalOccurredExceptionStartDate DATETIME = NULL,
    @ExternalOccurredExceptionEndDate DATETIME = NULL,
    @StartedAsServiceOccurredExceptionStartDate DATETIME = NULL,
    @StartedAsServiceOccurredExceptionEndDate DATETIME = NULL,
    @StartedAsESBOccurredExceptionStartDate DATETIME = NULL,
    @StartedAsESBOccurredExceptionEndDate DATETIME = NULL,
    @TimeoutOccurredExceptionStartDate DATETIME = NULL,
    @TimeoutOccurredExceptionEndDate DATETIME = NULL,
    @GatewayHasNoDefaultSequenceFlowOccurredExceptionStartDate DATETIME = NULL,
    @GatewayHasNoDefaultSequenceFlowOccurredExceptionEndDate DATETIME = NULL,
    @OccurredExceptionFinishedStartDate DATETIME = NULL,
    @OccurredExceptionFinishedEndDate DATETIME = NULL,
    @InvalidOutputServiceDataOccurredExceptionStartDate DATETIME = NULL,
    @InvalidOutputServiceDataOccurredExceptionEndDate DATETIME = NULL,
    @ErrorInGetValVarOccurredExceptionStartDate DATETIME = NULL,
    @ErrorInGetValVarOccurredExceptionEndDate DATETIME = NULL,
    @AnExceptionOccurredDuringStartingProcessStartDate DATETIME = NULL,
    @AnExceptionOccurredDuringStartingProcessEndDate DATETIME = NULL,
    @AnExceptionOccurredDuringContinuingProcessStartDate DATETIME = NULL,
    @AnExceptionOccurredDuringContinuingProcessEndDate DATETIME = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionStartDate DATETIME = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionEndDate DATETIME = NULL

### 6. شاخص‌های جامع یک گروه فرآیند
`ProcessBAM_GetAggregatedGeneralInfoByProcessGroupId`

این رویه یک ردیف آماری جامع برای کارت‌های وضعیت نمونه و کارت‌های خطا تولید می‌کند. هر کارت زوج تاریخ اختصاصی دارد؛ تغییر یک زوج تاریخ، مخرج همان کارت را نیز تغییر می‌دهد. چهار پارامتر ترجمه پیش‌فرض ندارند و باید هنگام فراخوانی تأمین شوند؛ امکان ارسال صریح NULL به SQL همچنان وجود دارد.

ابتدا همه نمونه‌های غیرآزمایشی در #CreationsDates، خطاهای مرتبط با همین نمونه‌ها در #Exceptions و نام متغیرهای @@StartFromService و @@StartFromESB در #VariablesLogs گردآوری می‌شوند. وضعیت‌های NULL نمونه به صفر تبدیل می‌شوند. جدول موقت Boundary از ProcessBoundaryEventConfig و ProcessElements ساخته می‌شود.

کارت‌های جاری، خاتمه، لغو، کارتابل/رونوشت و نمونه دارای خطا همگی با تاریخ ایجاد نمونه فیلتر می‌شوند. کارت‌های تعداد خطا و دسته‌های خطا با تاریخ وقوع خطا فیلتر می‌شوند. مثلاً نمونه ایجادشده در بازه که بعداً خطا داده است، در شمارنده نمونه دارای خطا می‌ماند.

نسبت هر کارت Count×100.0/TotalCount است و در مخرج صفر، مقدار صفر بازمی‌گردد. برای کارت‌های وضعیت، TotalCount کل نمونه‌های همان بازه است. برای دسته خطا، TotalCount تمام خطاهای همان بازه اختصاصی است.

کارتابل دو سطح دارد: شمارش نمونه یکتا با حداقل یک ارجاع اصلی یا رونوشت، و شمارش خود ردیف‌های ارجاع. یک نمونه می‌تواند هم در اصلی و هم در رونوشت قرار گیرد. کارتابل از Process_InterimInboxData فعلی خوانده می‌شود و تاریخ آن تاریخ ایجاد نمونه است.

در مرحله گردآوری نمونه‌ها، گروه اجباری روی ProcessList.FK_ProcessGroupID اعمال می‌شود. به جای ProcessesGroupsCount، سه خروجی ActiveProcessesCount، InActiveProcessesCount و SuspendedProcessesCount درباره نسخه‌های همین گروه بازمی‌گردد. این سه خروجی مستقل از تاریخ هستند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@InvalidOutputServiceData_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان «خروجی نامعتبر سرویس» برای تشخیص دسته ۱۱؛ برابری عنوان خطا ملاک است.`@ErrorInGetValVar_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان «خطا در دریافت مقدار متغیر» برای دسته ۱۲.`@AnExceptionOccurredDuringStartingOrContinuingProcess_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان خطای شروع/ادامه؛ همراه نوع المان ۱۸ یا ۲۳ برای دسته ۱۳ یا ۱۴.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate``NVARCHAR(500)`اجباری؛ بدون پیش‌فرضمتن ترجمه‌شده دقیق عنوان نبود مسیر ادامه/لغو نمونه برای دسته ۱۵.`@FinishedStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه خاتمه‌یافته»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@FinishedEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه خاتمه‌یافته»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NewStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه جاری»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NewEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه جاری»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@DiscardStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه لغوشده»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@DiscardEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه لغوشده»؛ مبنا: تاریخ ایجاد نمونه. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InCartableStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «کارتابل و رونوشت»؛ مبنا: تاریخ ایجاد نمونه، نه تاریخ ورود ارجاع. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InCartableEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «کارتابل و رونوشت»؛ مبنا: تاریخ ایجاد نمونه، نه تاریخ ورود ارجاع. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionCreationStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نمونه دارای خطا»؛ مبنا: تاریخ ایجاد نمونه؛ خود خطا می‌تواند خارج بازه رخ داده باشد. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionCreationEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نمونه دارای خطا»؛ مبنا: تاریخ ایجاد نمونه؛ خود خطا می‌تواند خارج بازه رخ داده باشد. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «کل خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «کل خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AutoHandledOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دارای Boundary»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AutoHandledOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دارای Boundary»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NotAutoHandledOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای بدون Boundary و سایر خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@NotAutoHandledOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای بدون Boundary و سایر خطاها»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AccessViolationOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دسترسی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AccessViolationOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دسترسی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ExternalOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای سرویس خارجی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ExternalOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای سرویس خارجی»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsServiceOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نمونه آغازشده با سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsServiceOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نمونه آغازشده با سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsESBOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نمونه آغازشده با ESB»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@StartedAsESBOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نمونه آغازشده با ESB»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@TimeoutOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای مهلت‌زمان»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@TimeoutOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای مهلت‌زمان»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@GatewayHasNoDefaultSequenceFlowOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای نبود مسیر پیش‌فرض Gateway»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@GatewayHasNoDefaultSequenceFlowOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای نبود مسیر پیش‌فرض Gateway»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionFinishedStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطا در نمونه خاتمه‌یافته»؛ مبنا: زمان وقوع خطا؛ وضعیت خاتمه در زمان خواندن. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@OccurredExceptionFinishedEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطا در نمونه خاتمه‌یافته»؛ مبنا: زمان وقوع خطا؛ وضعیت خاتمه در زمان خواندن. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InvalidOutputServiceDataOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خروجی نامعتبر سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@InvalidOutputServiceDataOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خروجی نامعتبر سرویس»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ErrorInGetValVarOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای دریافت مقدار متغیر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ErrorInGetValVarOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای دریافت مقدار متغیر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringStartingProcessStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای شروع با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringStartingProcessEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای شروع با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringContinuingProcessStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «خطای ادامه با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@AnExceptionOccurredDuringContinuingProcessEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «خطای ادامه با تایمر»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionStartDate``DATETIME``NULL`مرز شروع مستقل شاخص «نبود مسیر ادامه یا لغو نمونه»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionEndDate``DATETIME``NULL`مرز پایان مستقل شاخص «نبود مسیر ادامه یا لغو نمونه»؛ مبنا: زمان وقوع خطا. NULL یعنی این حد اعمال نشود. مرزها فراگیرند.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ActiveProcessesCount`تعداد نسخه‌های گروه با IsActive=1 و Suspended=0؛ مستقل از تاریخ نمونه‌ها.`InActiveProcessesCount`تعداد نسخه‌های گروه با IsActive<>1 و Suspended=0. مقدار NULL در IsActive شرط <>1 را برآورده نمی‌کند.`SuspendedProcessesCount`تعداد نسخه‌های گروه با Suspended=1، مستقل از فعال/غیرفعال بودن.`FinishedCreationsCount`تعداد نمونه خاتمه‌یافته بدون لغو؛ COUNT_BIG.`FinishedCreationsTotalCount`تمام نمونه‌های بازه ایجاد اختصاصی کارت خاتمه‌یافته، شامل وضعیت‌های دیگر.`FinishedCreationsPercentage`تعداد خاتمه‌یافته ×۱۰۰ تقسیم بر مخرج گزارش؛ در پراکندگی، کل نمونه‌های بازه در دامنه گزارش است.`NewCreationsCount`تعداد نمونه جاری: Finish=0 و Discard≠1؛ عنوان New به معنی تمام نمونه‌های تازه ایجادشده نیست.`NewCreationsTotalCount`تمام نمونه‌های غیرآزمایشی بازه اختصاصی کارت جاری، شامل خاتمه و لغو.`NewCreationsPercentage`NewCreationsCount ×۱۰۰ / NewCreationsTotalCount؛ مخرج صفر، نتیجه صفر.`DiscardedCreationsCount`تعداد نمونه‌هایی که Discard=1؛ مستقل از مقدار Finish.`DiscardedCreationsTotalCount`تمام نمونه‌های بازه ایجاد اختصاصی کارت لغوشده.`DiscardedCreationsPercentage`DiscardedCreationsCount ×۱۰۰ / DiscardedCreationsTotalCount؛ مخرج صفر، نتیجه صفر.`InCartableCreationsCount`تعداد نمونه‌های یکتایی که دست‌کم یک ارجاع اصلی در Process_InterimInboxData دارند.`CopiesInCartableCreationsCount`تعداد نمونه‌های یکتایی که دست‌کم یک رونوشت در کارتابل دارند؛ ممکن است با نمونه‌های کارت اصلی هم‌پوشانی داشته باشد.`InCartableAndCopiesCreationsTotalCount`کل نمونه‌های بازه ایجاد کارتابل؛ تعداد نمونه‌های دارای ارجاع یا رونوشت نیست.`InCartableCreationsPercentage`درصد نمونه‌های دارای ارجاع اصلی از کل نمونه‌های بازه کارتابل.`CopiesInCartableCreationsPercentage`درصد نمونه‌های دارای رونوشت از کل نمونه‌های بازه کارتابل.`InCartableRefsCount`تعداد ردیف ارجاع اصلی کارتابل برای نمونه‌های بازه؛ SQL با SUM محاسبه می‌کند و در نبود ارجاع ممکن است NULL باشد.`CopiesInCartableRefsCount`تعداد ردیف رونوشت کارتابل برای نمونه‌های بازه؛ در مجموعه خالی ممکن است NULL باشد.`OccurredExceptionCount`تعداد رکورد خطا در بازه وقوع خطا؛ چند خطا در یک نمونه جداگانه شمرده می‌شوند.`OccurredExceptionCreationsCount`تعداد نمونه یکتای دارای حداقل یک خطا؛ بازه روی تاریخ ایجاد نمونه است و زمان خود خطا محدود نمی‌شود.`OccurredExceptionCreationsTotalCount`تعداد کل نمونه‌ها در بازه ایجاد اختصاصی شاخص نمونه دارای خطا.`OccurredExceptionCreationsPercentage`درصد نمونه یکتای دارای خطا از کل نمونه‌های همان بازه ایجاد.`AutoHandledOccurredExceptionCreationsCount`تعداد رکورد خطای «مدیریت‌شده توسط Boundary»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برای المان خطا Boundary متصل در همان نسخه فرآیند وجود داشته باشد؛ اتصال با AttachedTo و FK_ProcessID بررسی می‌شود.`AutoHandledOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AutoHandledOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`NotAutoHandledOccurredExceptionCreationsCount`تعداد رکورد خطای «مدیریت‌نشده توسط Boundary»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برای المان خطا چنین Boundaryای پیدا نشود.`NotAutoHandledOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`NotAutoHandledOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AccessViolationOccurredExceptionCreationsCount`تعداد رکورد خطای «خطای دسترسی»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionType = 'AccessViolationException'.`AccessViolationOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AccessViolationOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ExternalOccurredExceptionCreationsCount`تعداد رکورد خطای «سرویس خارجی»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. نوع المان ServiceTask (۲)، دارای ProcessServiceTaskConfig و Provider با DataType برابر ۶ (WSDL) یا ۷ (REST).`ExternalOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ExternalOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`StartedAsServiceOccurredExceptionCreationsCount`تعداد رکورد خطای «نمونه آغازشده با سرویس»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وجود نام متغیر @@StartFromService در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.`StartedAsServiceOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`StartedAsServiceOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`StartedAsESBOccurredExceptionCreationsCount`تعداد رکورد خطای «نمونه آغازشده با ESB»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وجود نام متغیر @@StartFromESB در لاگ غیرآزمایشی نمونه؛ مقدار متغیر بررسی نمی‌شود.`StartedAsESBOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`StartedAsESBOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`TimeoutOccurredExceptionCreationsCount`تعداد رکورد خطای «مهلت‌زمان / Timeout»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionType = 'TimeoutException' یا تطبیق دقیق پیام SQL timeout تعریف‌شده در کد.`TimeoutOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`TimeoutOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsCount`تعداد رکورد خطای «نبود مسیر پیش‌فرض Gateway»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionMessage دقیقاً 'Gateway has not default sequence flow'.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`GatewayHasNoDefaultSequenceFlowOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`OccurredExceptionFinishedCreationsCount`تعداد رکورد خطای «خطا در نمونه خاتمه‌یافته»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. وضعیت فعلی Finish نمونه برابر ۱؛ الزام نمی‌کند خطا پس از خاتمه رخ داده باشد.`OccurredExceptionFinishedCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`OccurredExceptionFinishedCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`InvalidOutputServiceDataOccurredExceptionCreationsCount`تعداد رکورد خطای «خروجی نامعتبر سرویس»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionTitle برابر متن پارامتر InvalidOutputServiceData_Translate.`InvalidOutputServiceDataOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`InvalidOutputServiceDataOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ErrorInGetVariableValueOccurredExceptionCreationsCount`تعداد رکورد خطای «دریافت مقدار متغیر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. ExceptionTitle برابر متن پارامتر ErrorInGetValVar_Translate.`ErrorInGetVariableValueOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ErrorInGetVariableValueOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AnExceptionOccurredDuringStartingProcessCreationsCount`تعداد رکورد خطای «شروع با تایمر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. عنوان برابر ترجمه شروع/ادامه و نوع المان ۱۸ (Start Timer).`AnExceptionOccurredDuringStartingProcessCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AnExceptionOccurredDuringStartingProcessCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`AnExceptionOccurredDuringContinuingProcessCreationsCount`تعداد رکورد خطای «ادامه با تایمر»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. عنوان برابر ترجمه شروع/ادامه و نوع المان ۲۳ (Intermediate Timer).`AnExceptionOccurredDuringContinuingProcessCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`AnExceptionOccurredDuringContinuingProcessCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsCount`تعداد رکورد خطای «نبود مسیر ادامه / نمونه لغوشده»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. برابری دقیق عنوان با ترجمه مربوط؛ نام دسته به‌تنهایی اثبات لغوشدن وضعیت نمونه نیست.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.`OtherExceptionCreationsCount`تعداد رکورد خطای «سایر خطاها»؛ با وجود واژه Creations در نام، واحد شمارش رکورد خطاست. خطای بدون Boundary که هیچ‌یک از شرط‌های دسته‌های ۴ تا ۱۵ را ندارد. بازه آن در تجمیع از NotAutoHandledOccurredException گرفته می‌شود.`OtherExceptionCreationsTotalCount`تمام رکوردهای خطا در بازه مستقل این دسته؛ مخرج درصد، فقط تعداد اعضای دسته نیست.`OtherExceptionCreationsPercentage`تعداد رکوردهای این دسته ×۱۰۰ تقسیم بر تمام خطاهای بازه همان دسته؛ مخرج صفر، نتیجه صفر.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- وضعیت جاری/خاتمه/لغو وضعیت فعلی نمونه در زمان خواندن است، نه بازسازی وضعیت تاریخی آن در روز انتخاب‌شده. فقط این سه دسته با بازه یکسان، دسته‌های ناسازگار با یکدیگرند؛ کارتابل و خطا با آن‌ها هم‌پوشانی دارند.
- با وجود نام ...ExceptionCreationsCount، دسته‌های ۲ تا ۱۶ رکوردهای خطا را می‌شمارند. تنها OccurredExceptionCreationsCount نمونه یکتا می‌شمارد. یک نمونه با چند خطا می‌تواند چند بار در دسته‌های خطا شمرده شود.
- دسته‌های موضوعی خطا مستقل‌اند و ممکن است هم‌پوشانی داشته باشند؛ جمع اعداد آن‌ها الزاماً کل خطاها نیست. Other از «بدون Boundary» و خارج همه دسته‌های ۴ تا ۱۵ ساخته می‌شود، نه از تفریق ساده اعداد کارت‌ها.
- «مدیریت‌شده» در SQL به معنی وجود Boundary متصل در تعریف فرآیند است؛ این محاسبه لاگ موفقیت اجرای مسیر مدیریت خطا را بررسی نمی‌کند. خود جدول Boundary بر نوع رویداد خطا شرط صریح ندارد.
- اگر متن ترجمه لازم NULL یا با متن ذخیره‌شده متفاوت باشد، دسته‌های ۱۱ تا ۱۵ ممکن است صفر شوند و بخشی از خطاها در Other بمانند. تشخیص سرویس/ESB صرفاً با وجود نام متغیر است.
- InCartableRefsCount و CopiesInCartableRefsCount به‌علت SUM بدون ISNULL ممکن است در مجموعه خالی NULL باشند؛ سایر شمارنده‌های COUNT_BIG صفر هستند.
- در شمارش نسخه غیرفعال، شرط IsActive<>1 با مقدار NULL برقرار نیست؛ نسخه با پرچم نامشخص الزاماً در سه شمارنده جای نمی‌گیرد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessList`، `dbo.ProcessServiceTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.Process_InterimInboxData`، `dbo.Provider`

CREATE OR ALTER PROCEDURE ProcessBAM_GetAggregatedGeneralInfoByProcessGroupId
    @ProcessGroupId INT,
    @InvalidOutputServiceData_Translate NVARCHAR(500),
    @ErrorInGetValVar_Translate NVARCHAR(500),
    @AnExceptionOccurredDuringStartingOrContinuingProcess_Translate NVARCHAR(500),
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate NVARCHAR(500),
    @FinishedStartDate DATETIME = NULL,
    @FinishedEndDate DATETIME = NULL,
    @NewStartDate DATETIME = NULL,
    @NewEndDate DATETIME = NULL,
    @DiscardStartDate DATETIME = NULL,
    @DiscardEndDate DATETIME = NULL,
    @InCartableStartDate DATETIME = NULL,
    @InCartableEndDate DATETIME = NULL,
    @OccurredExceptionCreationStartDate DATETIME = NULL,
    @OccurredExceptionCreationEndDate DATETIME = NULL,
    @OccurredExceptionStartDate DATETIME = NULL,
    @OccurredExceptionEndDate DATETIME = NULL,
    @AutoHandledOccurredExceptionStartDate DATETIME = NULL,
    @AutoHandledOccurredExceptionEndDate DATETIME = NULL,
    @NotAutoHandledOccurredExceptionStartDate DATETIME = NULL,
    @NotAutoHandledOccurredExceptionEndDate DATETIME = NULL,
    @AccessViolationOccurredExceptionStartDate DATETIME = NULL,
    @AccessViolationOccurredExceptionEndDate DATETIME = NULL,
    @ExternalOccurredExceptionStartDate DATETIME = NULL,
    @ExternalOccurredExceptionEndDate DATETIME = NULL,
    @StartedAsServiceOccurredExceptionStartDate DATETIME = NULL,
    @StartedAsServiceOccurredExceptionEndDate DATETIME = NULL,
    @StartedAsESBOccurredExceptionStartDate DATETIME = NULL,
    @StartedAsESBOccurredExceptionEndDate DATETIME = NULL,
    @TimeoutOccurredExceptionStartDate DATETIME = NULL,
    @TimeoutOccurredExceptionEndDate DATETIME = NULL,
    @GatewayHasNoDefaultSequenceFlowOccurredExceptionStartDate DATETIME = NULL,
    @GatewayHasNoDefaultSequenceFlowOccurredExceptionEndDate DATETIME = NULL,
    @OccurredExceptionFinishedStartDate DATETIME = NULL,
    @OccurredExceptionFinishedEndDate DATETIME = NULL,
    @InvalidOutputServiceDataOccurredExceptionStartDate DATETIME = NULL,
    @InvalidOutputServiceDataOccurredExceptionEndDate DATETIME = NULL,
    @ErrorInGetValVarOccurredExceptionStartDate DATETIME = NULL,
    @ErrorInGetValVarOccurredExceptionEndDate DATETIME = NULL,
    @AnExceptionOccurredDuringStartingProcessStartDate DATETIME = NULL,
    @AnExceptionOccurredDuringStartingProcessEndDate DATETIME = NULL,
    @AnExceptionOccurredDuringContinuingProcessStartDate DATETIME = NULL,
    @AnExceptionOccurredDuringContinuingProcessEndDate DATETIME = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionStartDate DATETIME = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionEndDate DATETIME = NULL

### 7. خلاصه زمانی عملکرد کل سامانه
`ProcessBAM_GetSummaryPerDay`

چهار خانواده داده به یک جدول موقت #Facts افزوده می‌شود: وضعیت نمونه، ردیف کارتابل، ارسال فرم و خطا. هر ردیف ابتدا با EventDate خودش محدود می‌شود و سپس در دسته زمانی قرار می‌گیرد. داده نمونه آزمایشی حذف می‌شود.

BasedOnCreationDate و BasedOnCreationCount مستقل‌اند: اولی محور زمان و دومی واحد شمارش کارتابل، ارسال و خطا را تعیین می‌کند. IncludeCopies فقط نوع ردیف‌های کارتابل و ارسال را انتخاب می‌کند؛ وضعیت نمونه و خطاها را به رونوشت/اصلی تقسیم نمی‌کند.

ارسال فرم در این رویه یعنی ProcessReferences با FKFormsConfigID غیرNULL و ExitCartableDateTime غیرNULL و IsCopy انتخابی. نام UserTask در خروجی مدت دیده می‌شود، اما شرط مستقیم ItemTypeID=1 در این بخش وجود ندارد.

اگر BasedOnCreationDate=1 باشد، برای هر نمونه تعداد ارسال‌ها و میانگین ثانیه‌های ارسال محاسبه می‌شود؛ سپس بیشترین این میانگین‌ها در هر دسته خروجی می‌آید. اگر صفر باشد، هر ارجاع ارسال‌شده یک Fact دارد و بیشینه، کندترین ارجاع منفرد است.

خروجی یک مجموعه نتیجه است که شکل ستون‌های زمان آن متغیر است: اگر هر دو تاریخ NULL باشند یا هر دو مقدار داشته باشند و DATEDIFF(DAY)>1 شود، ستون‌های AtYear/AtMonth/AtDay می‌آیند؛ در سایر حالت‌ها AtHour/AtMinute/AtSecond می‌آیند. روز/ساعت بدون Fact، ردیف صفر مصنوعی در SQL ندارد.

در هر دسته، نمونه دارای خطای Boundary و خطای بدون Boundary می‌تواند در هر دو شمارنده یکتا دیده شود. شناسه نمونه متناظر با بیشترین مدت در تساوی با کوچک‌ترین شناسه تعیین می‌شود. خروجی SQL ORDER BY نهایی ندارد و نظم نمودار باید در لایه بالاتر اعمال شود.

نسخه عمومی علاوه بر شناسه نمونه بیشینه، MaximumUserTaskSubmitDurationGroupName را نیز برمی‌گرداند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@IncludeCopies``BIT``0`انتخاب نوع ارجاع: صفر فقط ارجاع اصلی، یک فقط رونوشت. شرط SQL برابری IsCopy است؛ مقدار یک به معنی جمع اصلی و رونوشت نیست.`@BasedOnCreationDate``BIT``1`یک: مبنای زمان همه سری‌ها تاریخ ایجاد نمونه است. صفر: تاریخ رخداد هر سری به کار می‌رود؛ جزئیات در جدول مبنای زمان آمده است.`@BasedOnCreationCount``BIT``1`یک: کارتابل، ارسال فرم و خطاها برحسب نمونه یکتا در هر بازه شمارش می‌شوند. صفر: تعداد ارجاع/ارسال/خطا شمرده می‌شود. وضعیت‌های جاری/خاتمه/لغو همواره نمونه‌اند.
همه ستون‌های خروجی

ستون‌های AtYear/AtMonth/AtDay و AtHour/AtMinute/AtSecond دو شکل جایگزین‌اند؛ هم‌زمان در یک خروجی نمی‌آیند. سایر ستون‌ها در هر دو شاخه مشترک‌اند.

ستون خروجیمعنا / واحد / نحوه محاسبه`AtYear`سال میلادی EventDate، خروجی DATEPART SQL؛ تبدیل تقویم در لایه نمایش انجام می‌شود.`AtMonth`ماه EventDate در شاخه روزانه.`AtDay`روز EventDate در شاخه روزانه.`NewCount`تعداد نمونه با وضعیت فعلی جاری در این دسته زمانی؛ نمونه خاتمه/لغوشده هم‌زمان جاری شمرده نمی‌شود.`FinishedCount`تعداد نمونه با وضعیت فعلی خاتمه‌یافته و لغونشده در دسته زمانی.`DiscardedCount`تعداد نمونه با وضعیت فعلی لغوشده در دسته زمانی.`InCartableCount`با BasedOnCreationCount=1 نمونه یکتا در دسته زمانی، و با صفر تعداد ردیف کارتابل از نوع ارجاع انتخابی.`SubmittedTasksCount`با BasedOnCreationCount=1 تعداد نمونه یکتای دارای حداقل یک ارسال فرم؛ با صفر جمع تعداد ارسال‌های واجد شرایط.`MaximumTaskSubmissionDuration`ثانیه؛ با BasedOnCreationDate=1 بزرگ‌ترین میانگین زمان ارسال در سطح یک نمونه؛ با صفر بیشترین زمان یک ارجاع ارسال‌شده.`MaximumUserTaskSubmitDurationCreationId`شناسه نمونه متناظر با مقدار بیشینه. در تساوی، کوچک‌ترین CreationId انتخاب می‌شود.`MaximumUserTaskSubmitDurationGroupName`نام گروه نمونه متناظر با بیشینه؛ فقط خروجی گزارش عمومی.`ExceptionNotAutoHandledByErrorBoundaryCount`بدون Boundary؛ برحسب نمونه یکتا یا تعداد خطا طبق BasedOnCreationCount.`ExceptionAutoHandledByErrorBoundaryCount`دارای Boundary متصل؛ برحسب نمونه یکتا یا تعداد خطا طبق BasedOnCreationCount.`AtHour`ساعت EventDate در شاخه ساعتی/ثانیه‌ای.`AtMinute`دقیقه EventDate.`AtSecond`ثانیه EventDate؛ میلی‌ثانیه در دسته‌بندی کنار گذاشته می‌شود.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- شرط روزانه دقیقاً >1 است؛ بازه دو تاریخ مجاور، حتی اگر دو روز تقویمی را بپوشاند، وارد شاخه ساعت/دقیقه/ثانیه می‌شود. با یک مرز NULL نیز همین شاخه انتخاب می‌شود. در این شاخه تاریخ روز در GROUP BY نیست و ساعت مشابه روزهای مختلف می‌تواند ادغام شود.
- BasedOnCreationDate=1، زمان ارسال و وقوع خطا را داخل بازه انتخابی محدود نمی‌کند؛ فقط نمونه‌ای را می‌گیرد که CreateDate آن در بازه باشد. بنابراین این حالت تحلیل نسل نمونه‌هاست.
- MaximumTaskSubmissionDuration به معنی حداکثر زمان کل اجرای فرآیند نیست. DATEDIFF و AVG در این رویه INT هستند؛ مقادیر اعشاری میانگین حذف می‌شوند و بازه‌های بسیار طولانی با محدودیت INT مواجه‌اند.
- کارتابل از جدول فعلی Process_InterimInboxData خوانده می‌شود؛ این سری تاریخچه کامل ورود و خروج کارتابل را بازسازی نمی‌کند.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.ProcessReferences`، `dbo.Process_InterimInboxData`

CREATE OR ALTER PROCEDURE ProcessBAM_GetSummaryPerDay
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @IncludeCopies BIT = 0,
    @BasedOnCreationDate BIT = 1,
    @BasedOnCreationCount BIT = 1

### 8. خلاصه زمانی عملکرد یک گروه
`ProcessBAM_GetSummaryPerDayByProcessGroupId`

چهار خانواده داده به یک جدول موقت #Facts افزوده می‌شود: وضعیت نمونه، ردیف کارتابل، ارسال فرم و خطا. هر ردیف ابتدا با EventDate خودش محدود می‌شود و سپس در دسته زمانی قرار می‌گیرد. داده نمونه آزمایشی حذف می‌شود.

BasedOnCreationDate و BasedOnCreationCount مستقل‌اند: اولی محور زمان و دومی واحد شمارش کارتابل، ارسال و خطا را تعیین می‌کند. IncludeCopies فقط نوع ردیف‌های کارتابل و ارسال را انتخاب می‌کند؛ وضعیت نمونه و خطاها را به رونوشت/اصلی تقسیم نمی‌کند.

ارسال فرم در این رویه یعنی ProcessReferences با FKFormsConfigID غیرNULL و ExitCartableDateTime غیرNULL و IsCopy انتخابی. نام UserTask در خروجی مدت دیده می‌شود، اما شرط مستقیم ItemTypeID=1 در این بخش وجود ندارد.

اگر BasedOnCreationDate=1 باشد، برای هر نمونه تعداد ارسال‌ها و میانگین ثانیه‌های ارسال محاسبه می‌شود؛ سپس بیشترین این میانگین‌ها در هر دسته خروجی می‌آید. اگر صفر باشد، هر ارجاع ارسال‌شده یک Fact دارد و بیشینه، کندترین ارجاع منفرد است.

خروجی یک مجموعه نتیجه است که شکل ستون‌های زمان آن متغیر است: اگر هر دو تاریخ NULL باشند یا هر دو مقدار داشته باشند و DATEDIFF(DAY)>1 شود، ستون‌های AtYear/AtMonth/AtDay می‌آیند؛ در سایر حالت‌ها AtHour/AtMinute/AtSecond می‌آیند. روز/ساعت بدون Fact، ردیف صفر مصنوعی در SQL ندارد.

در هر دسته، نمونه دارای خطای Boundary و خطای بدون Boundary می‌تواند در هر دو شمارنده یکتا دیده شود. شناسه نمونه متناظر با بیشترین مدت در تساوی با کوچک‌ترین شناسه تعیین می‌شود. خروجی SQL ORDER BY نهایی ندارد و نظم نمودار باید در لایه بالاتر اعمال شود.

تمام خانواده Facts با EXISTS روی ProcessList و ProcessGroupId محدود می‌شوند. ستون نام گروه نمونه بیشینه حذف شده، چون دامنه گزارش از قبل یک گروه است؛ سایر معانی برقرارند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@IncludeCopies``BIT``0`انتخاب نوع ارجاع: صفر فقط ارجاع اصلی، یک فقط رونوشت. شرط SQL برابری IsCopy است؛ مقدار یک به معنی جمع اصلی و رونوشت نیست.`@BasedOnCreationDate``BIT``1`یک: مبنای زمان همه سری‌ها تاریخ ایجاد نمونه است. صفر: تاریخ رخداد هر سری به کار می‌رود؛ جزئیات در جدول مبنای زمان آمده است.`@BasedOnCreationCount``BIT``1`یک: کارتابل، ارسال فرم و خطاها برحسب نمونه یکتا در هر بازه شمارش می‌شوند. صفر: تعداد ارجاع/ارسال/خطا شمرده می‌شود. وضعیت‌های جاری/خاتمه/لغو همواره نمونه‌اند.
همه ستون‌های خروجی

ستون‌های AtYear/AtMonth/AtDay و AtHour/AtMinute/AtSecond دو شکل جایگزین‌اند؛ هم‌زمان در یک خروجی نمی‌آیند. سایر ستون‌ها در هر دو شاخه مشترک‌اند.

ستون خروجیمعنا / واحد / نحوه محاسبه`AtYear`سال میلادی EventDate، خروجی DATEPART SQL؛ تبدیل تقویم در لایه نمایش انجام می‌شود.`AtMonth`ماه EventDate در شاخه روزانه.`AtDay`روز EventDate در شاخه روزانه.`NewCount`تعداد نمونه با وضعیت فعلی جاری در این دسته زمانی؛ نمونه خاتمه/لغوشده هم‌زمان جاری شمرده نمی‌شود.`FinishedCount`تعداد نمونه با وضعیت فعلی خاتمه‌یافته و لغونشده در دسته زمانی.`DiscardedCount`تعداد نمونه با وضعیت فعلی لغوشده در دسته زمانی.`InCartableCount`با BasedOnCreationCount=1 نمونه یکتا در دسته زمانی، و با صفر تعداد ردیف کارتابل از نوع ارجاع انتخابی.`SubmittedTasksCount`با BasedOnCreationCount=1 تعداد نمونه یکتای دارای حداقل یک ارسال فرم؛ با صفر جمع تعداد ارسال‌های واجد شرایط.`MaximumTaskSubmissionDuration`ثانیه؛ با BasedOnCreationDate=1 بزرگ‌ترین میانگین زمان ارسال در سطح یک نمونه؛ با صفر بیشترین زمان یک ارجاع ارسال‌شده.`MaximumUserTaskSubmitDurationCreationId`شناسه نمونه متناظر با مقدار بیشینه. در تساوی، کوچک‌ترین CreationId انتخاب می‌شود.`ExceptionNotAutoHandledByErrorBoundaryCount`بدون Boundary؛ برحسب نمونه یکتا یا تعداد خطا طبق BasedOnCreationCount.`ExceptionAutoHandledByErrorBoundaryCount`دارای Boundary متصل؛ برحسب نمونه یکتا یا تعداد خطا طبق BasedOnCreationCount.`AtHour`ساعت EventDate در شاخه ساعتی/ثانیه‌ای.`AtMinute`دقیقه EventDate.`AtSecond`ثانیه EventDate؛ میلی‌ثانیه در دسته‌بندی کنار گذاشته می‌شود.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- شرط روزانه دقیقاً >1 است؛ بازه دو تاریخ مجاور، حتی اگر دو روز تقویمی را بپوشاند، وارد شاخه ساعت/دقیقه/ثانیه می‌شود. با یک مرز NULL نیز همین شاخه انتخاب می‌شود. در این شاخه تاریخ روز در GROUP BY نیست و ساعت مشابه روزهای مختلف می‌تواند ادغام شود.
- BasedOnCreationDate=1، زمان ارسال و وقوع خطا را داخل بازه انتخابی محدود نمی‌کند؛ فقط نمونه‌ای را می‌گیرد که CreateDate آن در بازه باشد. بنابراین این حالت تحلیل نسل نمونه‌هاست.
- MaximumTaskSubmissionDuration به معنی حداکثر زمان کل اجرای فرآیند نیست. DATEDIFF و AVG در این رویه INT هستند؛ مقادیر اعشاری میانگین حذف می‌شوند و بازه‌های بسیار طولانی با محدودیت INT مواجه‌اند.
- کارتابل از جدول فعلی Process_InterimInboxData خوانده می‌شود؛ این سری تاریخچه کامل ورود و خروج کارتابل را بازسازی نمی‌کند.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessList`، `dbo.ProcessReferences`، `dbo.Process_InterimInboxData`

CREATE OR ALTER PROCEDURE ProcessBAM_GetSummaryPerDayByProcessGroupId
    @ProcessGroupId INT,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @IncludeCopies BIT = 0,
    @BasedOnCreationDate BIT = 1,
    @BasedOnCreationCount BIT = 1

### 9. فهرست و ریزگزارش خطاها در کل سامانه
`ProcessBAM_GetExceptions`

یک ردیف به‌ازای هر خطای واجد شرایط از ProcessErrorLogs که نمونه غیرآزمایشی معتبر دارد؛ جداول ProcessList، ProcessGroup، المان/نوع، کاربر/نقش و والدها به صورت LEFT JOIN غنی‌سازی می‌شوند. ستون Element از FK_ElementID خود خطا به دست می‌آید.

همه ورودی‌های جست‌وجوی ارائه‌شده با AND ترکیب می‌شوند؛ عنوان/پیام/نام نقش/عنوان نمونه و والد جست‌وجوی شامل متن دارند و شناسه‌ها، نوع فنی خطا و شناسه BPMN تطبیق دقیق دارند. بازه اصلی این فهرست روی زمان وقوع خطاست.

ExceptionCategory از جدول دسته‌بندی این پیوست پیروی می‌کند. جدول Boundary فقط برای ۲، ۳، ۱۶ و جدول نام متغیرهای آغاز سرویس/ESB فقط برای ۶، ۷، ۱۶ پر می‌شود. مرزهای تاریخ فراگیرند.

ExceptionOccurredDuration اختلاف ثانیه از CreateDate خطا تا GETDATE زمان اجرای SQL است. مقدار ذخیره‌شده در کش، گذر زمان بعد از دریافت را منعکس نمی‌کند. این مقدار مدت اجرای گام یا عمر نمونه نیست.

**تفاوت قرارداد عمومی و گروهی: **DistinctCreations=1 فقط جدیدترین خطای هر نمونه را پس از اعمال فیلترها نگه می‌دارد. StrictTranslateCheck=1 برای دسته‌های ۱۱، ۱۲، ۱۳/۱۴، ۱۵ و ۱۶ نبود ترجمه لازم را با RAISERROR گزارش می‌کند. این دو قابلیت در امضای نسخه گروهی موجود نیستند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@InvalidOutputServiceData_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان «خروجی نامعتبر سرویس» برای تشخیص دسته ۱۱؛ برابری عنوان خطا ملاک است.`@ErrorInGetValVar_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان «خطا در دریافت مقدار متغیر» برای دسته ۱۲.`@AnExceptionOccurredDuringStartingOrContinuingProcess_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان خطای شروع/ادامه؛ همراه نوع المان ۱۸ یا ۲۳ برای دسته ۱۳ یا ۱۴.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان نبود مسیر ادامه/لغو نمونه برای دسته ۱۵.`@ExceptionId``INT``NULL`شناسه دقیق یک رکورد خطا در ProcessErrorLogs.`@ExceptionOccurredDateTimeFrom``DATETIME``NULL`شروع بازه وقوع خطا روی ProcessErrorLogs.CreateDate؛ >= و مستقل از تاریخ ایجاد نمونه.`@ExceptionOccurredDateTimeTo``DATETIME``NULL`پایان بازه وقوع خطا روی ProcessErrorLogs.CreateDate؛ <=.`@ExceptionTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان خطا.`@ExceptionMessage``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در شرح/پیام خطا.`@ExceptionType``NVARCHAR(500)``NULL`نام فنی نوع استثنا با تطبیق دقیق، مانند FormatException.`@ExceptionOwnerRoleId``INT``NULL`شناسه نقش صاحب خطا.`@ExceptionOwnerRoleName``NVARCHAR(500)``NULL`جست‌وجوی شامل متن در نام نقش صاحب خطا.`@ExceptionOwnerUserId``INT``NULL`شناسه کاربر صاحب خطا.`@CreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه (LIKE با % در دو طرف). کاراکترهای خاص LIKE مانند % و _ در SQL معنای الگو دارند.`@HasCreationFinished``BIT``NULL`وضعیت خاتمه نمونه خطادار؛ NULL بدون فیلتر. در این رویه NULL ستون Finish به صفر تبدیل می‌شود.`@HasCreationDiscarded``BIT``NULL`وضعیت لغو نمونه خطادار؛ NULL بدون فیلتر. در این رویه NULL ستون DiscardProcessState به صفر تبدیل می‌شود.`@ProcessGroupId``INT``NULL`شناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@ElementBpmnId``NVARCHAR(150)``NULL`شناسه BPMN المانی که در خود رکورد خطا ثبت شده، با تطبیق دقیق.`@ElementTypeId``INT``NULL`شناسه نوع المان ثبت‌شده در رکورد خطا.`@ParentProcessGroupId``INT``NULL`شناسه گروه فرآیند والد؛ فقط نمونه/خطای دارای والد متناظر.`@ParentStepBpmnId``NVARCHAR(150)``NULL`شناسه BPMN گام والد (ElementID)، با تطبیق دقیق.`@ParentStepTypeId``INT``NULL`شناسه نوع گام والد، با تطبیق دقیق.`@ParentCreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه والد.`@ExceptionCategory``TINYINT``NULL`کد دسته خطا از ۱ تا ۱۶، مطابق جدول دسته‌بندی این پیوست. NULL یا ۱ یعنی همه؛ کد نامعتبر شرطی را برآورده نمی‌کند و خروجی خالی است.`@CreationId``INT``NULL`شناسه دقیق نمونه ProcessCreation.ID؛ NULL یعنی همه نمونه‌های واجد سایر شرایط.`@DistinctCreations``BIT``0`فقط در رویه عمومی: یک یعنی پس از همه فیلترها از هر نمونه یک خطا، با اولویت جدیدترین CreateDate و سپس بزرگ‌ترین Id؛ صفر یعنی همه خطاها.`@StrictTranslateCheck``BIT``0`فقط در رویه عمومی: یک یعنی برای دسته‌های وابسته به ترجمه، نبود متن لازم خطای RAISERROR می‌دهد؛ صفر یعنی مقایسه معمول SQL.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`CreationId`شناسه نمونه ProcessCreation.ID.`ExceptionId`شناسه یکتای رکورد خطا.`ExceptionOccurredDateTime`تاریخ و ساعت ثبت خطا.`ExceptionOccurredDuration`ثانیه سپری‌شده از وقوع خطا تا GETDATE سرور در لحظه اجرای رویه؛ با کش شدن دیگر خودکار به‌روز نمی‌شود.`ExceptionTitle`عنوان خطا.`ExceptionMessage`شرح/پیام فنی خطا.`ExceptionType`نوع فنی Exception.`ExceptionOwner`مالک/منبع خطا به شکل ذخیره‌شده در لاگ.`ExceptionParameters`پارامترهای ثبت‌شده هنگام خطا؛ ممکن است تهی باشد.`ExceptionErrorType`کد ErrorType ذخیره‌شده در لاگ؛ با نام ExceptionType متفاوت است.`ExceptionOwnerRoleId`شناسه نقش صاحب خطا.`ExceptionOwnerRoleName`نام نقش صاحب خطا.`ExceptionOwnerUserId`شناسه کاربر صاحب خطا.`ExceptionOwnerUserName`نام کاربری صاحب خطا.`CreationTitle`عنوان نمونه اجرایی.`CreationCreateDate`زمان ایجاد نمونه مرتبط؛ در خروجی عمومی خطاها وجود دارد و در رویه گروهی وجود ندارد.`HasCreationFinished`وضعیت فعلی خاتمه نمونه خطادار؛ BIT با NULL→0.`HasCreationDiscarded`وضعیت فعلی لغو نمونه خطادار؛ BIT با NULL→0.`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessId`شناسه نسخه فرآیند در ProcessList.`ProcessGroupName`نام گروه فرآیند.`ElementId`شناسه داخلی المان در ProcessElements.`ElementBpmnId`شناسه BPMN المان.`ElementName`نام المان.`ElementTypeId`شناسه نوع المان.`ElementTypeName`نام نوع المان.`ParentProcessGroupId`شناسه گروه فرآیند والد.`ParentProcessId`شناسه نسخه فرآیند والد.`ParentProcessGroupName`نام گروه فرآیند والد.`ParentStepBpmnId`شناسه BPMN گام والد.`ParentStepId`شناسه داخلی گام والد.`ParentStepName`نام گام والد.`ParentStepTypeId`شناسه نوع گام والد.`ParentStepElementTypeName`نام نوع المان والد.`ParentCreationId`شناسه نمونه والد.`ParentCreationTitle`عنوان نمونه والد.`ParentReferenceId`شناسه ارجاع والد.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- عنوان‌های ترجمه‌ای دسته‌های ۱۱ تا ۱۵ باید دقیقاً با داده ذخیره‌شده سازگار باشند؛ NULL باعث عدم تطبیق آن دسته می‌شود. دسته ۱۶ با ترجمه ناقص ممکن است مجموعه بزرگ‌تری بگیرد.
- تاریخ این فهرست با کارت «نمونه دارای خطا» متفاوت است: کارت با تاریخ ایجاد نمونه کار می‌کند، فهرست با زمان وقوع خطا. برای تطبیق کامل، لایه فراخوان باید فیلتر نسل نمونه را جداگانه رعایت کند.
- SQL ترتیب نهایی خروجی را تضمین نمی‌کند؛ جدیدترین‌ها باید توسط لایه سرویس/جدول مرتب شوند. ROW_NUMBER برای حذف تکرار، به معنی ORDER BY نهایی نیست.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.ProcessServiceTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.Provider`، `dbo.RoleTable`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE ProcessBAM_GetExceptions
    @InvalidOutputServiceData_Translate NVARCHAR(500) = NULL,
    @ErrorInGetValVar_Translate NVARCHAR(500) = NULL,
    @AnExceptionOccurredDuringStartingOrContinuingProcess_Translate NVARCHAR(500) = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate NVARCHAR(500) = NULL,
    @ExceptionId INT = NULL,
    @ExceptionOccurredDateTimeFrom DATETIME = NULL,
    @ExceptionOccurredDateTimeTo DATETIME = NULL,
    @ExceptionTitle NVARCHAR(MAX) = NULL,
    @ExceptionMessage NVARCHAR(MAX) = NULL,
    @ExceptionType NVARCHAR(500) = NULL,
    @ExceptionOwnerRoleId INT = NULL,
    @ExceptionOwnerRoleName NVARCHAR(500) = NULL,
    @ExceptionOwnerUserId INT = NULL,
    @CreationTitle NVARCHAR(MAX) = NULL,
    @HasCreationFinished BIT = NULL,
    @HasCreationDiscarded BIT = NULL,
    @ProcessGroupId INT = NULL,
    @ElementBpmnId NVARCHAR(150) = NULL,
    @ElementTypeId INT = NULL,
    @ParentProcessGroupId INT = NULL,
    @ParentStepBpmnId NVARCHAR(150) = NULL,
    @ParentStepTypeId INT = NULL,
    @ParentCreationTitle NVARCHAR(MAX) = NULL,
    

    @ExceptionCategory TINYINT = NULL,
    @CreationId INT = NULL,
    
    @DistinctCreations BIT = 0,
    
    @StrictTranslateCheck BIT = 0

### 10. فهرست و ریزگزارش خطاهای یک گروه
`ProcessBAM_GetExceptionsByProcessGroupId`

یک ردیف به‌ازای هر خطای واجد شرایط از ProcessErrorLogs که نمونه غیرآزمایشی معتبر دارد؛ جداول ProcessList، ProcessGroup، المان/نوع، کاربر/نقش و والدها به صورت LEFT JOIN غنی‌سازی می‌شوند. ستون Element از FK_ElementID خود خطا به دست می‌آید.

همه ورودی‌های جست‌وجوی ارائه‌شده با AND ترکیب می‌شوند؛ عنوان/پیام/نام نقش/عنوان نمونه و والد جست‌وجوی شامل متن دارند و شناسه‌ها، نوع فنی خطا و شناسه BPMN تطبیق دقیق دارند. بازه اصلی این فهرست روی زمان وقوع خطاست.

ExceptionCategory از جدول دسته‌بندی این پیوست پیروی می‌کند. جدول Boundary فقط برای ۲، ۳، ۱۶ و جدول نام متغیرهای آغاز سرویس/ESB فقط برای ۶، ۷، ۱۶ پر می‌شود. مرزهای تاریخ فراگیرند.

ExceptionOccurredDuration اختلاف ثانیه از CreateDate خطا تا GETDATE زمان اجرای SQL است. مقدار ذخیره‌شده در کش، گذر زمان بعد از دریافت را منعکس نمی‌کند. این مقدار مدت اجرای گام یا عمر نمونه نیست.

ProcessGroupId اجباری است؛ ProcessId اختیاری امکان محدودسازی به یک نسخه را می‌دهد. این فایل برخلاف عمومی پارامتر DistinctCreations و StrictTranslateCheck ندارد و ستون CreationCreateDate را هم برنمی‌گرداند. حذف تکرار نمونه در Drill-down گروهی باید در لایه فراخوان انجام شود.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@InvalidOutputServiceData_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان «خروجی نامعتبر سرویس» برای تشخیص دسته ۱۱؛ برابری عنوان خطا ملاک است.`@ErrorInGetValVar_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان «خطا در دریافت مقدار متغیر» برای دسته ۱۲.`@AnExceptionOccurredDuringStartingOrContinuingProcess_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان خطای شروع/ادامه؛ همراه نوع المان ۱۸ یا ۲۳ برای دسته ۱۳ یا ۱۴.`@ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate``NVARCHAR(500)``NULL`متن ترجمه‌شده دقیق عنوان نبود مسیر ادامه/لغو نمونه برای دسته ۱۵.`@ExceptionId``INT``NULL`شناسه دقیق یک رکورد خطا در ProcessErrorLogs.`@ExceptionOccurredDateTimeFrom``DATETIME``NULL`شروع بازه وقوع خطا روی ProcessErrorLogs.CreateDate؛ >= و مستقل از تاریخ ایجاد نمونه.`@ExceptionOccurredDateTimeTo``DATETIME``NULL`پایان بازه وقوع خطا روی ProcessErrorLogs.CreateDate؛ <=.`@ExceptionTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان خطا.`@ExceptionMessage``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در شرح/پیام خطا.`@ExceptionType``NVARCHAR(500)``NULL`نام فنی نوع استثنا با تطبیق دقیق، مانند FormatException.`@ExceptionOwnerRoleId``INT``NULL`شناسه نقش صاحب خطا.`@ExceptionOwnerRoleName``NVARCHAR(500)``NULL`جست‌وجوی شامل متن در نام نقش صاحب خطا.`@ExceptionOwnerUserId``INT``NULL`شناسه کاربر صاحب خطا.`@CreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه (LIKE با % در دو طرف). کاراکترهای خاص LIKE مانند % و _ در SQL معنای الگو دارند.`@HasCreationFinished``BIT``NULL`وضعیت خاتمه نمونه خطادار؛ NULL بدون فیلتر. در این رویه NULL ستون Finish به صفر تبدیل می‌شود.`@HasCreationDiscarded``BIT``NULL`وضعیت لغو نمونه خطادار؛ NULL بدون فیلتر. در این رویه NULL ستون DiscardProcessState به صفر تبدیل می‌شود.`@ProcessId``INT``NULL`شناسه یک نسخه در ProcessList؛ با Version نمایشی یا شناسه ProcessGroup یکسان نیست. NULL یعنی همه نسخه‌های گروه.`@ElementBpmnId``NVARCHAR(150)``NULL`شناسه BPMN المانی که در خود رکورد خطا ثبت شده، با تطبیق دقیق.`@ElementTypeId``INT``NULL`شناسه نوع المان ثبت‌شده در رکورد خطا.`@ParentProcessGroupId``INT``NULL`شناسه گروه فرآیند والد؛ فقط نمونه/خطای دارای والد متناظر.`@ParentStepBpmnId``NVARCHAR(150)``NULL`شناسه BPMN گام والد (ElementID)، با تطبیق دقیق.`@ParentStepTypeId``INT``NULL`شناسه نوع گام والد، با تطبیق دقیق.`@ParentCreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه والد.`@ExceptionCategory``TINYINT``NULL`کد دسته خطا از ۱ تا ۱۶، مطابق جدول دسته‌بندی این پیوست. NULL یا ۱ یعنی همه؛ کد نامعتبر شرطی را برآورده نمی‌کند و خروجی خالی است.`@CreationId``INT``NULL`شناسه دقیق نمونه ProcessCreation.ID؛ NULL یعنی همه نمونه‌های واجد سایر شرایط.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`CreationId`شناسه نمونه ProcessCreation.ID.`ExceptionId`شناسه یکتای رکورد خطا.`ExceptionOccurredDateTime`تاریخ و ساعت ثبت خطا.`ExceptionOccurredDuration`ثانیه سپری‌شده از وقوع خطا تا GETDATE سرور در لحظه اجرای رویه؛ با کش شدن دیگر خودکار به‌روز نمی‌شود.`ExceptionTitle`عنوان خطا.`ExceptionMessage`شرح/پیام فنی خطا.`ExceptionType`نوع فنی Exception.`ExceptionOwner`مالک/منبع خطا به شکل ذخیره‌شده در لاگ.`ExceptionParameters`پارامترهای ثبت‌شده هنگام خطا؛ ممکن است تهی باشد.`ExceptionErrorType`کد ErrorType ذخیره‌شده در لاگ؛ با نام ExceptionType متفاوت است.`ExceptionOwnerRoleId`شناسه نقش صاحب خطا.`ExceptionOwnerRoleName`نام نقش صاحب خطا.`ExceptionOwnerUserId`شناسه کاربر صاحب خطا.`ExceptionOwnerUserName`نام کاربری صاحب خطا.`CreationTitle`عنوان نمونه اجرایی.`HasCreationFinished`وضعیت فعلی خاتمه نمونه خطادار؛ BIT با NULL→0.`HasCreationDiscarded`وضعیت فعلی لغو نمونه خطادار؛ BIT با NULL→0.`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessId`شناسه نسخه فرآیند در ProcessList.`ProcessGroupName`نام گروه فرآیند.`ElementId`شناسه داخلی المان در ProcessElements.`ElementBpmnId`شناسه BPMN المان.`ElementName`نام المان.`ElementTypeId`شناسه نوع المان.`ElementTypeName`نام نوع المان.`ParentProcessGroupId`شناسه گروه فرآیند والد.`ParentProcessId`شناسه نسخه فرآیند والد.`ParentProcessGroupName`نام گروه فرآیند والد.`ParentStepBpmnId`شناسه BPMN گام والد.`ParentStepId`شناسه داخلی گام والد.`ParentStepName`نام گام والد.`ParentStepTypeId`شناسه نوع گام والد.`ParentStepElementTypeName`نام نوع المان والد.`ParentCreationId`شناسه نمونه والد.`ParentCreationTitle`عنوان نمونه والد.`ParentReferenceId`شناسه ارجاع والد.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- عنوان‌های ترجمه‌ای دسته‌های ۱۱ تا ۱۵ باید دقیقاً با داده ذخیره‌شده سازگار باشند؛ NULL باعث عدم تطبیق آن دسته می‌شود. دسته ۱۶ با ترجمه ناقص ممکن است مجموعه بزرگ‌تری بگیرد.
- تاریخ این فهرست با کارت «نمونه دارای خطا» متفاوت است: کارت با تاریخ ایجاد نمونه کار می‌کند، فهرست با زمان وقوع خطا. برای تطبیق کامل، لایه فراخوان باید فیلتر نسل نمونه را جداگانه رعایت کند.
- SQL ترتیب نهایی خروجی را تضمین نمی‌کند؛ جدیدترین‌ها باید توسط لایه سرویس/جدول مرتب شوند. ROW_NUMBER برای حذف تکرار، به معنی ORDER BY نهایی نیست.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessBoundaryEventConfig`، `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.ProcessServiceTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.Provider`، `dbo.RoleTable`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE ProcessBAM_GetExceptionsByProcessGroupId
    @ProcessGroupId INT,
    @InvalidOutputServiceData_Translate NVARCHAR(500) = NULL,
    @ErrorInGetValVar_Translate NVARCHAR(500) = NULL,
    @AnExceptionOccurredDuringStartingOrContinuingProcess_Translate NVARCHAR(500) = NULL,
    @ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscarded_Translate NVARCHAR(500) = NULL,
    @ExceptionId INT = NULL,
    @ExceptionOccurredDateTimeFrom DATETIME = NULL,
    @ExceptionOccurredDateTimeTo DATETIME = NULL,
    @ExceptionTitle NVARCHAR(MAX) = NULL,
    @ExceptionMessage NVARCHAR(MAX) = NULL,
    @ExceptionType NVARCHAR(500) = NULL,
    @ExceptionOwnerRoleId INT = NULL,
    @ExceptionOwnerRoleName NVARCHAR(500) = NULL,
    @ExceptionOwnerUserId INT = NULL,
    @CreationTitle NVARCHAR(MAX) = NULL,
    @HasCreationFinished BIT = NULL,
    @HasCreationDiscarded BIT = NULL,
    @ProcessId INT = NULL,
    @ElementBpmnId NVARCHAR(150) = NULL,
    @ElementTypeId INT = NULL,
    @ParentProcessGroupId INT = NULL,
    @ParentStepBpmnId NVARCHAR(150) = NULL,
    @ParentStepTypeId INT = NULL,
    @ParentCreationTitle NVARCHAR(MAX) = NULL,
    

    @ExceptionCategory TINYINT = NULL,
    @CreationId INT = NULL

### 11. خلاصه نمونه‌های تمام فرآیندها
`ProcessBAM_GetProcessesCreationsSummaries`

فهرست نمونه‌های غیرآزمایشی از ProcessCreation، با پیوند اجباری کاربر آغازکننده، نسخه فرآیند، گروه و گروه ماژول بازگردانده می‌شود. کاربر پایان/لغو و اطلاعات والد پیوند اختیاری دارند. در خود SQL صفحه‌بندی و ORDER BY وجود ندارد.

هر سه بازه زمان مستقل‌اند: بازه آغاز روی CreateDate، بازه پایان روی EndCreateDate و بازه لغو روی DiscardProcessDate. ترکیب آن‌ها با AND است؛ با محدودکردن بازه پایان، نمونه‌های بی‌تاریخ پایان کنار می‌روند.

عنوان نمونه، شرح لغو و عنوان والد LIKE شامل متن هستند؛ باقی فیلترهای شناسه/وضعیت تطبیق مستقیم دارند. NULL ورودی یعنی فیلتر اعمال نشود. فیلتر Finish=0 یا Discard=0 در SQL برخلاف خروجی ISNULL، مقدار NULL ذخیره‌شده را صفر تلقی نمی‌کند.

فیلدهای والد مشخص می‌کنند این نمونه از کدام نمونه، نسخه، گروه و گام والد آغاز شده است. خالی‌بودن آن‌ها معمولاً یعنی والد متناظری در داده ذخیره نشده است.

در نسخه عمومی، ProcessGroupId اختیاری است و فیلتر ProcessId یا IsProcessActive در امضا وجود ندارد. Duration با DATEDIFF_BIG(SECOND) برحسب ثانیه محاسبه می‌شود و برای نمونه بدون زمان پایان NULL است.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@CreationId``INT``NULL`شناسه دقیق نمونه ProcessCreation.ID؛ NULL یعنی همه نمونه‌های واجد سایر شرایط.`@CreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه (LIKE با % در دو طرف). کاراکترهای خاص LIKE مانند % و _ در SQL معنای الگو دارند.`@ProcessGroupId``INT``NULL`شناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@IsProcessSuspended``BIT``NULL`فیلتر معلق بودن نسخه فرآیند؛ این ویژگی تعریف فرآیند است و با خاتمه/لغو نمونه تفاوت دارد.`@ProcessGroupModuleGroupId``INT``NULL`شناسه گروه ماژول منتسب به گروه فرآیند.`@StarterUserId``INT``NULL`شناسه کاربر آغازکننده نمونه (SUID).`@StartRangeOfStartDateTime``DATETIME2``NULL`شروع بازه زمان آغاز نمونه، روی CreateDate با >=.`@EndRangeOfStartDateTime``DATETIME2``NULL`پایان بازه زمان آغاز نمونه، روی CreateDate با <=.`@Finish``BIT``NULL`فیلتر مستقیم Finish نمونه؛ NULL یعنی همه. در فیلتر SQL مقدار NULL ستون با صفر یکسان‌سازی نمی‌شود، هرچند خروجی نمایش آن را صفر می‌کند.`@FinisherUserId``INT``NULL`شناسه کاربر خاتمه‌دهنده نمونه (EUID).`@StartRangeOfEndDateTime``DATETIME2``NULL`شروع بازه زمان خاتمه نمونه، روی EndCreateDate با >=؛ نمونه فاقد تاریخ پایان در بازه قرار نمی‌گیرد.`@EndRangeOfEndDateTime``DATETIME2``NULL`پایان بازه زمان خاتمه نمونه، روی EndCreateDate با <=.`@Discard``BIT``NULL`فیلتر مستقیم DiscardProcessState نمونه؛ NULL بدون محدودیت. صفر با NULL ذخیره‌شده ستون یکسان نیست.`@DiscarderUserId``INT``NULL`شناسه کاربر لغوکننده نمونه.`@DiscardDescription``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در علت لغو نمونه.`@StartRangeOfDiscardDate``DATETIME2``NULL`شروع بازه زمان لغو نمونه، روی DiscardProcessDate با >=.`@EndRangeOfDiscardDate``DATETIME2``NULL`پایان بازه زمان لغو نمونه، روی DiscardProcessDate با <=.`@ParentProcessGroupId``INT``NULL`شناسه گروه فرآیند والد؛ فقط نمونه/خطای دارای والد متناظر.`@ParentCreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه والد.`@ParentStepBpmnId``NVARCHAR(150)``NULL`شناسه BPMN گام والد (ElementID)، با تطبیق دقیق.`@ParentStepTypeId``INT``NULL`شناسه نوع گام والد، با تطبیق دقیق.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`CreationId`شناسه نمونه ProcessCreation.ID.`CreationTitle`عنوان نمونه اجرایی.`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessId`شناسه نسخه فرآیند در ProcessList.`ProcessGroupName`نام گروه فرآیند.`ProcessTitle`ترکیب نام گروه و شناسه نسخه در پرانتز؛ عدد پرانتز Version نیست.`ProcessVersion`شماره نسخه تعریف فرآیند.`IsProcessActive`وضعیت فعال بودن نسخه؛ NULL در خروجی صفر می‌شود.`IsProcessSuspended`وضعیت معلق بودن نسخه فرآیند.`ProcessGroupModuleGroupId`شناسه گروه ماژول فرآیند.`ProcessGroupModuleGroupName`عنوان گروه ماژول.`ProcessGroupIconArchiveFileId`شناسه فایل آیکن گروه در بایگانی.`StarterUserTitle`نام و نام خانوادگی و نام کاربری آغازکننده.`StarterUserId`شناسه کاربر آغازکننده.`StartDateTime`زمان ایجاد نمونه در خلاصه نمونه‌ها؛ زمان ایجاد ارجاع در جزئیات نمونه.`Finish`وضعیت خاتمه نمونه؛ NULL خروجی صفر می‌شود.`FinisherUserTitle`مشخصات کاربر خاتمه‌دهنده؛ در نبود EUID مقدار NULL.`FinisherUserId`شناسه کاربر خاتمه‌دهنده.`EndDateTime`زمان پایان نمونه در خلاصه؛ زمان خروج ارجاع از کارتابل در جزئیات.`Discard`وضعیت لغو نمونه؛ NULL خروجی صفر می‌شود.`DiscarderUserTitle`مشخصات کاربر لغوکننده یا NULL.`DiscarderUserId`شناسه کاربر لغوکننده.`DiscardDescription`علت/شرح لغو نمونه.`DiscardProcessDate`زمان لغو نمونه.`Duration`اختلاف زمان شروع و پایان؛ در نبود زمان پایان NULL. واحد دقیق در شرح همین رویه آمده است.`ParentProcessGroupId`شناسه گروه فرآیند والد.`ParentProcessId`شناسه نسخه فرآیند والد.`ParentProcessGroupName`نام گروه فرآیند والد.`ParentCreationId`شناسه نمونه والد.`ParentCreationTitle`عنوان نمونه والد.`ParentStepBpmnId`شناسه BPMN گام والد.`ParentStepId`شناسه داخلی گام والد.`ParentStepTitle`نام و شناسه BPMN گام والد در پرانتز؛ اگر گام والد نباشد NULL.`ParentStepTypeId`شناسه نوع گام والد.`ParentStepTypeName`نام نوع گام والد.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- INNER JOIN به UsersTable و ModuleGroup سبب می‌شود نمونه فاقد رکورد مرجع معتبر در این فهرست نیاید، حتی اگر در شمارنده‌ای که فقط ProcessCreation را می‌خواند حضور داشته باشد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ModuleGroup`، `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetProcessesCreationsSummaries
    @CreationId INT = NULL,
    @CreationTitle NVARCHAR(MAX) = NULL,
    @ProcessGroupId INT = NULL,
    @IsProcessSuspended BIT = NULL,
    @ProcessGroupModuleGroupId INT = NULL,
    @StarterUserId INT = NULL,
    @StartRangeOfStartDateTime DATETIME2 = NULL,
    @EndRangeOfStartDateTime DATETIME2 = NULL,
    @Finish BIT = NULL,
    @FinisherUserId INT = NULL,
    @StartRangeOfEndDateTime DATETIME2 = NULL,
    @EndRangeOfEndDateTime DATETIME2 = NULL,
    @Discard BIT = NULL,
    @DiscarderUserId INT = NULL,
    @DiscardDescription NVARCHAR(MAX) = NULL,
    @StartRangeOfDiscardDate DATETIME2 = NULL,
    @EndRangeOfDiscardDate DATETIME2 = NULL,
    @ParentProcessGroupId INT = NULL,
    @ParentCreationTitle NVARCHAR(MAX) = NULL,
    @ParentStepBpmnId NVARCHAR(150) = NULL,
    @ParentStepTypeId INT = NULL

### 12. خلاصه نمونه‌های یک گروه فرآیند
`ProcessBAM_GetProcessesCreationsSummariesByProcessGroupId`

فهرست نمونه‌های غیرآزمایشی از ProcessCreation، با پیوند اجباری کاربر آغازکننده، نسخه فرآیند، گروه و گروه ماژول بازگردانده می‌شود. کاربر پایان/لغو و اطلاعات والد پیوند اختیاری دارند. در خود SQL صفحه‌بندی و ORDER BY وجود ندارد.

هر سه بازه زمان مستقل‌اند: بازه آغاز روی CreateDate، بازه پایان روی EndCreateDate و بازه لغو روی DiscardProcessDate. ترکیب آن‌ها با AND است؛ با محدودکردن بازه پایان، نمونه‌های بی‌تاریخ پایان کنار می‌روند.

عنوان نمونه، شرح لغو و عنوان والد LIKE شامل متن هستند؛ باقی فیلترهای شناسه/وضعیت تطبیق مستقیم دارند. NULL ورودی یعنی فیلتر اعمال نشود. فیلتر Finish=0 یا Discard=0 در SQL برخلاف خروجی ISNULL، مقدار NULL ذخیره‌شده را صفر تلقی نمی‌کند.

فیلدهای والد مشخص می‌کنند این نمونه از کدام نمونه، نسخه، گروه و گام والد آغاز شده است. خالی‌بودن آن‌ها معمولاً یعنی والد متناظری در داده ذخیره نشده است.

ProcessGroupId اجباری است و ورودی‌های اختیاری ProcessId و IsProcessActive نیز اضافه شده‌اند؛ کاربر می‌تواند نمونه‌های یک نسخه و وضعیت فعال‌بودن آن نسخه را محدود کند.

**تفاوت واحد مدت در قراردادها: **تفاوت واقعی قرارداد توصیف‌شده: Duration در این نسخه با DATEDIFF_BIG(DAY) محاسبه می‌شود و تعداد عبور از مرز روز تقویمی را می‌دهد؛ این واحد با نسخه عمومی که SECOND است فرق دارد. این اختلاف در پیوست کیفیت داده ثبت شده است.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@CreationId``INT``NULL`شناسه دقیق نمونه ProcessCreation.ID؛ NULL یعنی همه نمونه‌های واجد سایر شرایط.`@CreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه (LIKE با % در دو طرف). کاراکترهای خاص LIKE مانند % و _ در SQL معنای الگو دارند.`@ProcessId``INT``NULL`شناسه یک نسخه در ProcessList؛ با Version نمایشی یا شناسه ProcessGroup یکسان نیست. NULL یعنی همه نسخه‌های گروه.`@IsProcessActive``BIT``NULL`فیلتر وضعیت فعال بودن نسخه فرآیند؛ NULL بدون محدودیت، یک فعال، صفر غیرفعال.`@IsProcessSuspended``BIT``NULL`فیلتر معلق بودن نسخه فرآیند؛ این ویژگی تعریف فرآیند است و با خاتمه/لغو نمونه تفاوت دارد.`@ProcessGroupModuleGroupId``INT``NULL`شناسه گروه ماژول منتسب به گروه فرآیند.`@StarterUserId``INT``NULL`شناسه کاربر آغازکننده نمونه (SUID).`@StartRangeOfStartDateTime``DATETIME2``NULL`شروع بازه زمان آغاز نمونه، روی CreateDate با >=.`@EndRangeOfStartDateTime``DATETIME2``NULL`پایان بازه زمان آغاز نمونه، روی CreateDate با <=.`@Finish``BIT``NULL`فیلتر مستقیم Finish نمونه؛ NULL یعنی همه. در فیلتر SQL مقدار NULL ستون با صفر یکسان‌سازی نمی‌شود، هرچند خروجی نمایش آن را صفر می‌کند.`@FinisherUserId``INT``NULL`شناسه کاربر خاتمه‌دهنده نمونه (EUID).`@StartRangeOfEndDateTime``DATETIME2``NULL`شروع بازه زمان خاتمه نمونه، روی EndCreateDate با >=؛ نمونه فاقد تاریخ پایان در بازه قرار نمی‌گیرد.`@EndRangeOfEndDateTime``DATETIME2``NULL`پایان بازه زمان خاتمه نمونه، روی EndCreateDate با <=.`@Discard``BIT``NULL`فیلتر مستقیم DiscardProcessState نمونه؛ NULL بدون محدودیت. صفر با NULL ذخیره‌شده ستون یکسان نیست.`@DiscarderUserId``INT``NULL`شناسه کاربر لغوکننده نمونه.`@DiscardDescription``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در علت لغو نمونه.`@StartRangeOfDiscardDate``DATETIME2``NULL`شروع بازه زمان لغو نمونه، روی DiscardProcessDate با >=.`@EndRangeOfDiscardDate``DATETIME2``NULL`پایان بازه زمان لغو نمونه، روی DiscardProcessDate با <=.`@ParentProcessGroupId``INT``NULL`شناسه گروه فرآیند والد؛ فقط نمونه/خطای دارای والد متناظر.`@ParentCreationTitle``NVARCHAR(MAX)``NULL`جست‌وجوی شامل متن در عنوان نمونه والد.`@ParentStepBpmnId``NVARCHAR(150)``NULL`شناسه BPMN گام والد (ElementID)، با تطبیق دقیق.`@ParentStepTypeId``INT``NULL`شناسه نوع گام والد، با تطبیق دقیق.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`CreationId`شناسه نمونه ProcessCreation.ID.`CreationTitle`عنوان نمونه اجرایی.`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessId`شناسه نسخه فرآیند در ProcessList.`ProcessGroupName`نام گروه فرآیند.`ProcessTitle`ترکیب نام گروه و شناسه نسخه در پرانتز؛ عدد پرانتز Version نیست.`ProcessVersion`شماره نسخه تعریف فرآیند.`IsProcessActive`وضعیت فعال بودن نسخه؛ NULL در خروجی صفر می‌شود.`IsProcessSuspended`وضعیت معلق بودن نسخه فرآیند.`ProcessGroupModuleGroupId`شناسه گروه ماژول فرآیند.`ProcessGroupModuleGroupName`عنوان گروه ماژول.`ProcessGroupIconArchiveFileId`شناسه فایل آیکن گروه در بایگانی.`StarterUserTitle`نام و نام خانوادگی و نام کاربری آغازکننده.`StarterUserId`شناسه کاربر آغازکننده.`StartDateTime`زمان ایجاد نمونه در خلاصه نمونه‌ها؛ زمان ایجاد ارجاع در جزئیات نمونه.`Finish`وضعیت خاتمه نمونه؛ NULL خروجی صفر می‌شود.`FinisherUserTitle`مشخصات کاربر خاتمه‌دهنده؛ در نبود EUID مقدار NULL.`FinisherUserId`شناسه کاربر خاتمه‌دهنده.`EndDateTime`زمان پایان نمونه در خلاصه؛ زمان خروج ارجاع از کارتابل در جزئیات.`Discard`وضعیت لغو نمونه؛ NULL خروجی صفر می‌شود.`DiscarderUserTitle`مشخصات کاربر لغوکننده یا NULL.`DiscarderUserId`شناسه کاربر لغوکننده.`DiscardDescription`علت/شرح لغو نمونه.`DiscardProcessDate`زمان لغو نمونه.`Duration`اختلاف زمان شروع و پایان؛ در نبود زمان پایان NULL. واحد دقیق در شرح همین رویه آمده است.`ParentProcessGroupId`شناسه گروه فرآیند والد.`ParentProcessId`شناسه نسخه فرآیند والد.`ParentProcessGroupName`نام گروه فرآیند والد.`ParentCreationId`شناسه نمونه والد.`ParentCreationTitle`عنوان نمونه والد.`ParentStepBpmnId`شناسه BPMN گام والد.`ParentStepId`شناسه داخلی گام والد.`ParentStepTitle`نام و شناسه BPMN گام والد در پرانتز؛ اگر گام والد نباشد NULL.`ParentStepTypeId`شناسه نوع گام والد.`ParentStepTypeName`نام نوع گام والد.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- اگر فرانت خروجی هر دو رویه را به‌عنوان ثانیه قالب‌بندی کند، مدت نمونه در تب گروه نادرست نمایش داده می‌شود. این موضوع نیازمند اصلاح کد است و سند نباید آن را مدت دقیق اجرا معرفی کند.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ModuleGroup`، `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetProcessesCreationsSummariesByProcessGroupId
    @ProcessGroupId INT,
    @CreationId INT = NULL,
    @CreationTitle NVARCHAR(MAX) = NULL,
    @ProcessId INT = NULL,
    @IsProcessActive BIT = NULL,
    @IsProcessSuspended BIT = NULL,
    @ProcessGroupModuleGroupId INT = NULL,
    @StarterUserId INT = NULL,
    @StartRangeOfStartDateTime DATETIME2 = NULL,
    @EndRangeOfStartDateTime DATETIME2 = NULL,
    @Finish BIT = NULL,
    @FinisherUserId INT = NULL,
    @StartRangeOfEndDateTime DATETIME2 = NULL,
    @EndRangeOfEndDateTime DATETIME2 = NULL,
    @Discard BIT = NULL,
    @DiscarderUserId INT = NULL,
    @DiscardDescription NVARCHAR(MAX) = NULL,
    @StartRangeOfDiscardDate DATETIME2 = NULL,
    @EndRangeOfDiscardDate DATETIME2 = NULL,
    @ParentProcessGroupId INT = NULL,
    @ParentCreationTitle NVARCHAR(MAX) = NULL,
    @ParentStepBpmnId NVARCHAR(150) = NULL,
    @ParentStepTypeId INT = NULL

### 13. جزئیات ارجاع‌های یک نمونه
`ProcessBAM_GetProcessCreationDetails`

ورودی یک شناسه نمونه است و تمام ProcessReferences آن، همراه المان و نوع المان، گیرنده، نقش، زمان‌ها، فرم/موجودیت، یادداشت، متغیرها و خطاهای مرتبط بازمی‌گردد. ORDER BY بر ReferencesID صعودی است.

Duration اختلاف ثانیه شروع ارجاع تا ExitCartableDateTime است؛ ارجاع باز، Duration و EndDateTime تهی دارد. زمان مشاهده ViewedAt از خروج کارتابل مستقل است.

یادداشت از ProcessReferNotes.ReferenceNote گرفته می‌شود و اگر NULL باشد ReferencesExplain جایگزین می‌شود. تنظیم فرم و UserTask فقط برای نوع المان ۱ پیوند می‌شود.

متغیرهای خروجی با FK_ProcessReferenceId و خطاها با FK_ReferenceID به همان ارجاع وصل می‌شوند؛ فرم/موجودیت شامل Url، EntityId، EntityTitle، EntityTypeId، ElementConfigId و EntityMode است.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessCreationId``INT`اجباری؛ بدون پیش‌فرضشناسه دقیق یک نمونه برای بازگرداندن جزئیات تمام ارجاع‌های آن؛ اجباری.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ReferencesId`شناسه ارجاع؛ یک ارجاع ممکن است به‌علت پیوند چند لاگ/خطا در چند ردیف خروجی تکرار شود.`ElementTitle`نام المان همراه شناسه BPMN در پرانتز.`ElementTypeId`شناسه نوع المان.`ElementTypeName`نام نوع المان.`ElementId`شناسه داخلی المان در ProcessElements.`ReferencesRead`وضعیت خوانده‌شدن ارجاع، به شکل ذخیره‌شده.`RecieverUserId`شناسه کاربر گیرنده ارجاع (املای Reciever در قرارداد خروجی حفظ شده است).`RecieverUserTitle`نام و نام خانوادگی و نام کاربری گیرنده؛ در نبود گیرنده NULL.`RecieverRoleId`شناسه نقش گیرنده.`RecieverRoleName`عنوان نقش گیرنده.`StartDateTime`زمان ایجاد نمونه در خلاصه نمونه‌ها؛ زمان ایجاد ارجاع در جزئیات نمونه.`EndDateTime`زمان پایان نمونه در خلاصه؛ زمان خروج ارجاع از کارتابل در جزئیات.`Duration`اختلاف زمان شروع و پایان؛ در نبود زمان پایان NULL. واحد دقیق در شرح همین رویه آمده است.`DraftFormState`وضعیت پیش‌نویس فرم در ارجاع.`IsView`پرچم مشاهده ارجاع.`ViewedAt`زمان مشاهده ارجاع.`ReferencesNoteExplain`ReferenceNote در صورت غیرNULL بودن، وگرنه ReferencesExplain؛ رشته خالی نیز مقدار معتبر محسوب می‌شود.`Url`نشانی فرم/موجودیت از ProcessEntityConfig.`EntityId`شناسه FKFormsConfigID ارجاع؛ تعبیر آن باید با EntityType/EntityMode همراه شود.`EntityTitle`اگر IsCustomForm=1 باشد Description تنظیمات؛ وگرنه Title فرم GBForm متناظر.`EntityTypeId`نوع موجودیت/فرم در تنظیمات.`ElementConfigId`شناسه تنظیمات UserTask.`EntityMode`حالت موجودیت در تنظیمات UserTask.`OutputVariableLogId`شناسه لاگ متغیر مرتبط با همین ارجاع.`OutputVariableName`نام متغیر ثبت‌شده در لاگ ارجاع.`OutputVariableValue`مقدار متغیر ثبت‌شده.`ExceptionId`شناسه یکتای رکورد خطا.`ExceptionOccurredDateTime`تاریخ و ساعت ثبت خطا.`ExceptionTitle`عنوان خطا.`ExceptionMessage`شرح/پیام فنی خطا.`ExceptionType`نوع فنی Exception.`ExceptionOwner`مالک/منبع خطا به شکل ذخیره‌شده در لاگ.`ExceptionParameters`پارامترهای ثبت‌شده هنگام خطا؛ ممکن است تهی باشد.`ExceptionErrorType`کد ErrorType ذخیره‌شده در لاگ؛ با نام ExceptionType متفاوت است.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- چند یادداشت، چند لاگ متغیر و چند خطا برای یک ارجاع می‌توانند ضرب ردیف‌های JOIN ایجاد کنند. شمارش ساده ردیف خروجی معادل تعداد گام‌ها نیست؛ لایه نمایش باید برحسب شناسه ارجاع گروه‌بندی کند.
- این رویه در خود WHERE فقط شناسه نمونه دارد و فیلتر تاریخ یا TestEnvironmentId ندارد؛ دامنه مجاز نمونه باید در مسیر فراخوان کنترل شود.

شواهد منبع و امضای دقیق
جداول منبع: `GBForm`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessEntityConfig`، `dbo.ProcessErrorLogs`، `dbo.ProcessReferNotes`، `dbo.ProcessReferences`، `dbo.ProcessUserTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.RoleTable`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetProcessCreationDetails @ProcessCreationId INT

### 14. تاریخچه متغیرهای یک یا چند نمونه
`ProcessBAM_GetProcessesCreationsVariablesLogs`

از Process_AllProcessVariablesLog تمام تغییرات متعلق به شناسه‌های نمونه ورودی را می‌خواند؛ جدیدترین Id لاگ ابتدا می‌آید. هر ردیف یک ثبت تاریخی متغیر است و مقدار قبلی یا بعدی دیگری را حذف نمی‌کند.

نام کاربری با FK_UserId تکمیل می‌شود. OUTER APPLY المان ارجاع را از ProcessReferences می‌یابد؛ اگر لاگ ارجاع داشته باشد، عنوان گام و فرم UserTask مرتبط نیز استخراج می‌شود.

لیست ورودی با STRING_SPLIT شکسته و با CAST شناسه نمونه به NVARCHAR مقایسه می‌شود. رویه این لیست را به جدول INT تبدیل یا TRY_CAST نمی‌کند.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessesCreationsIds``NVARCHAR(MAX)`اجباری؛ بدون پیش‌فرضرشته شناسه نمونه‌ها با جداکننده ویرگول؛ مثلاً 30046,30047. تطبیق با مقدار رشته‌ای شناسه انجام می‌شود؛ لیست خالی معمولاً خروجی خالی می‌دهد.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`Name`نام متغیر در لاگ.`Value`مقدار تاریخی ذخیره‌شده متغیر، نه الزاماً آخرین مقدار فعلی.`CreateDate`تاریخ و ساعت ثبت لاگ متغیر.`ProcessCreationId`شناسه نمونه صاحب لاگ.`CreatorUserId`شناسه ثبت‌کننده لاگ متغیر.`CreatorUserName`نام کاربری ثبت‌کننده لاگ متغیر.`Reference`نام المان و شناسه BPMN ارجاع مرتبط؛ برای متغیرهای فاقد ارجاع NULL.`UserTaskFormId`شناسه فرم ProcessEntityConfig.FK_FormID از تنظیم UserTask گام مرتبط؛ ممکن است NULL باشد.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- این رویه فیلتر تاریخ، نوع متغیر یا محیط آزمایشی ندارد؛ تمام تاریخچه نمونه‌های ورودی را می‌گیرد. وجود چند ردیف برای یک نام متغیر طبیعی است.
- شناسه‌ها را به شکل عدد معمولی و با ویرگول ارسال کنید؛ صفرهای پیشرو و فاصله ابتدایی می‌توانند به‌علت مقایسه رشته‌ای با قالب شناسه ذخیره‌شده متفاوت شوند.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessElements`، `dbo.ProcessEntityConfig`، `dbo.ProcessReferences`، `dbo.ProcessUserTaskConfig`، `dbo.Process_AllProcessVariablesLog`، `dbo.UsersTable`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetProcessesCreationsVariablesLogs @ProcessesCreationsIds NVARCHAR(MAX)

### 15. فهرست انواع فنی خطا
`ProcessBAM_GetExceptionsTypes`

بدون ورودی؛ DISTINCT ExceptionType از کل ProcessErrorLogs را برای پرکردن انتخاب‌گر نوع خطا برمی‌گرداند. تنها یک ستون ExceptionType دارد.

همه ورودی‌ها، نوع و پیش‌فرض

بدون پارامتر ورودی.

همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ExceptionType`نوع فنی Exception.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- فیلتر گروه، تاریخ و محیط آزمایشی ندارد؛ بنابراین گزینه‌ای ممکن است در انتخاب‌گر باشد که در گزارش فیلترشده فعلی هیچ ردیفی ندارد. NULL نیز در صورت وجود در داده می‌تواند بازگردد و ترتیب تضمین‌شده نیست.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessErrorLogs`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetExceptionsTypes

### 16. آمار زمان گام‌ها در کل سامانه
`ProcessBAM_GetStepsDurations`

آمار روی ProcessReferences با پایان غیرNULL محاسبه می‌شود؛ یعنی فقط ارجاع‌های خارج‌شده از کارتابل. ورودی تاریخ روی ReferencesCreateDate (شروع گام) اعمال می‌شود، نه تاریخ ایجاد نمونه و نه تاریخ خروج گام.

پیوندهای اجباری شامل نمونه، المان، Lane و نسخه فرآیند هستند. نمونه‌های آزمایشی کنار گذاشته می‌شوند. IncludeCopies=0 فقط اصلی و =1 فقط رونوشت را می‌گیرد؛ ارجاع با IsCopy NULL هیچ‌یک را برآورده نمی‌کند.

هر ردیف شامل تعداد ارجاع و کمینه، میانگین و بیشینه DATEDIFF_BIG(SECOND) تا ExitCartableDateTime است. ترتیب بر MaximumDuration نزولی است؛ ردیف اول بیشترین تأخیر ثبت‌شده در میان ردیف‌ها را دارد.

گروه‌بندی با شناسه BPMN و نام و نوع گام و شناسه/نام Lane انجام می‌شود. نسخه‌های مختلف با همین کلیدها می‌توانند در یک ردیف ادغام شوند؛ برای تحلیل نسخه، ProcessId را در حالت گروهی مشخص کنید.

خروجی عمومی ProcessGroupId و ProcessGroupName را نیز دارد و این دو در GROUP BY هستند؛ نام مشابه گام در دو گروه با هم ادغام نمی‌شود.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@StepTypeId``INT``NULL`شناسه نوع گام در ProcessElementTypes؛ NULL یعنی همه انواع. مثلاً نوع UserTask در این پیاده‌سازی شناسه ۱ دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@IncludeCopies``BIT``0`انتخاب نوع ارجاع: صفر فقط ارجاع اصلی، یک فقط رونوشت. شرط SQL برابری IsCopy است؛ مقدار یک به معنی جمع اصلی و رونوشت نیست.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessGroupName`نام گروه فرآیند.`StepBpmnId`شناسه BPMN گام؛ مبنای گروه‌بندی در کنار نام و نوع.`StepName`نام گام.`StepType`نام نوع گام، مانند UserTask یا ScriptTask.`StepLaneBpmnId`شناسه BPMN Lane گام.`StepLaneName`نام Lane گام.`Count`در مدت گام‌ها: تعداد ارجاع خاتمه‌یافته؛ در خطای گام‌ها: تعداد رکورد خطا، نه تعداد نمونه یکتا.`MinimumDuration`کمترین فاصله ReferencesCreateDate تا ExitCartableDateTime، برحسب ثانیه.`AverageDuration`میانگین فاصله ReferencesCreateDate تا ExitCartableDateTime؛ AVG روی BIGINT است و بخش اعشاری ثانیه حفظ نمی‌شود.`MaximumDuration`بیشترین فاصله ReferencesCreateDate تا ExitCartableDateTime، برحسب ثانیه.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- گام فاقد Lane معتبر به‌علت INNER JOIN حذف می‌شود. زمان منفی/غیرعادی به‌صورت خودکار پاک‌سازی نمی‌شود. AVG روی BIGINT ثانیه، دقت اعشاری ندارد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessGroup`، `dbo.ProcessLanes`، `dbo.ProcessList`، `dbo.ProcessReferences`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetStepsDurations
    @StepTypeId INT = NULL,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @IncludeCopies BIT = 0

### 17. آمار زمان گام‌های یک گروه
`ProcessBAM_GetStepsDurationsByProcessGroupId`

آمار روی ProcessReferences با پایان غیرNULL محاسبه می‌شود؛ یعنی فقط ارجاع‌های خارج‌شده از کارتابل. ورودی تاریخ روی ReferencesCreateDate (شروع گام) اعمال می‌شود، نه تاریخ ایجاد نمونه و نه تاریخ خروج گام.

پیوندهای اجباری شامل نمونه، المان، Lane و نسخه فرآیند هستند. نمونه‌های آزمایشی کنار گذاشته می‌شوند. IncludeCopies=0 فقط اصلی و =1 فقط رونوشت را می‌گیرد؛ ارجاع با IsCopy NULL هیچ‌یک را برآورده نمی‌کند.

هر ردیف شامل تعداد ارجاع و کمینه، میانگین و بیشینه DATEDIFF_BIG(SECOND) تا ExitCartableDateTime است. ترتیب بر MaximumDuration نزولی است؛ ردیف اول بیشترین تأخیر ثبت‌شده در میان ردیف‌ها را دارد.

گروه‌بندی با شناسه BPMN و نام و نوع گام و شناسه/نام Lane انجام می‌شود. نسخه‌های مختلف با همین کلیدها می‌توانند در یک ردیف ادغام شوند؛ برای تحلیل نسخه، ProcessId را در حالت گروهی مشخص کنید.

ProcessGroupId اجباری و ProcessId اختیاری است. ستون‌های هویت گروه از خروجی حذف شده‌اند؛ همان دامنه گروه از ورودی معلوم است.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@ProcessId``INT``NULL`شناسه یک نسخه در ProcessList؛ با Version نمایشی یا شناسه ProcessGroup یکسان نیست. NULL یعنی همه نسخه‌های گروه.`@StepTypeId``INT``NULL`شناسه نوع گام در ProcessElementTypes؛ NULL یعنی همه انواع. مثلاً نوع UserTask در این پیاده‌سازی شناسه ۱ دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.`@IncludeCopies``BIT``0`انتخاب نوع ارجاع: صفر فقط ارجاع اصلی، یک فقط رونوشت. شرط SQL برابری IsCopy است؛ مقدار یک به معنی جمع اصلی و رونوشت نیست.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`StepBpmnId`شناسه BPMN گام؛ مبنای گروه‌بندی در کنار نام و نوع.`StepName`نام گام.`StepType`نام نوع گام، مانند UserTask یا ScriptTask.`StepLaneBpmnId`شناسه BPMN Lane گام.`StepLaneName`نام Lane گام.`Count`در مدت گام‌ها: تعداد ارجاع خاتمه‌یافته؛ در خطای گام‌ها: تعداد رکورد خطا، نه تعداد نمونه یکتا.`MinimumDuration`کمترین فاصله ReferencesCreateDate تا ExitCartableDateTime، برحسب ثانیه.`AverageDuration`میانگین فاصله ReferencesCreateDate تا ExitCartableDateTime؛ AVG روی BIGINT است و بخش اعشاری ثانیه حفظ نمی‌شود.`MaximumDuration`بیشترین فاصله ReferencesCreateDate تا ExitCartableDateTime، برحسب ثانیه.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- این گزارش تعداد نمونه یکتای فرآیند نیست؛ تکرار اجرای یک گام در یک نمونه یا چند ارجاع آن می‌تواند Count را افزایش دهد. ارجاع باز در آمار مدت نیست.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessLanes`، `dbo.ProcessList`، `dbo.ProcessReferences`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetStepsDurationsByProcessGroupId
    @ProcessGroupId INT,
    @ProcessId INT = NULL,
    @StepTypeId INT = NULL,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL,
    @IncludeCopies BIT = 0

### 18. تعداد خطاها برحسب آخرین گام نمونه در کل سامانه
`ProcessBAM_GetStepsExceptions`

خطاهای ProcessErrorLogs به نمونه غیرآزمایشی، نسخه و گروه وصل می‌شوند و تاریخ روی PEL.CreateDate (وقوع خطا) اعمال می‌شود. هر رکورد خطا یک واحد Count است.

انتساب گام در این رویه با CROSS APPLY و TOP(1) از آخرین ارجاع نمونه برحسب ReferencesID DESC انجام می‌شود. بنابراین گام گزارش‌شده آخرین گام ارجاعی نمونه است و الزاماً همان FK_ElementID یا FK_ReferenceID ثبت‌شده روی خطا نیست.

فیلتر StepTypeId روی نوع همین آخرین گام اعمال می‌شود. گروه‌بندی بر شناسه BPMN، نام و نوع گام است و خروجی بر Count نزولی مرتب می‌شود.

نسخه عمومی هویت گروه را نیز در خروجی و GROUP BY دارد.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@StepTypeId``INT``NULL`شناسه نوع گام در ProcessElementTypes؛ NULL یعنی همه انواع. مثلاً نوع UserTask در این پیاده‌سازی شناسه ۱ دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`ProcessGroupId`شناسه گروه تعریف فرآیند.`ProcessGroupName`نام گروه فرآیند.`StepBpmnId`شناسه BPMN گام؛ مبنای گروه‌بندی در کنار نام و نوع.`StepName`نام گام.`StepType`نام نوع گام، مانند UserTask یا ScriptTask.`Count`در مدت گام‌ها: تعداد ارجاع خاتمه‌یافته؛ در خطای گام‌ها: تعداد رکورد خطا، نه تعداد نمونه یکتا.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- نمونه بدون ارجاع به‌علت CROSS APPLY از این گزارش حذف می‌شود. این شمارش تحلیل محل واقعی وقوع خطا نیست؛ برای آن از ستون‌های المان در فهرست خطا استفاده کنید.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.ProcessReferences`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetStepsExceptions
    @StepTypeId INT = NULL,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL

### 19. تعداد خطاها برحسب آخرین گام نمونه در یک گروه
`ProcessBAM_GetStepsExceptionsByProcessGroupId`

خطاهای ProcessErrorLogs به نمونه غیرآزمایشی، نسخه و گروه وصل می‌شوند و تاریخ روی PEL.CreateDate (وقوع خطا) اعمال می‌شود. هر رکورد خطا یک واحد Count است.

انتساب گام در این رویه با CROSS APPLY و TOP(1) از آخرین ارجاع نمونه برحسب ReferencesID DESC انجام می‌شود. بنابراین گام گزارش‌شده آخرین گام ارجاعی نمونه است و الزاماً همان FK_ElementID یا FK_ReferenceID ثبت‌شده روی خطا نیست.

فیلتر StepTypeId روی نوع همین آخرین گام اعمال می‌شود. گروه‌بندی بر شناسه BPMN، نام و نوع گام است و خروجی بر Count نزولی مرتب می‌شود.

علاوه بر گروه اجباری، ProcessId اختیاری برای یک نسخه دارد. هویت گروه در خروجی تکرار نمی‌شود.

همه ورودی‌ها، نوع و پیش‌فرض
پارامترنوع SQLپیش‌فرضمعنا و اثر`@ProcessGroupId``INT`اجباری؛ بدون پیش‌فرضشناسه گروه فرآیند. در رویه‌های ByProcessGroupId اجباری است و تمام نسخه‌های آن گروه را محدود می‌کند؛ با شناسه نسخه ProcessId تفاوت دارد.`@ProcessId``INT``NULL`شناسه یک نسخه در ProcessList؛ با Version نمایشی یا شناسه ProcessGroup یکسان نیست. NULL یعنی همه نسخه‌های گروه.`@StepTypeId``INT``NULL`شناسه نوع گام در ProcessElementTypes؛ NULL یعنی همه انواع. مثلاً نوع UserTask در این پیاده‌سازی شناسه ۱ دارد.`@FromDateTime``DATETIME``NULL`مرز شروع فراگیر بازه (>=). مبنای تاریخ در شرح همین رویه تعیین شده است. NULL یعنی بدون حد پایین، مگر در تجمیع پراکندگی که از قدیمی‌ترین ایجاد استفاده می‌شود.`@ToDateTime``DATETIME``NULL`مرز پایان فراگیر بازه (<=). برای پوشش کل امروز باید زمان پایان روز ارسال شود. NULL یعنی بدون حد بالا، مگر در تجمیع پراکندگی که از جدیدترین ایجاد استفاده می‌شود.
همه ستون‌های خروجی
ستون خروجیمعنا / واحد / نحوه محاسبه`StepBpmnId`شناسه BPMN گام؛ مبنای گروه‌بندی در کنار نام و نوع.`StepName`نام گام.`StepType`نام نوع گام، مانند UserTask یا ScriptTask.`Count`در مدت گام‌ها: تعداد ارجاع خاتمه‌یافته؛ در خطای گام‌ها: تعداد رکورد خطا، نه تعداد نمونه یکتا.
نکات تفسیر و اختلاف‌های مهم پیاده‌سازی
- نام نمایشی «تعداد نمونه» اگر برای Count به کار رود، معنای SQL را دقیق منتقل نمی‌کند: یک نمونه با سه خطا، سه واحد است. همچنین گام ممکن است از زمان ثبت خطا جلو رفته باشد.

شواهد منبع و امضای دقیق
جداول منبع: `dbo.ProcessCreation`، `dbo.ProcessElementTypes`، `dbo.ProcessElements`، `dbo.ProcessErrorLogs`، `dbo.ProcessGroup`، `dbo.ProcessList`، `dbo.ProcessReferences`

CREATE OR ALTER PROCEDURE dbo.ProcessBAM_GetStepsExceptionsByProcessGroupId
    @ProcessGroupId INT,
    @ProcessId INT = NULL,
    @StepTypeId INT = NULL,
    @FromDateTime DATETIME = NULL,
    @ToDateTime DATETIME = NULL

## نگاشت دقیق کنترل‌ها، ستون‌ها و راهنماهای رابط

این بخش نام فنی فیلدها و نکات اتصال رابط به گزارش را برای تطبیق درخواست شبکه و خروجی Provider حفظ می‌کند. هر شناسه باید همراه دامنه و واحد محاسبه خوانده شود. تفاوت‌های احتمالی نسخه رابط و بکند با برچسب نیازمند تأیید فنی مشخص می‌شوند.

### شناخت مفاهیم و نقشه صفحه
مفهومتعریف و تفاوتگروه فرآیندخانواده یک فرآیند با نام ثابت و نسخه‌های مختلف. انتخاب dqwdwqd با شناسه 24273 دامنه گزارش را به این خانواده محدود می‌کند.نسخه فرآیند / ProcessIdتعریف اجرایی یک نسخه از گروه فرآیند؛ شماره نسخه با شناسه عددی ProcessId یکی نیست.نمونه / CreationIdیک بار اجرای واقعی فرآیند. ممکن است چند گام، چند ارجاع و چند خطا داشته باشد.گام / الماننود مدل BPMN؛ نام گام برای کاربر و شناسه BpmnId برای شناسایی در مدل استفاده می‌شود.ارجاعواگذاری یک گام به کاربر/نقش. یک نمونه ممکن است بیش از یک ارجاع داشته باشد؛ بنابراین تعداد ارجاعات با تعداد نمونه‌ها الزاماً برابر نیست.رونوشتارجاعی با ماهیت رونوشت؛ در کنترل «بر اساس رونوشت‌ها»، داده فقط از همین ارجاعات محاسبه می‌شود.خطایک رخداد خطا. تعداد رخدادها با تعداد «نمونه دارای خطا» تفاوت دارد: یک نمونه می‌تواند چند خطا داشته باشد.والدفرآیند، نمونه و گامی که اجرای نمونه فرزند از آن آغاز شده است.Laneناحیه/مسیر مسئولیت در مدل BPMN؛ شناسه و نام lane در گزارش گام‌ها قابل مشاهده است.

### نوار بالای صفحه، انتخاب فرآیند و تب‌های گزارش
کنترل / خروجیرفتارعملکرد کلی سیستمبازگشت به گزارش تجمیعی؛ مقدار داخلی ProcessGrpId برابر ‎−1 است.نام تب گروهفعال‌سازی گروه و دریافت/بازیابی گزارش‌های متناظر؛ گزارش‌های عمومی از مسیر ByProcessGroupId استفاده می‌کنند.× روی تببستن تب؛ پیاده‌سازی پس از حذف، آخرین تب باقی‌مانده یا عملکرد کلی سیستم را انتخاب می‌کند.رفرش کنار تب فعالخواندن تازه گزارش‌ها با عبور از کش؛ حرکت آیکون نشان‌دهنده بارگذاری است.ورژن گروه فرآیندعنوان نمای کلی در رابط؛ مقدار از ProcessesGroupsCount می‌آید و شمار گروه‌هاست، نه تعداد نمونه‌های اجرایی.ورژن فرآیند فعالشمار نسخه‌های فعال گروه انتخاب‌شده. پیکان کنارش پنل تعداد نسخه‌های غیر فعال و متوقف‌شده را باز می‌کند.نوار افقی تب‌هاوقتی تعداد تب‌ها زیاد شود امکان حرکت افقی و دسترسی به تب‌های دیگر وجود دارد.باز کردن / بستن منو در نمایش کوچکنمایش یا جمع‌کردن نوار سرصفحه در اندازه‌های کوچک صفحه.

### فیلتر تاریخ صفحه و فیلتر تاریخ هر گزارش

> **متن راهنمای سامانه:** با تغییر بازهٔ زمانی موردنظر از این بخش، می‌توانید بازهٔ انتخاب‌شده را بر روی تمامی فیلترهای بازهٔ زمانی موجود در این صفحه اعمال کنید.
بخشجفت پارامتر دریافت‌کننده تاریخ صفحهجاریNewStartDate / NewEndDateکارتابل و رونوشتInCartableStartDate / InCartableEndDateنمونه دارای خطاOccurredExceptionCreationStartDate / OccurredExceptionCreationEndDateخاتمه‌یافتهFinishedStartDate / FinishedEndDateلغوشدهDiscardStartDate / DiscardEndDateخلاصه عملکردSummaryPerDayFromDateTime / SummaryPerDayToDateTimeپراکندگیDispersionsFromDateTime / DispersionsToDateTimeوضعیت خطاها و جدیدترین خطاهاSystemExceptionsStartDate / SystemExceptionsEndDateآخرین نمونه‌هاRecentCreationsFromDateTime / RecentCreationsToDateTimeجزییات آماری گروهStaticalDetailFromDateTime / StaticalDetailToDateTime
**نیازمند تأیید فنی: **معنای برچسب «اعمال نشده» این است که دیگر نباید فرض کرد همه گزارش‌های صفحه بازه یکسانی دارند. انتظار کاربری این است که تغییر حتی یک تاریخ محلی این برچسب را فعال کند. در قرارداد رابط، اعمال تاریخ اصلی برای تمام ده بخش پیاده شده، اما محاسبه برچسب نمایشی فقط شش جفت تاریخ کارت‌ها و خطاهای سیستم را دنبال می‌کند. بنابراین برای نمودار خلاصه عملکرد، پراکندگی، آخرین نمونه‌ها و جزییات آماری، تاریخ روی خود گزارش معیار قطعی است؛ ممکن است با وجود اختلاف، برچسب ظاهر نشود. این تفاوت، محدودیت قرارداد توصیف‌شده است.

تاریخ‌ها در رابط با تقویم شمسی و در درخواست‌ها با قالب میلادی YYYY-MM-DD HH:mm:ss نگه‌داری می‌شوند. خالی‌کردن یک حد در جستجوی پیشرفته، آن حد را از پارامترهای درخواست حذف می‌کند؛ خالی‌بودن با «امروز» یا «صفر» یکسان نیست. کنترل انتخاب تاریخ، محدودیت endsWith را از زمان جاری می‌گیرد؛ انتخاب پایان روز را پس از تأیید از خود ورودی بررسی کنید.

### شش شاخص بالای صفحه؛ صورت، مخرج، درصد و ارجاع
ورودی / ستونشناسه فنیمعنا و روش استفادهجاریNewCreationsCount / NewCreationsTotalCount / NewCreationsPercentageتعداد نمونه فرآیندهای لغو نشده و به اتمام نرسیده اند. آبی؛ نمونه‌های هنوز تمام‌نشده و لغونشده. نام فنی New به معنی «فقط امروز ساخته‌شده» نیست.کارتابلInCartableCreationsCount / InCartableAndCopiesCreationsTotalCount / InCartableCreationsPercentageتعداد نمونه فرآیندهایی که در کارتابل / رونوشت افراد قرار دارند. نارنجی؛ تعداد نمونه در کارتابل و برچسب «ارجاعات» با InCartableRefsCount. شمار ارجاع می‌تواند از شمار نمونه بیشتر باشد.رونوشتCopiesInCartableCreationsCount / InCartableAndCopiesCreationsTotalCount / CopiesInCartableCreationsPercentageتعداد نمونه فرآیندهایی که در کارتابل / رونوشت افراد قرار دارند. صورتی؛ تعداد نمونه در رونوشت‌ها و CopiesInCartableRefsCount برای شمار ارجاعات رونوشت. تاریخ آن با کارت کارتابل مشترک است.نمونه دارای خطاOccurredExceptionCreationsCount / OccurredExceptionCreationsTotalCount / OccurredExceptionCreationsPercentageتعداد نمونه فرآیند هایی که به هر نحوی با خطا مواجه شده اند (ممکن هست حل شده باشد یا نشده باشد). قرمز؛ تعداد نمونه‌های دارای حداقل یک خطا، حتی اگر خطا بعداً رفع شده باشد.خاتمه‌یافتهFinishedCreationsCount / FinishedCreationsTotalCount / FinishedCreationsPercentageتعداد نمونه فرآیندهایی که به هر نحوی خاتمه یافته اند. سبز؛ نمونه‌هایی که به هر طریق خاتمه یافته‌اند.لغوشدهDiscardedCreationsCount / DiscardedCreationsTotalCount / DiscardedCreationsPercentageتعداد نمونه فرآیندهایی که لغو شده‌اند. خاکستری؛ نمونه‌های لغوشده.

### گزارش خلاصه عملکرد فرآیندها
ورودی / ستونشناسه فنیمعنا و روش استفادهجاریNewCountتعداد نمونه فرآیندهای لغو نشده و به اتمام نرسیده اند.خاتمه‌یافتهFinishedCountتعداد نمونه فرآیندهایی که به هر نحوی خاتمه یافته اند.لغوشدهDiscardedCountتعداد نمونه فرآیندهایی که لغو شده‌اند.در کارتابلInCartableCountتعداد یوزرتسک هایی که به افراد ارجاع داده شده و در انتظار ثبت هستند.تعداد وظایف ثبت‌شدهSubmittedTasksCountتعداد یوزرتسک هایی که به افراد ارجاع داده شده و اقدام شده استخطای مدیریت‌شدهExceptionAutoHandledByErrorBoundaryCountتعداد خطاهایی که در سیستم رخ داده‌اند، اما به‌صورت خودکار توسط سازوکار Error Boundary شناسایی و مدیریت شده‌اند.خطای مدیریت‌نشدهExceptionNotAutoHandledByErrorBoundaryCountتعداد خطاهایی که در سیستم رخ داده‌اند و مدیریت نشده‌اند؛ به این معنا که فاقد سازوکار Error Boundary بوده و در نتیجه هیچ‌گونه مدیریت خطایی بر روی آن‌ها اعمال نشده است.حداکثر زمان انتظار در کارتابلMaximumTaskSubmissionDurationحداکثر زمانی که از ارجاع تسک های جاری در کارتابل میگذرد.کنترلگزینه‌ها / مقدار اولیهاثرشاخص‌هاانتخاب چندتایی از هشت سری؛ ابتدا همه انتخاب‌اندحذف یک تیک سری را از تصویر پنهان می‌کند. این کار فیلتر نمایش است؛ داده اصلی گزارش حذف نمی‌شود. شمار انتخاب‌ها در برچسب نشان داده می‌شود.نوع بازه زمانیروزانه، ماهانه، سالانه؛ ابتدا روزانهشمارها برای ماه و سال جمع می‌شوند؛ مدت انتظار، بیشینه مدت زیرمجموعه است. برای بازه حداکثر یک روز، ماهانه و سالانه غیر فعال و نمودار روزانه می‌شود.نوع چارتانباشته، میله‌ای، خطی؛ ابتدا انباشتهشکل نمایش را تغییر می‌دهد. سری مدت در هر سه حالت به صورت خط/منحنی مستقل است.توابع تجمیعیحداقل، حداکثر، میانگین؛ چندانتخابیتنها وقتی یک سری در نمای کلی سیستم باقی بماند ظاهر می‌شود؛ خطوط مرجع قرمز، نارنجی و آبی روی محور مربوط رسم می‌کند. این امکان در نمای گروه فعال نمی‌شود.بر اساس تاریخ ایجاد نمونهروشن در شروعزمان انتساب رویدادها به نمودار را تعیین می‌کند؛ توضیح دقیق در ادامه آمده است.بر اساس تعداد نمونهروشن در شروعواحد شمارش نمونه را برای شاخص‌های کارتابل/ثبت/خطا در برابر شمار رکورد قرار می‌دهد.بر اساس رونوشت‌هاخاموش در شروعنوع ارجاعات مورد استفاده برای کارتابل، ثبت وظایف و مدت را عوض می‌کند؛ روشن یعنی فقط رونوشت‌ها.راهنمای رنگ و نام سریدر حالت تمام‌صفحهنگه‌داشتن نشانگر روی نام، راهنما را نشان می‌دهد؛ کلیک روی نام سری نمایش آن را روشن/خاموش می‌کند.اسکرول / بزرگ‌نمایی / جابه‌جایینوار زیر نمودار، چرخ ماوس، کشیدن؛ Shift برای جابه‌جاییبرای بررسی دامنه زمانی یا مقادیر متراکم به کار می‌رود؛ مشاهده کامل تاریخ را از برچسب‌ها و راهنمای نقطه کنترل کنید.
> **متن راهنمای سامانه:** با فعال بودن این گزینه، تاریخ ایجاد نمونه مبنای محاسبه همه شاخص‌هاست. با غیرفعال کردن آن، «جاری» همچنان بر اساس تاریخ ایجاد نمونه، «خاتمه‌یافته» بر اساس تاریخ خاتمه و «لغوشده» بر اساس تاریخ لغو محاسبه می‌شوند؛ سایر شاخص‌ها نیز بر اساس تاریخ وقوع رویداد مربوطه محاسبه خواهند شد.

> **متن راهنمای سامانه:** به‌صورت پیش‌فرض، شاخص‌ها بر اساس تعداد نمونه‌های فرآیند محاسبه می‌شوند. با غیرفعال کردن این گزینه، «در کارتابل» و «وظایف ثبت‌شده» بر اساس تعداد ارجاعات و شاخص‌های خطا بر اساس تعداد رکوردهای خطا محاسبه می‌شوند. این گزینه روی «جاری»، «خاتمه‌یافته» و «لغوشده» تأثیری ندارد.

> **متن راهنمای سامانه:** با فعال بودن این گزینه، شاخص‌های «در کارتابل»، «وظایف ثبت‌شده» و زمان ثبت وظایف فقط بر اساس ارجاعات رونوشت محاسبه می‌شوند. با غیرفعال بودن آن، فقط ارجاعات اصلی در محاسبات در نظر گرفته می‌شوند.

مقدار میانگین خطوط مرجع از ردیف‌های فعلی داده نمودار محاسبه می‌شود؛ در سطح سبدبندی، میانگین روی سبدهاست. در نمایش شمارشی متن خط میانگین به عدد صحیح پایین گرد می‌شود. ستون‌های کوچک حداقل ارتفاع بصری دارند؛ مقدار دقیق را از محور/راهنمای نقطه بخوانید و صرفاً از نسبت ارتفاع پیکسل‌ها نتیجه نگیرید. مقدار صفر در رسم ستون به null تبدیل می‌شود تا ستون مصنوعی دیده نشود؛ داده واقعی صفر باقی می‌ماند.
**تطبیق راهنمای نمودار با SQL:**
عنوان و راهنمای «حداکثر زمان انتظار در کارتابل» در رابط از تسک‌های جاری صحبت می‌کند، اما رویه این قرارداد برای این سری فقط ارجاعاتی را می‌خواند که زمان خروج از کارتابل دارند. در حالت «بر اساس تاریخ ایجاد نمونه»، ابتدا میانگین مدت تسک‌های هر نمونه گرفته می‌شود و سپس بیشینه آن میانگین‌ها انتخاب می‌شود؛ بنابراین عدد لزوماً بیشترین انتظار یک تسک جاری یا بیشترین مدت یک ارجاع منفرد نیست. برای تفسیر قطعی، شرایط و فرمول پیوست SQL را مبنا قرار دهید.

در حالت مبتنی بر تاریخ رخداد، یک نمونه ممکن است در چند روز شمرده شود. تجمیع ماهانه/سالانه فرانت، جمع شمارهای روزهاست و شمارش DISTINCT تازه‌ای روی کل ماه/سال انجام نمی‌دهد. پس یک نمونه که در چند روز رویداد داشته ممکن است در جمع ماه چندبار سهم داشته باشد. مدت در ماه/سال با بیشینه ردیف‌های روزانه تجمیع می‌شود.

### پراکندگی نمونه‌ها در فرآیندها
ورودی / خروجیتوضیحتاریخ از / تانمونه‌ها در بازه گزارش پراکندگی؛ مستقل از تاریخ نمودار خلاصه عملکرد قابل تنظیم است.نوع شاخصانتخاب میان جاری، خاتمه‌یافته و مجموع برای مقدار هر قطعه حلقه.نمایش در هر ردیفتیک هر گروه/نسخه تعیین می‌کند در حلقه و محاسبات تجمیعی لحاظ شود. برداشتن تیک شناسه را در ExcludedIds قرار می‌دهد.نمایش همههمه ردیف‌ها را وارد یا خارج می‌کند؛ انتخابِ فقط ردیف‌های قابل مشاهده صفحه نیست.شناسهدر نمای کل شناسه گروه فرآیند؛ در نمای گروه شناسه ProcessId نسخه.عنواندر نمای کل عنوان فرآیند و در نمای گروه عنوان نسخه؛ رنگ نقطه با قطعه نمودار متناظر است.جاریInProcessCreationsCount؛ تعداد جاری هر ردیف.خاتمه‌یافتهFinishedCreationsCount؛ تعداد خاتمه‌یافته هر ردیف.مجموعجمع دو ستون جاری و خاتمه‌یافته در همان ردیف.پنج مورد کنار حلقهپنج سهم بزرگ‌تر از میان ردیف‌های انتخاب‌شده؛ عنوان، تعداد، رنگ و درصد گرد‌شده نمایش داده می‌شود.راهنمای قطعه حلقهنام گروه/نسخه، درصد سهم و مقدار شاخص انتخابی را نشان می‌دهد.میانگین روزانه در مرکزDailyAverageCreations؛ مقدار میانگین محاسبه‌شده برای فرآیندهای انتخاب‌شده، در رابط به عدد صحیح تبدیل می‌شود؛ مقادیر کمتر از یک به صورت صفر دیده می‌شوند.نمونه جدید پایین جدولActiveCreationsCount؛ مقدار تجمیعی خروجی روی انتخاب‌های فعلی. برای تعریف دقیق شرایط جاری/جدید به پیوست SQL مراجعه شود.نمونه خاتمه‌یافته پایین جدولFinishedCreationsCount؛ مقدار تجمیعی خاتمه‌یافته‌های انتخاب‌های فعلی.
> **متن راهنمای سامانه:** تعداد میانگین نمونه فرآیند هایی که به صورت روزانه اجرا میشوند(در صورتی که تعداد میانگین کمتر از 1 باشد، بنابراین میانگین روزانه برابر 0 خواهد بود، ناگفته نماند که این مقدار این میانگین با توجه به تقسیم تمامی نمونه فرآیند ایجاد شده فرآیند های انتخاب شده بر تعداد روز های در بین بازه انتخاب شد به دست می آیند).

### وضعیت خطاهای سیستم؛ هر کارت چه چیزی را می‌شمارد؟
کارت / شناسه دستهتوضیح راهنمای همان کارتخطا — 1تعداد کل خطاهای رخ‌داده در سیستم. این تعداد ممکن است بیشتر یا کمتر از تعداد نمونه‌ فرآیند ها باشد؛ زیرا هر نمونهٔ فرآیند می‌تواند بدون خطا اجرا شده یا خاتمه یافته باشد، و همچنین هر نمونهٔ فرآیند ممکن است شامل چندین خطا باشد.خطا رخ داده اما مدیریت شده — 2تعداد کل خطاهایی که در سیستم رخ داده‌اند، اما به‌صورت خودکار توسط سازوکار Error Boundary شناسایی و مدیریت شده‌اند.خطا مدیریت نشده — 3تعداد کل خطاهایی که در سیستم رخ داده‌اند و مدیریت نشده‌اند؛ به این معنا که فاقد سازوکار Error Boundary بوده و در نتیجه هیچ‌گونه مدیریت خطایی بر روی آن‌ها اعمال نشده است.خطا دسترسی — 4تعداد کل خطاهای مرتبط با سطح دسترسی. خطای دسترسی می‌تواند شامل انواع عملیات آغاز، ادامه، ادامهٔ فرآیند دیگر با استفاده از Message Boundary Event، Receive Task یا Signal Event، و همچنین آغاز فرآیند دیگر با استفاده از Call Activity Task یا Signal Event باشد.خطا در سرویس خارجی — 5تعداد کل خطاهای ناشی از سرویس‌های خارجی (در نودهای Service Task با منبع دادهٔ WSDL یا REST).خطا در سرویس داخلی — 6تعداد کل خطاهایی که در ابتدای فرآیند رخ داده‌اند. هر نوع شروعی که از مسیری غیر از کارتابل فرآیند انجام شده باشد، به‌عنوان شروع از بیرون تلقی می‌شود. این موارد شامل انواع زیر است: Conditional Start Event، Graph Mobile Start Event، Message Start Event، Signal Start Event و Timer Start Event.خطا در نمونه فرآیند های آغازشده توسط سرویس ESB — 7تعداد کل خطاهای رخ‌داده در نمونه فرآیندهایی که توسط سرویس ESB سامانه آغاز شده‌اند.خطا تایم اوت — 8تعداد کل خطاهای رخ‌داده که در نتیجهٔ وقفهٔ طولانی‌مدت ایجاد شده‌اند.خطا ناشی از گیت وی بدون مسیر پیشفرض — 9تعداد کل خطاهایی که به دلیل عدم وجود مسیر پیش‌فرض در Gateway رخ داده‌اند؛ به این دلیل که با عدم برآورده شدن شروط تمامی مسیرها، نمونه فرآیند با خطا مواجه شده است.خطا در نمونه فرآیند هایی که خاتمه یافته اند — 10تعداد کل خطاهای رخ‌داده شده در نمونه‌ فرآیند هایی که به هر نحوی خاتمه یافته‌اند؛ به این معنا که مشکل به هر روشی رفع شده و فرآیند یا مسیر اصلی خود را ادامه داده یا از مسیر دیگری به پایان رسیده است.خطا ناشی از خروجی نامعتبر متغیر — 11تعداد کل خطاهای رخ‌داده به دلیل نامعتبر بودن داده‌های خروجی سرویس. در این حالت، خروجی از Service Task، Script Task و سایر موارد ارائه نشده و به دلیل عدم تنظیم گزینهٔ «تنظیمات خروجی NULL»، نمونهٔ فرآیند با خطا مواجه شده است.خطا در دریافت مقدار متغیر — 12تعداد کل خطاهای رخ‌داده در زمان دریافت مقدار متغیر. این خطا می‌تواند ناشی از عدم وجود مقدار ثبت‌شده برای متغیر، عدم وجود خود متغیر، یا مسیردهی نادرست مقدار در حالت پیشرفته باشد.خطا مرتبط با تایمر شروع — 13تعداد کل خطاهایی که در شروع «StartTimerEvent» رخ داده اند.خطا مرتبط با تایمر میانی — 14تعداد کل خطاهایی که در میانه تایمر «IntermediateTimerEvent» رخ داده اند.خطا ناشی از عدم وجود مسیر ادامه و یا لغو شدن نمونه فرآیند — 15تعداد کل خطا هایی که به دلیل عدم وجود نودی برای ادامه یا لغو شدن نمونه فرآیند به وجود آمده اند.سایر خطا ها — 16تعداد کل سایر خطاهای رخ‌داده در سیستم. هر نوع خطایی که در بخش‌های دیگر تعریف نشده باشد، در اینجا قرار می‌گیرد.
کلیک روی هر کارت، مدال جستجوی خطاها را با همان دسته انتخاب‌شده باز و جستجو می‌کند. برای تغییر دامنه می‌توان تاریخ، فرآیند، نسخه، المان و سایر ورودی‌ها را اصلاح کرد و دوباره «جستجو» زد. راهنمای ⓘ فقط تعریف کارت را باز می‌کند. دسته «نوع خطا در گزارش» با «نوع خطا»ی فنی مثل FormatException متفاوت است؛ اولی یکی از ۱۶ دسته تحلیلی و دومی نام فنی استثناست.
**معنای فنی دسته‌ها در برابر متن راهنما:**
وجود Error Boundary در SQL ملاک دسته مدیریت‌شده است و به تنهایی موفق‌بودن بازیابی از خطا را ثابت نمی‌کند. «خطا در نمونه خاتمه‌یافته» فقط وضعیت فعلی Finish نمونه را بررسی می‌کند و رفع علت خطا را اثبات نمی‌کند. دسته سرویس داخلی نیز خطاهای نمونه‌ای را که لاگ شروع از سرویس دارد بررسی می‌کند؛ محدود به وقوع خطا دقیقاً در گام آغاز نیست. 

### مدال مشاهده همه خطاها؛ تک‌تک ورودی‌ها
ورودی / ستونشناسه فنیمعنا و روش استفادهنام فرآیندProcessGroupIdانتخاب خانواده فرآیند با جدول شناسه/عنوان. در مدال بازشده از تب گروه، همان گروه از قبل انتخاب و قفل می‌شود.شناسه نمونهCreationIdشماره دقیق اجرای مورد بررسی؛ با شناسه فرآیند تفاوت دارد.ورژن فرآیندProcessIdنسخه اجرایی از گروه انتخاب‌شده؛ لیست نسخه پس از انتخاب گروه آماده می‌شود.المانElementBpmnIdانتخاب گام از جدول شناسه BPMN، نام، شناسه نوع و نام نوع؛ همزمان نوع المان نیز تنظیم می‌شود.نقش ایجادکننده خطاExceptionOwnerRoleId / ExceptionOwnerRoleNameانتخاب نقش ثبت‌شده برای عامل رخداد؛ شناسه و نام نقش برای درخواست آماده می‌شوند.نوع خطاExceptionTypeفهرست انواع خطای فنی دریافت‌شده از GetExceptionsTypes؛ قابل جستجو و پاک‌کردن.عنوان خطاExceptionTitleعبارت مورد جستجو در عنوان. طبق راهنما تمام موارد شامل عبارت ورودی پیدا می‌شوند.متن پیام خطاExceptionMessageعبارت مورد جستجو در متن خطا؛ برای بخشی از متن مانند Gateway یا code کاربرد دارد.کاربر ایجادکننده خطاExceptionOwnerUserIdانتخاب کاربر در رابط وجود دارد. در قرارداد رابط handler جستجوی مدال این مقدار را به API منتقل نمی‌کند؛ اثر آن باید با رفتار نسخه اجراشده کنترل شود.تاریخ وقوع خطا ازExceptionOccurredDateTimeFromحد پایین تاریخ/ساعت رخداد خطا؛ مقدار اولیه از تاریخ بخش وضعیت خطاهاست.تاریخ وقوع خطا تاExceptionOccurredDateTimeToحد بالای تاریخ/ساعت رخداد خطا؛ فیلتر بر زمان وقوع است، نه الزاماً زمان شروع نمونه.خاتمه یافته؟HasCreationFinishedسه حالت: بله، خیر و بدون محدودیت. صرفاً خالی‌کردن را با خیر یکی ندانید.لغو شده؟HasCreationDiscardedسه حالت برای وضعیت لغو نمونه مرتبط با خطا.شناسه نوع المانElementTypeIdفیلد عددی فقط‌خواندنی؛ همراه انتخاب المان تعیین می‌شود.عنوان نمونه فرآیندCreationTitleمحدودکردن نتایج با عنوان اجرای فرآیند.نوع خطا در گزارشExceptionCategory ← TypeIdانتخاب یکی از ۱۶ دسته کارت‌های وضعیت خطا؛ مستقل از ExceptionType.فرآیند والد — پیشرفتهParentProcessGroupIdانتخاب گروه فرآیندی که نمونه از آن آغاز شده است.المان والد — پیشرفتهParentStepBpmnId / ParentStepTypeIdگام آغازکننده در فرآیند والد؛ انتخاب، شناسه BPMN و شناسه نوع را با هم مقداردهی می‌کند.عنوان نمونه والد — پیشرفتهParentCreationTitleجستجو در عنوان اجرای والد، برای ردیابی نمونه فرزند.
**نیازمند تأیید فنی: **«شناسه خطا» در مدل فنی پارامتر ExceptionId دارد، اما در قالب فعلی فرم جستجوی خطا نمایش داده نشده است. شناسه خطا در ستون نتایج موجود است. در نمای یک گروه، حذف فیلتر، قید گروه اصلی را حفظ می‌کند تا مدال از دامنه تب خارج نشود. در قرارداد توصیف‌شده پاک‌سازی دسته خطا/کاربر کامل نیست؛ پس بعد از حذف فیلتر وضعیت ورودی‌ها را نگاه کنید و موارد باقی‌مانده را از × پاک کنید.

### جدول جدیدترین خطاها و ستون‌های کامل آن
ورودی / ستونشناسه فنیمعنا و روش استفادهشناسه گروه فرآیندProcessGroupIdشناسه خانواده فرآیند مرتبط با خطا.نام فرآیندProcessGroupNameعنوان گروه فرآیند.عنوان نمونه فرآیندCreationTitleعنوان اجرای مرتبط؛ متن بلند با کلیک سلول قابل مشاهده است.شناسه فرآیندی المانElementBpmnIdشناسه نود در مدل BPMN.نوع المانElementTypeNameنام نوع نود، مانند UserTask، ScriptTask یا Gateway.عنوان المانElementNameنام قابل‌خواندن گامی که خطا در آن ثبت شده است.عنوان خطاExceptionTitleعنوان کوتاه رخداد.شرح خطاExceptionMessageپیام فنی/متن خطا؛ برای متن بلند روی سلول کلیک کنید.نوع خطاExceptionTypeنوع فنی استثنا.شناسه خطاExceptionIdشناسه یکتای رکورد خطا؛ مرتب‌سازی اولیه نزولی و جدیدترین شناسه نخست است.شناسه نمونهCreationIdکلید اجرای فرآیند برای اتصال به جزئیات و متغیرها.کاربر ایجادکننده خطاExceptionOwnerUserNameکاربر ثبت‌شده به عنوان عامل رخداد.نقش ایجادکننده خطاExceptionOwnerRoleNameنقش کاربر در رخداد.زمان خطاErrorTimeتاریخ و ساعت رخداد به نمایش شمسی.مدت زمان در انتظارExceptionOccurredDurationمدت محاسبه‌شده خروجی، با تبدیل ثانیه به واحد خوانا.عنوان نمونه والدParentCreationTitleعنوان نمونه بالادست.نوع گام والدParentStepElementTypeNameنوع نود آغازکننده در والد.شناسه فرآیندی المان والدParentStepBpmnIdشناسه BPMN گام والد.عنوان گام والدParentStepNameنام گام در فرآیند والد.خاتمه‌یافتهHasCreationFinishedپرچم پایان نمونه مرتبط.لغوشدهHasCreationDiscardedپرچم لغو نمونه مرتبط.عملیاتrow actionsجزئیات خطاها، متغیرها، متغیرهای خروجی و جزئیات ارجاعات.**خواندن زمان خطا و مدت:**
مدت ExceptionOccurredDuration در SQL تعداد ثانیه‌های گذشته از وقوع خطا تا زمان اجرای پرس‌وجو است؛ زمان انتظار یک وظیفه یا زمان لازم برای رفع خطا نیست. اگر داده از کش خوانده شود این سنِ خطا تا دریافت تازه ثابت می‌ماند. ستون زمان در فرانت به ErrorTime متصل است، اما رویه‌های این قرارداد ExceptionOccurredDateTime را برمی‌گردانند؛ در نگاشت مستقیم ممکن است ستون زمان خطا خالی دیده شود. در ریزارجاعات فیلد ExceptionOccurredDateTime مستقیم استفاده شده است.

### آخرین نمونه‌های ایجادشده؛ مشاهده همه و جستجوی پیشرفته
ورودی / ستونشناسه فنیمعنا و روش استفادهعنوان نمونهCreationTitleعبارت عنوان اجرای فرآیند.شناسه نمونهCreationIdشماره دقیق نمونه؛ از لینک بیشترین انتظار نمودار نیز خودکار پر می‌شود.گروه ماژولProcessGroupModuleGroupIdانتخاب دسته/ماژول فرآیند از جدول شناسه و عنوان؛ با گروه فرآیند یکسان نیست.کاربر خاتمه‌دهندهFinisherUserIdکاربری که نمونه را پایان داده است.نام فرآیندProcessGroupIdگروه فرآیند؛ در مدال تب گروه قفل می‌شود.ورژن فرآیندProcessIdنسخه اجرایی مشخص از گروه انتخابی.کاربر شروع‌کنندهStarterUserIdکاربری که اجرا را آغاز کرده است.تاریخ شروع ازStartRangeOfStartDateTimeحد پایین زمان شروع نمونه؛ مقدار اولیه از تاریخ گزارش آخرین نمونه‌ها.تاریخ شروع تاEndRangeOfStartDateTimeحد بالای زمان شروع نمونه.تاریخ پایان ازStartRangeOfEndDateTimeحد پایین زمان پایان؛ مستقل از بازه شروع.تاریخ پایان تاEndRangeOfEndDateTimeحد بالای زمان پایان.خاتمه‌یافتهFinishبله، خیر یا بدون محدودیت؛ برای دیدن موارد جاری وضعیت پایان را در نظر بگیرید.فرآیند فعال است؟IsProcessActiveوضعیت تعریف/نسخه فرآیند، نه وضعیت پایان همان نمونه.متوقف‌شدهIsProcessSuspendedوضعیت توقف تعریف فرآیند؛ سه‌حالته.عنوان نمونه والدParentCreationTitleورودی پیشرفته برای عنوان اجرای والد.نام فرآیند والدParentProcessGroupIdگروه فرآیند بالادست.المان والدParentStepBpmnId / ParentStepTypeIdنود والد که اجرا از آن ایجاد شده؛ انتخاب شناسه و نوع را با هم می‌فرستد.لغوشدهDiscardبله، خیر یا بدون محدودیت در بخش اطلاعات لغو.کاربر لغوکنندهDiscarderUserIdعامل لغو نمونه.توضیح لغوDiscardDescriptionمتن ثبت‌شده برای علت/توضیح لغو.تاریخ لغو ازStartRangeOfDiscardDateحد پایین تاریخ لغو.تاریخ لغو تاEndRangeOfDiscardDateحد بالای تاریخ لغو.
**نیازمند تأیید فنی: **در سورس قرارداد توصیف‌شده، انتهای پاک‌سازی این فرم به متغیری متعلق به فرم خطا ارجاع می‌دهد؛ ممکن است پس از پاک‌کردن سایر فیلدها خطای اسکریپت رخ دهد. بعد از استفاده از «حذف فیلتر»، ورودی‌های باقی‌مانده و نتیجه جستجو را کنترل کنید. 

### ستون‌های فهرست نمونه‌ها
ورودی / ستونشناسه فنیمعنا و روش استفادهشناسه گروه فرآیندProcessGroupIdشناسه خانواده فرآیند.عنوان گروه فرآیند / نام فرآیندProcessGroupNameعنوان گروه؛ در قالب فعلی دو ستون با عنوان‌های متفاوت همین مقدار را نمایش می‌دهند.شناسه گروه / شناسه گروه ماژولProcessGroupModuleGroupIdشناسه دسته ماژول؛ در دو جای جدول تکرار می‌شود.عنوان گروه / گروه ماژولProcessGroupModuleGroupNameنام گروه ماژول؛ ممکن است با دو عنوان ستون دیده شود.ورژن فرآیندProcessVersionشماره نسخه، مستقل از شناسه ProcessId.شناسه نمونهCreationIdشناسه اجرا و مبنای مرتب‌سازی اولیه نزولی.عنوان نمونهCreationTitleعنوان نمایش نمونه؛ HTML عنوان به متن خوانا تبدیل می‌شود.شناسه فرآیندProcessIdشناسه تعریف اجرایی نسخه.عنوان فرآیندProcessTitleستون در پیکربندی وجود دارد اما پیش‌فرض پنهان است.کاربر شروع‌کنندهStarterUserTitleعنوان/نام کاربر آغازکننده.شناسه کاربر شروع‌کنندهStarterUserIdشناسه عددی آغازکننده.زمان شروعStartDateTimeشروع اجرا به تاریخ/ساعت شمسی.در انتظار برای خاتمهDurationمدت اجرا/انتظار خروجی گزارش؛ در رابط به مدت خوانا تبدیل می‌شود.زمان پایانEndDateTimeتاریخ پایان در صورت وجود.کاربر خاتمه‌دهندهFinisherUserTitleنام عامل پایان.فرآیند والدParentProcessNameنام فرآیند بالادست.عنوان نمونه والدParentCreationTitleعنوان اجرای والد؛ برخی برچسب‌های رابط نام فرآیند والد نوشته‌اند، مقدار واقعی عنوان نمونه است.گام والدParentStepTitleعنوان نود والد.نوع گام والدParentStepTypeNameنام نوع نود والد.توضیح لغو فرآیندDiscardDescriptionمتن شرح لغو.فعالIsProcessActiveفعال‌بودن نسخه فرآیند.حذف‌شدهIsProcessDeletedحذف‌شدن تعریف فرآیند.متوقف‌شدهIsProcessSuspendedتوقف تعریف فرآیند.خاتمه‌یافتهFinishپرچم پایان نمونه.شناسه کاربر خاتمه‌دهندهFinisherUserIdشناسه عددی عامل پایان.لغوDiscardپرچم لغو نمونه.کاربر لغوکنندهDiscarderUserTitleنام عامل لغو.شناسه کاربر لغوکنندهDiscarderUserIdشناسه عددی عامل لغو.تاریخ لغو فرآیندDiscardProcessDateتاریخ و ساعت لغو.شناسه نمونه والدParentCreationIdکلید اجرای بالادست.شناسه فرآیند والدParentProcessIdشناسه نسخه بالادست.شناسه گام والدParentStepIdشناسه داخلی گام والد؛ با BpmnId رشته‌ای متفاوت است.شناسه نوع گام والدParentStepTypeIdکد نوع گام بالادست.عملیاتrow actionsجزئیات ارجاعات، متغیرها و متغیرهای خروجی.**دو تفاوت مهم خروجی قرارداد توصیف‌شده:**
**تفاوت واحد مدت در قراردادها: **رویه کلی Duration را به ثانیه و رویه گروهی آن را به روز محاسبه می‌کند، در حالی که هر دو در فرانت به تابع نمایش ثانیه داده می‌شوند. بنابراین ستون «در انتظار برای خاتمه» در تب گروه ممکن است مدت بسیار کوچک‌تر از واقع نشان دهد؛ این ستون تا یکسان‌شدن واحدها مبنای مقایسه گروه با کل سیستم نیست.

ستون فنی ParentProcessName در فرانت وجود دارد، اما خروجی SQL نام ParentProcessGroupName را برمی‌گرداند. در نگاشت مستقیم ممکن است ستون نام فرآیند والد خالی باشد، حتی وقتی شناسه یا عنوان نمونه والد وجود دارد. عنوان نمونه والد در ستون جداگانه ParentCreationTitle خوانده می‌شود.

### ریزگزارش جزئیات ارجاعات یک نمونه
ورودی / ستونشناسه فنیمعنا و روش استفادهعنوان المانElementTitleنام گام اجراشده.نوع المانElementTypeNameنوع BPMN گام.کاربر گیرندهRecieverUserTitleگیرنده ارجاع.نقش گیرندهRecieverRoleNameنقش مربوط به گیرنده.وضعیتReferencesRead۰: در انتظار؛ ۱: خاتمه‌یافته؛ ۲: ادامه‌یافته توسط مرزی؛ ۴: لغو. مقادیر دیگر در این نگاشت متن ندارند.شرحReferencesNoteExplainیادداشت/توضیح ثبت‌شده روی ارجاع.زمان شروعStartDateTimeآغاز ارجاع/اجرای گام.زمان پایانEndDateTimeپایان ارجاع در صورت وجود.مدتDurationثانیه خروجی با نمایش سال/ماه/روز/ساعت/دقیقه/ثانیه.عنوان فرمEntityTitleعنوان موجودیت/فرم مرتبط با گام.حالت فرمEntityModeedit ویرایش، insert افزودن، view نمایش؛ سایر مقادیر همان مقدار خام.ثبت موقتDraftFormStateبله/خیر برای وضعیت ذخیره موقت.نام متغیر خروجیOutputVariableNameنام خروجی گام/ارجاع.مقدار متغیر خروجیOutputVariableValueمقدار همان خروجی.عنوان خطاExceptionTitleعنوان خطای متصل به گام.شرح خطاExceptionMessageمتن خطای مرتبط.نوع خطاExceptionTypeنوع فنی استثنا.زمان خطاExceptionOccurredDateTimeزمان وقوع خطای این جزئیات.کاربر ایجادکننده خطاExceptionOwnerعامل خطا طبق داده خروجی گزارش.نقش ایجادکننده خطاExceptionOwnerRoleNameنقش مرتبط با عامل خطا.
وقتی از جدول خطاها باز شود، ردیف مرتبط با ExceptionId انتخابی با حاشیه قرمز برجسته می‌شود. اگر تطابق پیدا نشود، جدیدترین ردیف بر اساس زمان شروع برای برجسته‌سازی جایگزین می‌شود. پس رنگ قرمز را همیشه «خطای جدید مستقل» تفسیر نکنید؛ این رنگ برای یافتن محل مرتبط با انتخاب فعلی است. در بازکردن از فهرست نمونه‌ها برجسته‌سازی خطا فعال نیست.

### ریزگزارش متغیرها و جزئیات مقدار

دکمه متغیرها (نماد متغیر/ریشه) در عملیات خطا یا نمونه، مدال «متغیرها» را باز می‌کند. این گزارش لاگ مقدارهای ثبت‌شده برای نمونه انتخابی است؛ با فهرست متغیرهای خروجی نهایی متفاوت است. درخواست از ProcessesCreationsIds با شناسه همان ردیف استفاده می‌کند.
ورودی / ستونشناسه فنیمعنا و روش استفادهنام متغیرNameنام داده‌ای که مقدار آن ثبت شده است.مقدار متغیرValueمقدار ثبت‌شده در این ردیف لاگ. اگر JSON باشد با تورفتگی و رنگ‌بندی کلید، رشته، عدد، بولین و null نمایش داده می‌شود.المان منتسبReferenceگامی که مقدار به آن منتسب شده است.کاربر ایجادکنندهCreatorUserNameنام کاربر ثبت‌کننده مقدار.تاریخCreateDateتاریخ و ساعت ثبت مقدار؛ در نبود تاریخ ممکن است ------ نمایش داده شود.شناسه کاربرCreatorUserIdشناسه عددی کاربر ثبت‌کننده.

### ریزگزارش متغیرهای خروجی

دکمه «متغیرهای خروجی» (نماد خروجی فایل) با دو کلید ProcessId و ProcessCreationId اطلاعات خروجی نمونه را دریافت می‌کند. اگر یکی از شناسه‌ها موجود نباشد تابع بازکردن ادامه پیدا نمی‌کند. نتیجه سرویس یک شیء نام→مقدار است که رابط آن را به جدول دو ستونی تبدیل می‌کند.
ورودی / ستونشناسه فنیمعنا و روش استفادهنام متغیرNameنام خروجی تعریف‌شده فرآیند.مقدار متغیرValueمقدار خروجی برای نمونه انتخاب‌شده؛ شیء/آرایه JSON نیز قابل نمایش است.

### مدال جزئیات فنی خطا
بخشورودی / خروجیکاربردنوع خطاExceptionTypeنام فنی استثنا برای تشخیص خانواده خطا.متن خطاExceptionMessageپیام کامل استثنا.ترتیب بروز خطاExceptionOwner → LineNum, MethodName, ClassNameمسیر متدها/کلاس‌ها. شماره خط، نام متد/تابع و نام کلاس برای ارجاع به تیم توسعه کاربرد دارد.پارامترهای ورودی اولین متد/تابعExceptionParameters → Name, Valueنام و مقدار پارامترهایی که در رخداد ثبت شده‌اند. مقدار شیء با JSON چندخطی نمایش داده می‌شود.جزئیات خطای پروایدرProcessErrorProviderDetails یا ProviderDetails → Id, Name, Typeفقط وقتی اطلاعات پروایدر در داده وجود داشته باشد، بخش شناسه/نام/نوع افزوده می‌شود.اطلاعاتپیام «اطلاعاتی برای نمایش وجود ندارد»حالت نبود همه داده‌های قابل استفاده.
**نیازمند تأیید فنی:** در قرارداد رابط، تفاوت ساختار `items/template` با `rows/columns` می‌تواند نمایش نوع و متن خطا را تحت تأثیر قرار دهد. با این حال، نوع و پیام در نمای عملیاتی این مقاله نمایش داده می‌شوند؛ خرابی عمومی این دو بخش قابل تعمیم نیست. در آزمون نسخه مستقر مقدار واقعی ردیف را با بخش بازشونده مقایسه کنید. عنوان فارسی «خطی» به `LineNum` اشاره دارد.

### گزارش افزوده اول در تب گروه: گام‌ها

> **متن راهنمای سامانه:** اطلاعات آماری گام های اجرا شده.
ورودی / ستونشناسه فنیمعنا و روش استفادهتاریخ از / تاStaticalDetailFromDateTime / StaticalDetailToDateTimeبازه مشترک دو گزارش آماری؛ تغییر آن هر دو را بارگذاری می‌کند.انتخاب نسخه فرآیندProcessIdانتخاب نسخه خاص؛ پاک‌کردن یعنی محدودنکردن به یک نسخه در گروه فعلی. نمایش با شماره Version و درخواست با Id است.انتخاب نوع گامStepTypeIdنوع المان از فهرست انواع BPMN؛ مقدار اولیه UserTask با کد ۱. × همه انواع را در محدوده گروه/نسخه پوشش می‌دهد.بر اساس رونوشت‌هاIncludeCopiesبرای UserTask یا انتخاب همه انواع کاربرد دارد؛ روشن، ارجاعات رونوشت را انتخاب می‌کند. انتخاب نوع غیر UserTask مقدار را خاموش می‌کند.ورودی / ستونشناسه فنیمعنا و روش استفادهنام گامStepNameعنوان گام در مدل.شناسه گامStepBpmnIdشناسه BPMN نود.شناسه laneStepLaneBpmnIdشناسه BPMN ناحیه مسئولیت.نام laneStepLaneNameعنوان ناحیه مسئولیت.نوع گامStepTypeنام نوع المان.تعداد نمونهCountشمار موارد مطابق شرایط گزارش گام؛ برای تعبیر دقیق واحد، تعریف SQL مربوط به نوع گام را ببینید.کمترین زمان اجراMinimumDurationکمترین مدت خروجی در ثانیه، تبدیل‌شده به مدت خوانا.میانگین زمان اجراAverageDurationمیانگین مدت خروجی همان مجموعه.بیشترین زمان اجراMaximumDurationبیشترین مدت همان مجموعه.

### گزارش افزوده دوم در تب گروه: خطاهای گام‌ها

> **متن راهنمای سامانه:** اطلاعات آماری گام های با خطا مواجه شده.
ورودی / ستونشناسه فنیمعنا و روش استفادهتاریخ از / تاStaticalDetailFromDateTime / StaticalDetailToDateTimeاز کنترل مشترک جزییات آماری خوانده می‌شود.انتخاب نسخه فرآیندProcessIdمحدودکردن خطاهای آماری به نسخه‌ای مشخص؛ مستقل از نسخه جدول گام‌ها.انتخاب نوع گامStepTypeIdنوع نود مورد بررسی؛ نام متغیر داخلی ExeptionTypeId است ولی پارامتر ارسالی StepTypeId است. این کنترل نوع استثنا مثل FormatException نیست.نام گامStepNameعنوان گام دارای خطا.شناسه گامStepBpmnIdشناسه BPMN برای تشخیص دقیق نود.نوع گامStepTypeنوع نود دارای خطا.تعداد نمونهCountشمار خروجی تجمیعی برای گام مطابق منطق GetStepsExceptionsByProcessGroupId.**واحد شمارش خطاهای گام:**در رویه این قرارداد، لاگ‌های خطا به آخرین ارجاع انتخاب‌شده نمونه منتسب و شمارش می‌شوند. عدد Count را صرفاً تعداد نمونه یکتای معیوب در آن گام یا محل قطعی پیدایش تمام خطاها فرض نکنید؛ الگوریتم انتساب و شمارش در پیوست SQL تشریح شده است.

### ابزارهای مشترک جدول‌ها و پنجره‌های تو در تو
ابزاررفتار و نکته خواندنسرستونکلیک عنوان برای مرتب‌سازی؛ ردیف‌های خطا و نمونه در شروع بر اساس شناسه نزولی مرتب‌اند.ردیف جستجو زیر عنوانفیلتر محلی ستون؛ برای محدودکردن داده‌های دریافت‌شده. قیف کنار سرستون فهرست مقدارهای قابل انتخاب را باز می‌کند.نوار «ستون را برای گروه‌بندی به اینجا بکشید»کشیدن عنوان ستون، نتیجه را بر اساس همان ستون گروه‌بندی می‌کند؛ گزارش بک‌اند یا معیار شمارش SQL را عوض نمی‌کند.اسکرول افقیبرای ستون‌های پنهان‌شده در عرض صفحه؛ ستون‌های شناسه/عنوان یا عملیات ممکن است ثابت بمانند.صفحه‌بندیاندازه‌های ۵، ۱۰، ۲۰ و ۵۰؛ اندازه اولیه ۱۰. «صفحه … از … / تعداد کل» تعداد نتیجه جدول فعلی را بیان می‌کند.فیلتر فرم در برابر فیلتر جدولفرم جستجو پارامترهای دریافت داده را تعیین می‌کند؛ فیلتر/مرتب‌سازی جدول عمدتاً روی داده دریافتی در مرورگر انجام می‌شود.جستجوی سراسری جدولدر مدال ارجاعات/متغیرها/خروجی وجود دارد و متن را میان ستون‌های نتیجه جستجو می‌کند.خروجی اکسلدر مدال ارجاعات، متغیرها و متغیرهای خروجی فعال است؛ فعال‌بودن آن را به همه جدول‌های صفحه تعمیم ندهید.کلیک سلولبرای خواندن متن یا JSON بلند پنجره کوچکتر باز می‌شود. روی سلول خالی پنجره‌ای باز نمی‌شود.مدال روی مدالممکن است فرم مشاهده همه، مدال جزئیات و سپس پنجره مقدار سلول روی هم باز شوند. برای برگشت، فقط آخرین پنجره بازشده را ببندید.مدت‌زمان‌هاورودی عددی ثانیه است. رابط عدد منفی را صفر و اعشار را حذف می‌کند؛ ماه ۳۰ روز و سال ۳۶۵ روز برای نمایش مدت در نظر گرفته می‌شود. این، تبدیل مدت است نه تفاضل تقویمی ماه شمسی.داده خالی / بارگذاریچرخنده یا مقدار null نشانه بارگذاری است. جدول بدون ردیف ممکن است حاصل فیلتر بسیار محدود باشد؛ ابتدا زمان، گروه، نسخه و نوع گام را کنترل کنید.

### کش مرورگر، پارامترهای گزارش و دکمه به‌روزرسانی

برای سریع‌شدن جابه‌جایی و تکرار گزارش، پاسخ‌های گزارش‌های اصلی در مرورگر کش می‌شوند. وقتی همان نشانی سرویس با همان پارامترها دوباره لازم شود، ابتدا از حافظه و سپس از ذخیره‌سازی IndexedDB خوانده می‌شود. با تغییر تاریخ، گروه، نسخه یا هر پارامتر مؤثر، ترکیب جدید درخواست می‌شود؛ اگر همان ترکیب جدید قبلاً در کش معتبر باشد می‌تواند از همان‌جا بازیابی شود. دکمه رفرش تب فعال، مسیر خواندن تازه از بک‌اند را با hardReload فعال می‌کند و پاسخ جدید دوباره در کش نوشته می‌شود.
موضوعرفتار دقیق قرارداد توصیف‌شدهکلید دادهنشانی endpoint + JSON پارامترها + tabId اختیاری؛ بنابراین دو فیلتر متفاوت دو کلید متفاوت‌اند.حافظه سریعMap داخل صفحه؛ حداکثر ۶۰ ورودی، حذف قدیمی‌ترین مورد از نظر استفاده هنگام عبور از سقف.ذخیره‌سازی پایدارIndexedDB با نام BamDB، نسخه ۱، object store برابر Bam_cache؛ حداکثر هدف ۱۰۰ ورودی.مدت اعتبارTTL پیش‌فرض ۱۰ دقیقه؛ علاوه بر انقضای هر ورودی، تایمر صفحه هر ۱۰ دقیقه کل کش داده را invalidateAll می‌کند.تغییر پارامترکلید جدید می‌سازد؛ لزوماً رکورد پارامتر قدیمی را حذف نمی‌کند. بازگشت به پارامتر قبلی ممکن است کش معتبر قبلی را بازیابی کند.رفرشhardReload کش را دور می‌زند و پاسخ تازه همان کلید را جایگزین می‌کند؛ آیکون بارگذاری تا پایان مجموعه درخواست‌ها یا سقف ایمنی ۳۰ ثانیه فعال می‌ماند.درخواست همزمان تکراریدر حالت معمول درخواست‌های همزمان با کلید برابر یکی می‌شوند؛ مصرف‌کنندگان از نتیجه مشترک نسخه مستقل می‌گیرند.خطای ذخیره‌سازیخطای IndexedDB مانع مسیر اصلی دریافت داده نمی‌شود.تنظیمات شخصیlocalStorage با کلید bam_settings تاریخ‌های گزارش، تب‌ها و دامنه را نگه می‌دارد. این با کش داده گزارش در IndexedDB متفاوت است.استثناهاجزئیات ارجاعات، لاگ متغیرها، متغیرهای خروجی و درخواست دریل‌داون روزانه نمودار با Ajax مستقیم فراخوانی می‌شوند و از CachedAjax عبور نمی‌کنند.پارامترهای صرفاً نمایشینوع نمودار، انتخاب سری و فیلتر ستونی همیشه به معنی درخواست دوباره بک‌اند نیستند؛ بسیاری از آن‌ها فقط نمایش داده موجود را عوض می‌کنند.
بنابراین عبارت ساده «پس از یک‌بار دریافت، داده تا تغییر پارامتر یا رفرش از فرانت خوانده می‌شود» برای راهبری مفید است، ولی در این نسخه مطلق نیست: کش زمان انقضا و تخلیه دوره‌ای دارد، بازگشت به پارامتر قبلی ممکن است بدون درخواست تازه انجام شود، و چند ریزگزارش اصلاً در این کش نیستند. اگر داده عملیاتی تازه تولید شده و عدد صفحه هنوز قبلی است، از رفرش تب فعال استفاده و پایان بارگذاری را کنترل کنید.

### سناریوهای عملی خواندن گزارش
هدفمسیر پیشنهادیتصویر کلی از یک بازهتب عملکرد کلی سیستم ← فیلتر تاریخ صفحه ← بررسی شش کارت ← نمودار خلاصه عملکرد و پراکندگی. بازه همه بخش‌ها را از برچسب تاریخ همان بخش کنترل کنید.یافتن گلوگاه انتظارخلاصه عملکرد ← انتخاب فقط حداکثر زمان انتظار ← راهنمای نقطه بیشینه ← کلیک نام فرآیند ← نمونه مربوط ← جزئیات ارجاعات.بررسی خطای یک گاموضعیت خطاها ← کارت دسته مربوط یا مشاهده همه ← گروه/نسخه/المان ← جستجو ← جزئیات خطا ← ارجاعات و متغیرها.بررسی نمونه مشخصآخرین نمونه‌ها ← مشاهده همه ← شناسه نمونه ← جستجو. اگر نتیجه خالی بود تاریخ شروع و قیود دیگر را کنترل کنید.مقایسه نسخه‌های dqwdwqdانتخاب گروه 24273 ← تب جدید ← پراکندگی نسخه‌ها ← تیک نسخه‌های موردنظر ← جدول گام‌ها و خطاها با نسخه و تاریخ یکسان.مقایسه نمونه با ارجاعخلاصه عملکرد ← خاموش‌کردن «بر اساس تعداد نمونه» ← مقایسه کارتابل/وظایف/خطاها؛ جاری/خاتمه/لغو از این گزینه اثر نمی‌گیرند.بررسی رونوشتخلاصه عملکرد یا جدول گام‌ها ← روشن‌کردن «بر اساس رونوشت‌ها» ← خواندن عنوان و واحد شمارش. این انتخاب فقط رونوشت‌هاست، جمع ارجاع اصلی و رونوشت نیست.خواندن JSON کاملعملیات نمونه ← متغیرها یا متغیرهای خروجی ← کلیک سلول مقدار ← پنجره مقدار با اسکرول و رنگ‌بندی.

### مرجع متن همه راهنماهای صفحه

جدول زیر متن فارسی راهنماها و کلید ترجمه متناظر را برای کنترل اتصال راهنما به کنترل و محلی‌سازی ارائه می‌کند.
محل / شناسه راهنمامتن فارسی منبع ترجمهکلید ترجمهinclude_StartDateEndDate_helpبا تغییر بازهٔ زمانی موردنظر از این بخش، می‌توانید بازهٔ انتخاب‌شده را بر روی تمامی فیلترهای بازهٔ زمانی موجود در این صفحه اعمال کنید.ChangingFilterAffectsAllHistoryFiltersinfo-card-tooltip-textتعداد نمونه فرآیندهای لغو نشده و به اتمام نرسیده اند.ProcessBAM_NewCreations__Helpwarning-card-tooltip-textتعداد نمونه فرآیندهایی که در کارتابل / رونوشت افراد قرار دارند.ProcessBAM_InCartableCreations__Helperror-card-tooltip-textتعداد نمونه فرآیند هایی که به هر نحوی با خطا مواجه شده اند (ممکن هست حل شده باشد یا نشده باشد).SampleHasError_helpsuccess-card-tooltip-textتعداد نمونه فرآیندهایی که به هر نحوی خاتمه یافته اند.ProcessBAM_FinishedCreations__Helpdiscard-card-tooltip-textتعداد نمونه فرآیندهایی که لغو شده‌اند.ProcessBAM_DiscardedCreations__HelpbasedOnCreationDate_help-textبا فعال بودن این گزینه، تاریخ ایجاد نمونه مبنای محاسبه همه شاخص‌هاست. با غیرفعال کردن آن، «جاری» همچنان بر اساس تاریخ ایجاد نمونه، «خاتمه‌یافته» بر اساس تاریخ خاتمه و «لغوشده» بر اساس تاریخ لغو محاسبه می‌شوند؛ سایر شاخص‌ها نیز بر اساس تاریخ وقوع رویداد مربوطه محاسبه خواهند شد.BasedOnCreationDate_helpbasedOnCreationCount_help-textبه‌صورت پیش‌فرض، شاخص‌ها بر اساس تعداد نمونه‌های فرآیند محاسبه می‌شوند. با غیرفعال کردن این گزینه، «در کارتابل» و «وظایف ثبت‌شده» بر اساس تعداد ارجاعات و شاخص‌های خطا بر اساس تعداد رکوردهای خطا محاسبه می‌شوند. این گزینه روی «جاری»، «خاتمه‌یافته» و «لغوشده» تأثیری ندارد.BasedOnCreationCount_helpShowTranscript_help-textبا فعال بودن این گزینه، شاخص‌های «در کارتابل»، «وظایف ثبت‌شده» و زمان ثبت وظایف فقط بر اساس ارجاعات رونوشت محاسبه می‌شوند. با غیرفعال بودن آن، فقط ارجاعات اصلی در محاسبات در نظر گرفته می‌شوند.ShowTranscript_helpstep-analysis-card-tooltip-textاطلاعات آماری گام های اجرا شده.Step_Analysis__Helpexeption-analysis-card-tooltip-textاطلاعات آماری گام های با خطا مواجه شده.Exeption_Analysis__Helpexc_count_helpتعداد کل خطاهای رخ‌داده در سیستم. این تعداد ممکن است بیشتر یا کمتر از تعداد نمونه‌ فرآیند ها باشد؛ زیرا هر نمونهٔ فرآیند می‌تواند بدون خطا اجرا شده یا خاتمه یافته باشد، و همچنین هر نمونهٔ فرآیند ممکن است شامل چندین خطا باشد.ProcessBAM_OccurredExceptionCreations__Helpauto_exc_helpتعداد کل خطاهایی که در سیستم رخ داده‌اند، اما به‌صورت خودکار توسط سازوکار Error Boundary شناسایی و مدیریت شده‌اند.ProcessBAM_AutoHandledOccurredExceptionCreations__Helpnot-auto_exc_helpتعداد کل خطاهایی که در سیستم رخ داده‌اند و مدیریت نشده‌اند؛ به این معنا که فاقد سازوکار Error Boundary بوده و در نتیجه هیچ‌گونه مدیریت خطایی بر روی آن‌ها اعمال نشده است.ProcessBAM_NotAutoHandledOccurredExceptionCreations__Helpunknown_exc_count_helpتعداد کل سایر خطاهای رخ‌داده در سیستم. هر نوع خطایی که در بخش‌های دیگر تعریف نشده باشد، در اینجا قرار می‌گیرد.OtherErrors__helpext_exc_count_helpتعداد کل خطاهای ناشی از سرویس‌های خارجی (در نودهای Service Task با منبع دادهٔ WSDL یا REST).ProcessBAM_ExternalOccurredExceptionCreations__Helpsrv_exc_count_helpتعداد کل خطاهایی که در ابتدای فرآیند رخ داده‌اند. هر نوع شروعی که از مسیری غیر از کارتابل فرآیند انجام شده باشد، به‌عنوان شروع از بیرون تلقی می‌شود. این موارد شامل انواع زیر است: Conditional Start Event، Graph Mobile Start Event، Message Start Event، Signal Start Event و Timer Start Event.ProcessBAM_StartedAsServiceOccurredExceptionCreations__Helpesb_exc_count_helpتعداد کل خطاهای رخ‌داده در نمونه فرآیندهایی که توسط سرویس ESB سامانه آغاز شده‌اند.ExampleErrorInProcessServiceESB__helpgw_exc_helpتعداد کل خطاهایی که به دلیل عدم وجود مسیر پیش‌فرض در Gateway رخ داده‌اند؛ به این دلیل که با عدم برآورده شدن شروط تمامی مسیرها، نمونه فرآیند با خطا مواجه شده است.ProcessBAM_GatewayHasNoDefaultSequenceFlowOccurredExceptionCreations__Helpav_exc_count_helpتعداد کل خطاهای مرتبط با سطح دسترسی. خطای دسترسی می‌تواند شامل انواع عملیات آغاز، ادامه، ادامهٔ فرآیند دیگر با استفاده از Message Boundary Event، Receive Task یا Signal Event، و همچنین آغاز فرآیند دیگر با استفاده از Call Activity Task یا Signal Event باشد.ProcessBAM_AccessViolationOccurredExceptionCreations__Helpstart_proc_exc_help_1تعداد کل خطاهایی که در شروع «StartTimerEvent» رخ داده اند.ProcessBAM_AnExceptionOccurredDuringStartingProcessCreations__Helpto_exc_helpتعداد کل خطاهای رخ‌داده که در نتیجهٔ وقفهٔ طولانی‌مدت ایجاد شده‌اند.ProcessBAM_TimeoutOccurredExceptionCreations__Helpexc_finished_helpتعداد کل خطاهای رخ‌داده شده در نمونه‌ فرآیند هایی که به هر نحوی خاتمه یافته‌اند؛ به این معنا که مشکل به هر روشی رفع شده و فرآیند یا مسیر اصلی خود را ادامه داده یا از مسیر دیگری به پایان رسیده است.ProcessBAM_OccurredExceptionFinished__Helpinv_out_exc_helpتعداد کل خطاهای رخ‌داده به دلیل نامعتبر بودن داده‌های خروجی سرویس. در این حالت، خروجی از Service Task، Script Task و سایر موارد ارائه نشده و به دلیل عدم تنظیم گزینهٔ «تنظیمات خروجی NULL»، نمونهٔ فرآیند با خطا مواجه شده است.ProcessBAM_InvalidOutputServiceDataOccurredExceptionCreations__Helpstart_proc_exc_helpتعداد کل خطاهایی که در میانه تایمر «IntermediateTimerEvent» رخ داده اند.ProcessBAM_AnExceptionOccurredDuringStartingOrContinuingProcessCreations__Helpno_route_exc_helpتعداد کل خطا هایی که به دلیل عدم وجود نودی برای ادامه یا لغو شدن نمونه فرآیند به وجود آمده اند.ProcessBAM_ThereIsNoRouteToContinueProcessCreationOrProcessCreationHasBeenDiscardedOccurredExceptionCreations__Helpvar_value_exc_helpتعداد کل خطاهای رخ‌داده در زمان دریافت مقدار متغیر. این خطا می‌تواند ناشی از عدم وجود مقدار ثبت‌شده برای متغیر، عدم وجود خود متغیر، یا مسیردهی نادرست مقدار در حالت پیشرفته باشد.ProcessBAM_ErrorInGetVariableValueOccurredExceptionCreations__Helpinclude_exceptionTitle_helpتمام نتیجه هایی که شامل مقدار وارد شده باشد، پیدا میشوند.AllResultsContainEnteredValueWillBefoundaverage-samples-tooltip-textتعداد میانگین نمونه فرآیند هایی که به صورت روزانه اجرا میشوند(در صورتی که تعداد میانگین کمتر از 1 باشد، بنابراین میانگین روزانه برابر 0 خواهد بود، ناگفته نماند که این مقدار این میانگین با توجه به تقسیم تمامی نمونه فرآیند ایجاد شده فرآیند های انتخاب شده بر تعداد روز های در بین بازه انتخاب شد به دست می آیند).ScatterChartGuideراهنمای سری نمودارتعداد یوزرتسک هایی که به افراد ارجاع داده شده و در انتظار ثبت هستند.SampleCartable_Helpراهنمای سری نمودارتعداد یوزرتسک هایی که به افراد ارجاع داده شده و اقدام شده استSubmitedTaskCount_Helpراهنمای سری نمودارتعداد خطاهایی که در سیستم رخ داده‌اند، اما به‌صورت خودکار توسط سازوکار Error Boundary شناسایی و مدیریت شده‌اند.ExceptionAutoHandledByErrorBoundaryCreationsCount_Helpراهنمای سری نمودارتعداد خطاهایی که در سیستم رخ داده‌اند و مدیریت نشده‌اند؛ به این معنا که فاقد سازوکار Error Boundary بوده و در نتیجه هیچ‌گونه مدیریت خطایی بر روی آن‌ها اعمال نشده است.ExceptionNotAutoHandledByErrorBoundaryCreationsCount_Helpراهنمای سری نمودارحداکثر زمانی که از ارجاع تسک های جاری در کارتابل میگذرد.MaximumTaskSubmissionDuration_Help
