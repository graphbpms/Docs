---
id: 1477
graphDocId: 1505
title: "ProviderLimitations و تفاوت Create/Edit با Run"
subTitle: null
module: "04-provider"
wikiPath: "1450/1474/1477"
wikiUrl: "https://wiki.graphbpms.ir/1450/1474/1477"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1474/1477"
breadcrumbs: ["مدیریت منابع داده Provider","امنیت، دسترسی و عملیات","ProviderLimitations و تفاوت Create/Edit با Run"]
parentId: 1474
parentTitle: "امنیت، دسترسی و عملیات"
url: "1477"
priority: 3
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۵. امنیت، دسترسی و عملیات. امنیت، دسترسی و عملیات. GraphBPMS • Provider ProviderLimitations و تفاوت Create/Edit با Run در این آموزش 60. کنترل دسترسی و ProviderLimitations 60. کنترل دسترسی و ProviderLimitations کنترل‌هایی که در این بخش توضیح داده می‌شوند مربوط به ایجاد و ویرایش Provider و تحلیل Query آن هستند. این کنترل‌ها مجوز اجرای Provider نیستند. 60.1. محورهای اصلی: 60. کنترل دسترسی و ProviderLimitations؛ 60.1. مرز مهم: Create/Edit با Run متفاوت است؛ 60.2. محدودیت دسترسی به جدول Form و SubForm؛ 60.3. لایه System Settings — کلید ProviderLimitations؛ Operationهای مشاهده‌شده در Limiter جاری؛ رفتار Parser و JSON نامعتبر؛ استثنای راهبر و Admin سازمان مرکزی."
updatedAt: "2026-10-18T16:08:51"
---

# ProviderLimitations و تفاوت Create/Edit با Run

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `امنیت، دسترسی و عملیات` ❯ `ProviderLimitations و تفاوت Create/Edit با Run`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1474/1477](https://wiki.graphbpms.ir/1450/1474/1477)

در این آموزش

-   [60\. کنترل دسترسی و ProviderLimitations](#wiki-provider-limitations-section-01)

## 60\. کنترل دسترسی و ProviderLimitations

کنترل‌هایی که در این بخش توضیح داده می‌شوند مربوط به **ایجاد و ویرایش Provider و تحلیل Query آن** هستند. این کنترل‌ها مجوز اجرای Provider نیستند.

### 60.1. مرز مهم: Create/Edit با Run متفاوت است

در وضعیت جاری:

```text
ایجاد / ویرایش Provider
→ Query Analyzer + Permission/Limitation Checks

اجرای Provider موجود
→ این ProviderLimitations و Permission Checkها دوباره به‌عنوان مجوز اجرای Provider اعمال نمی‌شوند
```

بنابراین «اجازه ساخت یا ویرایش Query» را با «اجازه اجرای Provider ذخیره‌شده» یکسان ندانید.

همچنین در UI/Flow جاری عملیات عمومی «حذف Provider» وجود ندارد؛ بحث این فصل درباره ایجاد و ویرایش است.

### 60.2. محدودیت دسترسی به جدول Form و SubForm

برای Queryهایی که به بانک اطلاعاتی اصلی GraphBPMS متصل‌اند، **در حالت کاربر غیرراهبر** دسترسی SQL به جدول‌های ساخته‌شده توسط Form Builder با مجوزهای فرم مرتبط می‌شود.

برای مثال، برای دسترسی کامل خواندن جدول یک Form و جدول‌های SubForm آن، کاربر باید مجوز:

```text
مشاهده لیست رکورد ها
```

را برای همان Form داشته باشد.

لایه C# از `Auth_SystemPermissions` محدودیت‌های SQL را به Operationهای Query Analyzer تبدیل می‌کند:

| Operation تحلیل‌شده | مفهوم SQL |
| --- | --- |
| `SqlSelectStatement` | خواندن / `SELECT` |
| `SqlInsertStatement` | درج / `INSERT` |
| `SqlUpdateStatement` | ویرایش / `UPDATE` |
| `SqlDeleteStatement` | حذف داده / `DELETE` |

دسترسی‌های Role Group نیز در محاسبه مجوز نقش لحاظ می‌شوند.

نکته مهم درباره SubForm: کد جاری `FormXml` را برای `SubFormSetting` بررسی می‌کند و جدول‌های SubForm را نیز وارد همان مدل محدودیت می‌کند. بنابراین مجوز Form فقط به Table اصلی محدود نیست و SubFormهای آن نیز در تحلیل دسترسی وارد می‌شوند.

### 60.3. لایه System Settings — کلید `ProviderLimitations`

در تنظیمات سیستم GraphBPMS کلیدی با نام:

```text
ProviderLimitations
```

وجود دارد که مقدار آن می‌تواند JSON Array از Ruleهای محدودکننده باشد.

نمونه:

```json
[
  {
    "UserId": 1,
    "RoleId": 1,
    "OrgId": 1,
    "ObjectName": null,
    "ObjectType": null,
    "Operations": [
      "SqlVariableDeclareStatement"
    ]
  },
  {
    "UserId": 1,
    "RoleId": null,
    "OrgId": 1,
    "ObjectName": "ProcessCreation",
    "ObjectType": 0,
    "Operations": [
      "SqlSelectStatement",
      "SqlDeleteStatement"
    ]
  },
  {
    "UserId": 1,
    "RoleId": null,
    "OrgId": null,
    "ObjectName": "InfoFunc",
    "ObjectType": 1,
    "Operations": [
      "SqlSelectStatement"
    ]
  },
  {
    "UserId": null,
    "RoleId": null,
    "OrgId": null,
    "ObjectName": "ArchiveFileDelete",
    "ObjectType": 2,
    "Operations": [
      "SqlInsertStatement",
      "SqlSelectStatement"
    ]
  }
]
```

هر Rule سه بخش مفهومی دارد:

| بخش | کاربرد |
| --- | --- |
| `UserId` / `RoleId` / `OrgId` | تعیین Scope شخص/نقش/سازمان |
| `ObjectName` / `ObjectType` | تعیین Object SQL مورد محدودیت |
| `Operations` | تعیین نوع عملیات SQL/Analyzer که ممنوع است |

ترکیب Scopeها در کد جاری شامل حالت‌های زیر است:

| UserId | RoleId | OrgId | دامنه |
| --- | --- | --- | --- |
| مقدار | مقدار | مقدار | شخص + نقش + سازمان |
| مقدار | مقدار | `null` | شخص + نقش |
| مقدار | `null` | مقدار | شخص + سازمان |
| مقدار | `null` | `null` | شخص |
| `null` | مقدار | مقدار | نقش + سازمان |
| `null` | مقدار | `null` | نقش |
| `null` | `null` | مقدار | سازمان |
| `null` | `null` | `null` | همه |

اگر `ObjectName` و `ObjectType` خالی باشند، Rule می‌تواند صرفاً یک Operation تحلیل‌شده را در Scope مورد نظر محدود کند؛ نمونه روشن آن:

```json
{
  "ObjectName": null,
  "ObjectType": null,
  "Operations": [
    "SqlVariableDeclareStatement"
  ]
}
```

است که برای محدود کردن Statement از نوع `DECLARE` استفاده می‌شود.

مطابق نمونه تنظیم جاری، مقادیر `ObjectType` به این شکل استفاده شده‌اند:

| مقدار نمونه | نوع Object |
| --- | --- |
| `0` | Table |
| `1` | Table-Valued Function |
| `2` | Stored Procedure |

`[وابسته به نسخه/استقرار]` اگر Enum داخلی `ObjectTypes` در Build دیگری تغییر کند، مقدار عددی باید با همان Build تطبیق داده شود؛ نام مفهومی Object معیار اصلی طراحی Rule است.

### Operationهای مشاهده‌شده در Limiter جاری

| Operation | کاربرد |
| --- | --- |
| `SqlVariableDeclareStatement` | تعریف Variable با `DECLARE` |
| `SqlSelectStatement` | `SELECT` |
| `SqlInsertStatement` | `INSERT` |
| `SqlUpdateStatement` | `UPDATE` |
| `SqlDeleteStatement` | `DELETE` |
| `SqlExecuteModuleStatement` | اجرای Module/Procedure در Query Analyzer |
| `SqlObjectReference` | Reference به Object در Query Analyzer |
| `SqlNullInsertSource` | نوع تحلیل داخلی Insert Source |
| `SqlTableValuedFunctionRefExpression` | Reference Expression به Table-Valued Function |

این جدول فقط Operationهایی را شامل می‌شود که در C# ارسالی مشاهده شده‌اند؛ Operation جدیدی خارج از Query Analyzer جاری به GraphBPMS نسبت داده نشده است.

### رفتار Parser و JSON نامعتبر

C# فقط زمانی تلاش می‌کند Setting را به `List<ProviderLimitation>` تبدیل کند که **اولین کاراکتر مقدار `[` باشد**. بنابراین مقدار را بدون Space/Line Break قبل از `[` ذخیره کنید.

اگر مقدار با `[` شروع شود ولی JSON معتبر نباشد، C# خطا را Log می‌کند و برای این لایه یک List خالی برمی‌گرداند. بنابراین JSON این Setting باید قبل از Production حتماً Validate شود. این خطا WebConfig Limitation و محدودیت دسترسی Form/SubForm را حذف نمی‌کند؛ فقط Ruleهای همین System Setting بارگذاری نمی‌شوند.

### استثنای راهبر و Admin سازمان مرکزی

در کد جاری، Ruleهای **System Configuration ProviderLimitations** برای `Rahbar` و همچنین Admin سازمان مرکزی Skip می‌شوند. این استثنا به معنی غیرفعال شدن WebConfig Limitation نیست.

### 60.4. لایه سخت‌گیرانه WebConfig — `ProviderLimitations`

یک لایه سخت‌گیرانه مستقل با همان نام کلید، در `appSettings` فایل `web.config` تعریف می‌شود:

```xml
<add key="ProviderLimitations" value="*" />
```

مقدار پیش‌فرض `*` یعنی در **این لایه WebConfig** محدودیت اضافه‌ای تعریف نشده است.

این لایه بالاترین اجبار را دارد؛ اگر Query با Rule این لایه Match شود، Ruleهای پایین‌تر نمی‌توانند آن را Allow کنند.

به بیان اجرایی، عبور از System Settings یا Permissionهای Form/SubForm نمی‌تواند Rule منع‌کننده `web.config` را Override کند.

### حالت `MaximumLimitation`

```xml
<add
    key="ProviderLimitations"
    value="MaximumLimitation"
/>
```

در کد جاری، `MaximumLimitation` همه Objectهای `BpmsDb.Objects` با `Owner == "Infrastructure"` را برای Typeهای پشتیبانی‌شده وارد Deny List می‌کند.

Typeهای Map‌شده در کد:

```text
View / Table
→ ObjectTypes.Table

StoredProcedure
→ ObjectTypes.StoredProcedure

TableValuedFunction / InlineTableValuedFunction
→ ObjectTypes.TableValuedFunction
```

برای این Objectها Operationهای زیر به Limitation اضافه می‌شوند:

```text
SqlSelectStatement
SqlInsertStatement
SqlDeleteStatement
SqlUpdateStatement
```

نکته دقیق C#: Typeهای دیگری مانند `ScalarFunction` در Branch `default` رد می‌شوند و توسط همین `MaximumLimitation` به List اضافه نمی‌شوند. بنابراین مستند دقیق‌تر از عبارت کلی «تمام Objectها» این است که Maximum Limitation تمام **Infrastructure Objectهای Map‌شده در کد بالا** را محدود می‌کند.

اگر Token `MaximumLimitation` در Config وجود داشته باشد، کد پس از ساخت این List بلافاصله `return` می‌کند؛ در نتیجه Ruleهای Custom دیگر همان مقدار Config پردازش نمی‌شوند.

### محدود کردن یک Operation در همه Objectها

```xml
<add
    key="ProviderLimitations"
    value="Operation_SqlVariableDeclareStatement"
/>
```

فرمت:

```text
Operation_<AnalyzerOperation>
```

نمونه دیگر:

```text
Operation_SqlDeleteStatement
```

### محدود کردن Table

فرم‌های پشتیبانی‌شده در Parser جاری:

```text
Table_dbo.<TableName>
Table_<ObjectName>
```

در فرم عمومی `Table_<ObjectName>`، `ObjectName` می‌تواند همان نامی باشد که Query Analyzer برای Object تشخیص می‌دهد؛ در سناریوهای Schema سفارشی می‌توان نام Schema-qualified مانند `MySchema.MyTable` را در همین بخش قرار داد. برای `dbo` یک Prefix اختصاصی `Table_dbo.` در Parser وجود دارد.

نمونه:

```xml
<add
    key="ProviderLimitations"
    value="Table_dbo.Users"
/>
```

اگر بخش Operation مشخص نشود، برای Table این چهار Operation محدود می‌شوند:

```text
SqlSelectStatement
SqlInsertStatement
SqlDeleteStatement
SqlUpdateStatement
```

برای محدود کردن فقط Operationهای مشخص:

```xml
<add
    key="ProviderLimitations"
    value="Table_dbo.Users(SqlSelectStatement|SqlDeleteStatement)"
/>
```

فرمت:

```text
Table_<ObjectName>(Operation1|Operation2|...)
```

در بخش داخل پرانتز جداکننده Operationها `|` است.

### محدود کردن Stored Procedure

فرم‌های پشتیبانی‌شده:

```text
StoredProcedure_dbo.<ProcedureName>
StoredProcedure_<ObjectName>
```

فرم عمومی می‌تواند ObjectName تشخیص‌داده‌شده توسط Analyzer، از جمله نام Schema-qualified، را حمل کند.

نمونه:

```xml
<add
    key="ProviderLimitations"
    value="StoredProcedure_dbo.ArchiveFileDelete"
/>
```

برای Stored Procedure کد جاری مجموعه زیر را در Limitation قرار می‌دهد:

```text
SqlExecuteModuleStatement
SqlNullInsertSource
SqlObjectReference
SqlSelectStatement
SqlInsertStatement
SqlDeleteStatement
SqlUpdateStatement
```

### محدود کردن Table-Valued Function

فرم‌های پشتیبانی‌شده:

```text
TableValuedFunction_dbo.<FunctionName>
TableValuedFunction_<ObjectName>
```

فرم عمومی می‌تواند ObjectName تشخیص‌داده‌شده توسط Analyzer، از جمله نام Schema-qualified، را حمل کند.

نمونه:

```xml
<add
    key="ProviderLimitations"
    value="TableValuedFunction_dbo.InfoFunc"
/>
```

Operationهای این Rule در کد جاری:

```text
SqlTableValuedFunctionRefExpression
SqlExecuteModuleStatement
SqlNullInsertSource
SqlObjectReference
SqlSelectStatement
SqlInsertStatement
SqlDeleteStatement
SqlUpdateStatement
```

### ترکیب چند Rule در یک AppSetting

Config با `,` به چند Rule تقسیم می‌شود. برای مثال:

```xml
<add
    key="ProviderLimitations"
    value="Operation_SqlVariableDeclareStatement,Table_dbo.Users(SqlDeleteStatement|SqlUpdateStatement),StoredProcedure_dbo.ArchiveFileDelete"
/>
```

قواعد Separator:

```text
,   → جداکننده Ruleهای مستقل
|   → جداکننده Operationها داخل پرانتز Table Rule
```

### نکات دقیق Parser در Build جاری

-   مقدار بدون Trim با `Split(',')` خوانده می‌شود؛ برای اطمینان، دور Tokenها Space نگذارید.
-   `*` را برای حالت بدون محدودیت به‌تنهایی استفاده کنید.
-   Token `MaximumLimitation` در کد فعلی با همین Casing بررسی می‌شود؛ آن را دقیقاً با همین نام بنویسید.
-   Prefixهای `Operation_`، `Table_`، `StoredProcedure_` و `TableValuedFunction_` با مقایسه Case-Insensitive بررسی می‌شوند.
-   برای یک Table، همه Operationهای مدنظر را در **یک Rule** قرار دهید:

```text
Table_dbo.MyTable(SqlSelectStatement|SqlDeleteStatement|SqlUpdateStatement)
```

در Build جاری، Merge چند Rule جداگانه برای یک Table به‌دلیل نحوه استفاده از `Concat` قابل اتکا نیست؛ شکستن Operationهای یک Table بین چند Token توصیه نمی‌شود.

### 60.5. ترتیب و اولویت اعمال محدودیت‌ها

`CheckQueryLimitationsAsync` Query را با T-SQL Query Analyzer تحلیل می‌کند و سپس سه Check را با شرط OR اعمال می‌کند:

```text
Query
  ↓
T-SQL Query Analyzer
  ↓
1) WebConfig ProviderLimitations
  │  Match? → Reject
  ↓
2) System Settings ProviderLimitations
  │  Match? → Reject
  ↓
3) Form / SubForm Permissions
  │  Match? → Reject
  ↓
Allow Create/Edit
```

در کد:

```csharp
if (
    CheckWebConfigProviderLimitations(...)
    || await CheckSystemConfigurationProviderLimitationsAsync(...)
    || await CheckGBFormLimitationsAsync(...)
)
{
    return false;
}
```

بنابراین Ruleها **Deny-based و تجمعی** هستند: کافی است یکی از لایه‌ها Query را ممنوع کند. WebConfig سخت‌گیرانه‌ترین لایه است و Allow شدن در System Settings یا سطح دسترسی Form، Deny آن را خنثی نمی‌کند.

### 60.6. این محدودیت‌ها هنگام Run Provider اعمال نمی‌شوند

این موضوع برای طراحی امنیت بسیار مهم است:

```text
ProviderLimitations
≠
Provider Runtime Authorization
```

در وضعیت جاری، کنترل‌های بالا برای مرحله ایجاد/ویرایش Provider هستند و هنگام اجرای Provider ذخیره‌شده، همان Query دوباره بر اساس این سه لایه مجوزسنجی نمی‌شود.

پس Providerای که ساخته شده است باید از ابتدا با حداقل دسترسی و حداقل داده لازم طراحی شود؛ چون اتکا به `ProviderLimitations` به‌عنوان Runtime Permission Model اشتباه است.
