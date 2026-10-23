---
id: 1628
graphDocId: 1446
title: "API و Custom JavaScript فرآیند"
subTitle: null
module: "02-process"
wikiPath: "Process/1627/1628"
wikiUrl: "https://wiki.graphbpms.ir/Process/1627/1628"
parentPageUrl: "https://wiki.graphbpms.ir/Process/1627/1628"
breadcrumbs: ["مدیریت و مدل‌سازی فرآیندها","توسعه و یکپارچه‌سازی","API و Custom JavaScript فرآیند"]
parentId: 1627
parentTitle: "توسعه و یکپارچه‌سازی"
url: "1628"
priority: 1
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت فرآیند > مدیریت و مدل‌سازی فرآیندها > توسعه و یکپارچه‌سازی. مرجع فنی API و Custom JavaScript فرآیند است: شروع Process از فرم با absoluteMethodHelper و runProcessWithVerbosityLevel، سطوح Verbose، ادامه Message و ارسال Signal، سرویس‌های SharedApi شامل Login، RunProcess، ContinueProcessWithMessage، ThrowProcessSignal، SendFile و خروجی گزارش. همچنین قرارداد ScriptTask در C# و JavaScript، خواندن/نوشتن Variableها، Contextهای مختلف، Timeout، مدیریت خطا و فراخوانی RunProcess از Provider REST را پوشش می‌دهد."
updatedAt: "2026-10-18T16:08:51"
---

# API و Custom JavaScript فرآیند

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت و مدل‌سازی فرآیندها` ❯ `توسعه و یکپارچه‌سازی` ❯ `API و Custom JavaScript فرآیند`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/Process/1627/1628](https://wiki.graphbpms.ir/Process/1627/1628)

# مرجع فنی Process — API و Custom JS

این مرجع برای توسعه‌دهنده‌ای است که می‌خواهد فرآیند را از یک سامانهٔ دیگر، Custom JS فرم یا Script Task فراخوانی کند. سه قرارداد را جدا نگه دارید: /SharedApi برای مصرف‌کنندهٔ بیرونی، absoluteMethodHelper در محیط فرم GraphBPMS و توابع محیط اجرای Script Task. خروجی و شیوهٔ احراز هویت این سه یکسان نیست.

## آغاز از فرم با تنظیم استاندارد دکمه

اگر هدف فقط شروع فرآیند از فرم است، در تنظیمات اختصاصی فیلد دکمه، نوع عملیات را «شروع فرآیند» قرار دهید. فرآیند مقصد، عنوان نمونه و پارامترها را تنظیم کنید. عنوان می‌تواند از مقدار ثابت، مقدار سیستمی، کنترل، Query String یا پارامتر فرم ترکیب شود. نحوهٔ شروع «معمولی» یا «به‌صورت سرویس» و Target بازشدن فرم نیز در همین بخش تعیین می‌شوند.

[![شروع فرآیند از فیلد دکمه؛ انتخاب مقصد، ساخت عنوان، نوع شروع، Target و افزودن پارامتر](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4D2636B7-9BB3-49DB-8BAE-BFF2E1F4FFAC)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=4D2636B7-9BB3-49DB-8BAE-BFF2E1F4FFAC "بازکردن تصویر در اندازه اصلی")

شروع فرآیند از فیلد دکمه؛ انتخاب مقصد، ساخت عنوان، نوع شروع، Target و افزودن پارامتر

آغاز به‌صورت سرویس برای مدلی است که تا پایان به گام انتظار نرسد. UserTask، GraphMobileTask، ReceiveTask، TimerIntermediateCatchEvent، SignalIntermediateCatchEvent و CallActivityTask در راهنمای این مسیر به‌عنوان گام‌های بازدارنده ذکر شده‌اند. انتخاب حالت سرویس، انتظار واقعی مدل را حذف نمی‌کند.

## Context فرم جاری

`absoluteMethodHelper.getProcessInfo()` داده‌های Context را از Query String فرم جاری می‌خواند و شیء `{state,message,data}` می‌دهد. در پیاده‌سازی بررسی‌شده، اگر هر مقدار `null` یا رشتهٔ خالی باشد، state برابر `fail` است؛ این وضعیت را قبل از مصرف داده بررسی کنید. مقادیر Query String رشته هستند و در نقطهٔ نیاز باید پس از اعتبارسنجی تبدیل شوند.

| Property در data | پارامتر URL | معنی |
| --- | --- | --- |
| ProcessCreationID | pcid | شناسهٔ نمونهٔ اجرایی |
| ReferencesID | refid | شناسهٔ ارجاع |
| ProcessID | pid | شناسهٔ نسخهٔ فرآیند |
| FormsRecordID | recid | شناسهٔ رکورد فرم |
| TaskID | tid | شناسهٔ گام |
| EntityID | eid | شناسهٔ تعریف فرم |
| EntityMode | emd | حالت فرم: افزودن، ویرایش یا مشاهده |
| EntityConfigID | ecid | تنظیم فرم در فرآیند |
| EntityTypeID | etid | نوع عنصر فرآیند |
| ElementConfigID | elcid | تنظیم عنصر |
| IsUserTaskFormCopy | isUserTaskFormCopy | وضعیت رونوشت؛ اگر در URL نباشد، از سرویس وضعیت ارجاع دریافت می‌شود. نوع مقدار ممکن است برحسب منبع رشته، مقدار پاسخ یا null باشد. |

```javascript
const info = absoluteMethodHelper.getProcessInfo();
if (info.state !== "done") throw new Error(info.message);
const processCreationId = Number(info.data.ProcessCreationID);
const stepId = Number(info.data.TaskID);
if (!Number.isFinite(processCreationId) || !Number.isFinite(stepId)) {
  throw new Error("Context فرآیند معتبر نیست");
}
```

## شروع با runProcessWithVerbosityLevel

امضای شیء ورودی در کد بررسی‌شده از نام `variables` استفاده می‌کند. نام `variablesNameValue` در این فایل وجود ندارد. نام پارامتر را از امضای بارگذاری‌شده در استقرار مقصد بگیرید؛ تغییر خودسرانهٔ آن باعث ارسال‌نشدن متغیرها می‌شود.

| پارامتر | نوع/کاربرد | نکته |
| --- | --- | --- |
| processGroupID | number؛ شناسهٔ خانواده | متد جاری این ورودی را در ProcessGroup می‌فرستد؛ ProcessID ورودی این امضا نیست. |
| variables | Array از {Name,Value} | متغیرهای از پیش تعریف‌شده، با نام بدون @@ |
| customTitle | string | عنوان نمونه در ProcessTitle |
| runType | number / RunProcessType | برای آغاز پیام: RunProcessType.MessageStartEvent |
| anonymous | boolean | مسیر فرم ناشناس را انتخاب می‌کند؛ خودِ مقدار true مجوز اعطا نمی‌کند. |
| formId | number یا null | در مسیر ناشناس به درخواست افزوده می‌شود. |
| verboseLevel | integer، 0 تا 6 | در خود helper مقدار پیش‌فرض تعیین نشده؛ برای قرارداد روشن صریح ارسال کنید. |

```javascript
const response = await absoluteMethodHelper.runProcessWithVerbosityLevel({
  processGroupID: 1001, // شناسهٔ آموزشی؛ مقدار محیط خود را جایگزین کنید
  variables: [{ Name: "RequestCode", Value: "DEMO-001" }],
  customTitle: "درخواست آموزشی",
  runType: RunProcessType.MessageStartEvent,
  anonymous: false,
  formId: null,
  verboseLevel: 6
});
const result = response.content;
if (result?.HasError) throw new Error(result.Message || "اجرای فرآیند ناموفق بود");
```

در مسیر عادی، helper درخواست `POST /Process/RunProcess` می‌فرستد و در موفقیت `{state:"done",content:data}` می‌دهد. خطای کاربردی `HasError` با state برابر `error` و خطای انتقال با state برابر `fail` reject می‌شود. در Context ماژول موبایل، مسیر `/Mobile/RunProcessShared` و RunType موبایل استفاده می‌شود و content موفقیت `true` است؛ مسیر خواندن فرم از Content در این حالت کاربرد ندارد.

وقتی anonymous فعال باشد، مسیر `/AnonymousForm/RunProcess` استفاده می‌شود. سیاست دسترسی، فرم مجاز و هویت مؤثر اجرای ناشناس باید در استقرار مقصد تأیید شود. پارامترهای سمت مرورگر جای کنترل دسترسی سرور را نمی‌گیرند.

### بازکردن فرم بعد از شروع

ابتدا وجود TaskElementResult و ProcessFlowModel را کنترل کنید و سپس مقادیر را بخوانید. سطح خروجی‌ای که این جزئیات را حذف می‌کند برای این کاربرد مناسب نیست. اگر اجرا به پایان یا گام انتظار غیر فرمی رسیده باشد، نبود User Task طبیعی است.

```javascript
const task = response.content?.Content?.TaskElementResult;
const flow = task?.ProcessFlowModel;
if (flow && task?.IsUserTask === true) {
  const userTaskData = { ...flow, target: "_self", FormsRecordID: task.RecordID };
  new serviceManager().formView(
    userTaskData, flow.ReferenceID, flow.ProcessCreationID, "i"
  );
}
// target: _self صفحهٔ جاری، _blank تب تازه، _top سطح بالای صفحه
```

## هفت سطح Verbose و نمونهٔ کامل پاسخ

VerboseLevel شکل جزئیات پاسخ را تغییر می‌دهد. `HasError:false` به‌تنهایی پایان نمونه را ثابت نمی‌کند؛ ممکن است فرآیند به User Task یا انتظار رسیده باشد. در نمونه‌های زیر مسیر به End Event رسیده است. شناسه‌ها آموزشی شده‌اند؛ تمام فیلدهای هر نمونه حفظ شده‌اند.

| مقدار | نام | شکل Content |
| --- | --- | --- |
| 6 | Full | جزئیات Node/Process و متغیرهای خروجی پایان |
| 5 | Detailed | جزئیات اجرا بدون متغیرهای پایانی در نمونه |
| 4 | Basic | حداقل شناسهٔ نمونه و وضعیت کلی |
| 3 | Minimum | Content در نمونه null است؛ null در این سطح الزاماً خطا نیست. |
| 2 | PlainLite | آرایهٔ Name/Value با مقدار خام |
| 1 | Lite | آرایهٔ Name/Value با تبدیل رشتهٔ JSON به Object/Array در صورت امکان |
| 0 | BoostLite | Dictionary از نام به مقدار، با رفتار تبدیل Lite |

راهنمای helper مقدار `null` را معادل پیش‌فرض Full معرفی می‌کند، اما پیاده‌سازی JavaScript فقط مقدار را عبور می‌دهد و خود آن را به 6 تبدیل نمی‌کند. این پیش‌فرض به قرارداد سرور وابسته است؛ برای نتیجهٔ قابل اتکا 6 را صریح بفرستید. تفاوت Lite و PlainLite در نمونهٔ مقدار سادهٔ «1» دیده نمی‌شود؛ آن را با یک رشتهٔ JSON معتبر جداگانه آزمون کنید.

### سطح 6

```json
{
  "state": "done",
  "content": {
    "HasError": false,
    "ErrorCode": -1,
    "Message": "فرآیند به پایان رسید",
    "MessageException": "",
    "Content": {
      "ProcessCreationID": 10001,
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
          "ProcessID": 1001,
          "TaskID": 3001
        },
        "ElementBaseType": 3,
        "ElementType": 15,
        "IsUserTask": false,
        "ReferenceID": 20001,
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

### سطح 5

```json
{
  "state": "done",
  "content": {
    "HasError": false,
    "ErrorCode": -1,
    "Message": "فرآیند به پایان رسید",
    "MessageException": "",
    "Content": {
      "ProcessCreationID": 10001,
      "EventElementResult": {
        "ElementBaseType": 3,
        "ElementType": 15,
        "IsUserTask": false,
        "ReferenceID": 20001,
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

### سطح 4

```json
{
  "state": "done",
  "content": {
    "HasError": false,
    "ErrorCode": -1,
    "Message": "فرآیند به پایان رسید",
    "MessageException": "",
    "Content": {
      "ProcessCreationID": 10001
    }
  }
}
```

### سطح 3

```json
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

### سطح 2

```json
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

### سطح 1

```json
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

### سطح 0

```json
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

پوستهٔ کوچک‌حرف `state/content` در نمونه‌های بالا متعلق به helper شروع است. در API مستقیم، پاسخ سرور را با `HasError/Content` می‌خوانید؛ در helper ادامه نیز پوستهٔ شروع افزوده نمی‌شود.

## ادامهٔ انتظار پیام

برای Receive Task یا Message Boundary Event، شناسهٔ نمونه و شناسهٔ گام منتظر را از همان اجرای متوقف‌شده نگه دارید. شناسهٔ طراحی، شناسهٔ رکورد فرم و شناسهٔ ارجاع را به‌جای StepId نگذارید. ادامه دادن یک گام قبلاً مصرف‌شده، درخواست تازه نمی‌سازد.

```javascript
const raw = await absoluteMethodHelper.continueProcessByMessageWithVerbosityLevel(
  10001, 3001, [{ Name: "Decision", Value: "approved" }], 6
);
// این helper مستقیماً data پاسخ را resolve می‌کند؛ raw.content نداریم.
if (raw?.HasError) throw new Error(raw.Message || raw.MessageException || "ادامه ناموفق بود");
const task = raw?.Content?.TaskElementResult;
const flow = task?.ProcessFlowModel;
if (flow && task?.IsUserTask === true) {
  new serviceManager().formView(
    { ...flow, target: "_self", FormsRecordID: task.RecordID },
    flow.ReferenceID, flow.ProcessCreationID, "i"
  );
}
```

امضای این متد ترتیبی است: `(processCreationId, stepId, variables, verboseLevel)`. مسیر عادی `POST /Process/ContinueProcessByMessageWithVerbosityLevel` با کلیدهای `ProcessCreationId`، `StepId`، `Variables` و `VerboseLevel` است. در فرم ناشناس تشخیص‌داده‌شده توسط ViewModel، مسیر به `PUT /AnonymousForm/ContinueProcess` تغییر می‌کند و `FormId` افزوده می‌شود.

هر دو متد ادامه و متد سیگنال، موفقیت HTTP را با `resolve(data)` عبور می‌دهند و خودشان `HasError` را reject نمی‌کنند. کنترل خطای کاربردی در کد مصرف‌کننده ضروری است؛ ورود به then به‌تنهایی موفقیت کسب‌وکاری نیست.

## ارسال Signal از فرم

کلید سیگنال دامنهٔ گیرنده‌ها را تعیین می‌کند. فراخوانی می‌تواند همهٔ Signal Startهای منطبق را نمونه‌سازی کند و همهٔ Catchهای میانی منتظر با همان کلید را ادامه دهد؛ این عملیات معادل ادامهٔ یک Step مشخص نیست.

```javascript
const result = await absoluteMethodHelper.throwProcessSignal(
  "DemoSignal", [{ Name: "RequestCode", Value: "DEMO-001" }]
);
if (result?.HasError) throw new Error(result.Message || "ارسال سیگنال ناموفق بود");
```

helper از `POST /Process/ThrowSignal` با `signalKey` و `Variables` استفاده می‌کند. نتیجه خام است. اجرای سیگنال را با کلیدی محدود به سناریوی آزمایشی بررسی کنید تا دامنهٔ گیرندگان روشن باشد.

## امضاهای قدیمی و مهاجرت مصرف‌کننده

راهنمای همراه، متدهای زیر را برای نسخه‌های 2.1.5.131 و پایین‌تر معرفی کرده است. این شماره نسخه، محدودهٔ همان راهنماست؛ از وجود تابع در فایل JavaScript، نسخهٔ سرور را استنتاج نکنید.

```javascript
absoluteMethodHelper.runProcess(
  processGroupID, processID, roleID, userID, fromOut,
  variables, customTitle, runType, anonymous, formId
);
absoluteMethodHelper.continueProcessWithMessage(
  processCreationID, stepID, variables
);
```

runProcess قدیمی همان پوستهٔ state/content شروع را می‌سازد. continueProcessWithMessage به `POST /Process/ContinueProcessWithMessage` با `processCreationID`، `stepID` و `Variables` می‌فرستد و پاسخ خام می‌دهد. در کد بررسی‌شده پارامتر `fromOut` در امضا هست اما در بدنه مصرف نشده؛ صرف ارسال true را تضمین بازشدن فرم ندانید.

## سرویس‌های عمومی SharedApi

مصرف‌کنندهٔ بیرونی ابتدا Login می‌کند و PrivateToken معتبر را در هدر درخواست‌های بعدی می‌فرستد. عمر توکن تابع تنظیمات سامانه است. URLهای این مقاله با `https://bpms.example` نمایش داده می‌شوند و نمونهٔ نشانی آموزشی‌اند. نام و شکل بدنه از قرارداد هر endpoint پیروی می‌کند؛ آن‌ها را با پارامترهای helper مرورگر یکسان فرض نکنید.

| مسیر | کار | احراز هویت |
| --- | --- | --- |
| /SharedApi/Login | دریافت توکن؛ POST در مستند تولیدشدهٔ ESB | username/password/networkUser در بدنه |
| /SharedApi/RunProcess | آغاز فرآیند با آغاز پیام | privateToken در Header |
| /SharedApi/ContinueProcessWithMessage | ادامهٔ یک نمونه در گام منتظر | privateToken در Header |
| /SharedApi/ThrowProcessSignal | ارسال Signal به Catchهای منطبق | privateToken در Header |
| /SharedApi/SendFile | ثبت فایل در آرشیو | privateToken در Header |
| /SharedApi/GenerateStimulExportFile | خروجی قالب چاپ گزارش | privateToken در Header |

**وابسته به استقرار:** متن قرارداد عمومی برای پنج سرویس بعد از Login ستون روش HTTP را ذکر نکرده است. روش HTTP را از مستند فعال همان استقرار یا قرارداد سرور کنترل کنید؛ روش‌های POST/PUT helper داخلی، اثبات روش همان endpoint عمومی نیستند.

### Login و خواندن توکن

| ورودی | نوع | الزام |
| --- | --- | --- |
| username / userName | string | نام حساب فعال؛ راهنمای عمومی username و سند تولیدشده userName را نمایش می‌دهد؛ نام قرارداد مقصد را عیناً بگیرید. |
| password | string | الزامی؛ در کد Client-Side یا بستهٔ مستندات قرار نگیرد. |
| networkUser | boolean | الزامی در جدول؛ false پیش‌فرض، برای ورود LDAP مطابق تنظیم حساب true |

```json
{
  "userName": "<ACCOUNT_NAME>",
  "password": "<PASSWORD_FROM_SECURE_INPUT>",
  "networkUser": false
}
```

پاسخ توکن در سند تولیدشدهٔ ESB داخل `Content.Token.PrivateToken` قرار دارد. نمونه‌های کد تولیدشدهٔ Fetch/jQuery توکن را در ریشه می‌خوانند؛ این دو نمایش با هم ناسازگارند. مسیر را با پاسخ واقعی Login تطبیق دهید. جدول قدیمی Login نیز PublicToken را دو بار با دو معنای متفاوت آورده است؛ ردیف تکراری را تعریف یک فیلد خطای معتبر ندانید.

```json
{
  "HasError": false,
  "ErrorCode": -1,
  "Message": "عملیات موفق",
  "MessageException": "",
  "Content": { "Token": { "PrivateToken": "<PRIVATE_TOKEN>" } }
}
```

در ESB هدر `privateToken` یا `Authorization: Bearer <PRIVATE_TOKEN>` در مستند تولیدشده پذیرفته شده است. برای SharedApi عمومی، هدر مستندشده `privateToken` است؛ پشتیبانی Bearer را بدون قرارداد همان مسیر تعمیم ندهید.

### RunProcess: مدل ورودی

| کلید | نوع | قاعده |
| --- | --- | --- |
| ProcessGroup | number، اختیاری مشروط | فقط یکی از ProcessGroup یا Process؛ آخرین نگارش فعال خانواده انتخاب می‌شود. |
| Process | number، اختیاری مشروط | نسخهٔ مشخص باید فعال باشد. |
| RoleID | number | شناسهٔ نقش مجاز آغازکننده؛ الزامی در قرارداد عمومی |
| UserID | number | شناسهٔ کاربر مجاز آغازکننده؛ الزامی در قرارداد عمومی |
| RunType | number | برای مسیر آغاز پیام مقدار 500002 |
| Variables | Array، اختیاری | نام متغیر تعریف‌شده بدون @@؛ Name/Value یا name/value مطابق قرارداد مقصد |
| ProcessTitle | string، اختیاری | عنوان نمونه؛ در نبود آن عنوان پیش‌فرض طراحی |

```json
{
  "ProcessGroup": 1001,
  "RoleID": 101,
  "UserID": 201,
  "RunType": 500002,
  "Variables": [
    { "name": "RequestCode", "value": "DEMO-001" },
    { "name": "Count", "value": 2 },
    { "name": "Rows", "value": "[{\"ItemCode\":\"A01\",\"Quantity\":2}]" }
  ],
  "ProcessTitle": "درخواست آموزشی"
}
```

متن عمومی نوع value را در جدول string آورده ولی در مثال مقدار عددی و JSON نیز به کار برده است. برای دادهٔ پیچیده، نمونهٔ مستند رشتهٔ JSON است؛ آن را یک بار serialize کنید و ساختار مصرف‌کننده را با خروجی واقعی کنترل کنید. حساسیت سرور به حروف Name/Value از اختلاف مثال‌ها قابل نتیجه‌گیری نیست.

**هویت اجرا:** وجود UserID/RoleID در بدنه اجازهٔ جعل هویت یا عبور از دسترسی نیست. نسبت این دو مقدار با هویت احرازشدهٔ توکن و امکان جانشینی باید در استقرار مقصد تأیید شود. متد جاری helper این دو ورودی را اصلاً در امضای شیء خود ندارد.

### پاسخ مشترک سرویس‌ها

| Property | نوع مستند | تفسیر |
| --- | --- | --- |
| HasError | boolean | خطای کاربردی؛ مستقل از موفقیت انتقال HTTP |
| ErrorCode | number یا null | در قرارداد عمومی -1 نشانهٔ عملیات موفق |
| Message | string یا null | پیام نتیجه |
| MessageException | string یا null | جزئیات خطا، در صورت وجود |
| Content | any یا null | دادهٔ اختصاصی هر endpoint و سطح Verbose |

### ContinueProcessWithMessage: ورودی و ReferenceState

| ورودی | نوع | توضیح |
| --- | --- | --- |
| processCreationID | number، الزامی | نمونهٔ منتظر |
| stepID | number، الزامی | گام منتظر همان نمونه |
| Variables | Array، nullable | آرایهٔ نام/مقدار اختیاری |

```json
{
  "processCreationID": 10001,
  "stepID": 3001,
  "Variables": [{ "name": "Decision", "value": "approved" }]
}
```

| ReferenceState | نام | معنی |
| --- | --- | --- |
| 500090 | IsFired | قبلاً ادامه یافته؛ ادامهٔ دوباره با همین اطلاعات ممکن نیست. |
| 500091 | IsNotExist | نمونه/گام قابل ادامه با این اطلاعات وجود ندارد. |
| 500092 | IsNotFired | در راهنما مخفی و غیرقابل استفاده معرفی شده است. |
| 500093 | IsExceptionMode | ادامه با خطا؛ MessageException را بررسی کنید. |
| 500094 | IsFiredSuccessfully | ادامه با موفقیت انجام شد. |

```json
{
  "HasError": false,
  "ErrorCode": -1,
  "Message": "ادامه با موفقیت انجام شد",
  "MessageException": "",
  "Content": {
    "ReferenceState": 500094,
    "ReferenceStateMessage": "IsFiredSuccessfully"
  }
}
```

مقادیر پیام مثال آموزشی هستند؛ کد ReferenceState و HasError را بررسی کنید. موفقیت «ادامه» با «پایان کل فرآیند» یکسان نیست.

### ThrowProcessSignal: بدنه و نتیجهٔ چندنمونه‌ای

```json
{
  "SignalKey": "DemoSignal",
  "Variables": [{ "Name": "RequestCode", "Value": "DEMO-001" }]
}
```

SignalKey رشتهٔ الزامی است. Variables آرایهٔ اختیاری نام/مقدار است؛ غلط املایی `Variebale` در عنوان جدول عمومی با نمونهٔ بدنه و helper سازگار نیست و کلید قابل استفاده در نمونه‌ها `Variables` است.

```json
{
  "HasError": false,
  "ErrorCode": -1,
  "Message": "عملیات موفق",
  "MessageException": "",
  "Content": [
    {
      "ProcessCreationID": 10001,
      "StatusDescription": "وضعیت ادامهٔ این نمونه",
      "EndVariables": [{ "Name": "Result", "Value": "approved" }]
    }
  ]
}
```

به ازای هر نمونهٔ متأثر یک ردیف نتیجه وجود دارد. EndVariables فقط وقتی می‌تواند غیرخالی باشد که ادامهٔ ناشی از سیگنال به پایانی برسد که متغیر خروجی تعریف کرده است؛ آرایهٔ خالی را به‌تنهایی شکست سیگنال تفسیر نکنید.

### SendFile

| پارامتر | نوع | معنی |
| --- | --- | --- |
| fileContent | string، الزامی | محتوای فایل به Base64 |
| filename | string، الزامی | نام همراه پسوند مانند demo.pdf |
| Result | string، nullable، خروجی | شناسهٔ فایل ذخیره‌شده در آرشیو |

سرویس جزئیات فایل را بررسی می‌کند. مقدار خروجی شناسهٔ آرشیو است؛ آن را بدون بررسی نوع فیلد مقصد جای محتوای فایل نگذارید.

### GenerateStimulExportFile

| پارامتر | نوع | معنی |
| --- | --- | --- |
| exportId | number، الزامی | شناسهٔ قالب چاپ/خروجی طراحی‌شده |
| paramList | Array، الزامی | ردیف‌های ResourceId و Parameters |
| fileExportType | string، اختیاری | Pdf، html، csv یا jpg؛ پیش‌فرض pdf |
| Content | string، nullable، خروجی | فایل تولیدشده به Base64 |

```json
{
  "exportId": 1001,
  "paramList": [
    {
      "ResourceId": 2001,
      "Parameters": [{ "name": "RequestId", "value": "10001" }]
    }
  ],
  "fileExportType": "Pdf"
}
```

## قرارداد Script Task: C# و JavaScript

کد Script Task در محیط اجرای فرآیند قرار می‌گیرد. آن را با Custom JS مرورگر که به absoluteMethodHelper دسترسی دارد یکسان نگیرید. ورودی‌ها در args هستند؛ سه ورودی پایه ProcessID، ProcessCreationID و CurrentElementID به‌ترتیب نسخه، نمونه و گام جاری را مشخص می‌کنند. پارامترهای تعریف‌شدهٔ طراح نیز با همان نام از args خوانده می‌شوند.

### Timeout گام و سقف پیکربندی سرور

Timeout گام برحسب ثانیه است. در پیاده‌سازی بررسی‌شده، سقف مؤثر اجرا کمینهٔ Timeout همان گام و مقدار `ScriptTask:MaximumTimeout` در پیکربندی سرور است؛ بزرگ‌کردن مقدار پنل، محدودیت سرور را دور نمی‌زند. عبور زمان اجرا از سقف، خطای اجرا ایجاد می‌کند و خطای نمونه در `ProcessErrorLogs` ثبت می‌شود. مقدار واقعی پیکربندی محیط مقصد باید جدا بررسی شود؛ عدد ۶۰ در تصویر، مقدار همان تنظیم است و تضمین سقف همهٔ استقرارها نیست.

محدودیت تعداد دستورها از `ScriptTask:MaximumStatements` می‌آید. اجرای C# نیز به فعال‌بودن `ScriptTask:EnableCSharpLanguage` وابسته است. این تنظیمات سرور را با ورودی‌های args یا تنظیم پذیرش خروجی Null یکسان نگیرید.

| کار | C# | JavaScript |
| --- | --- | --- |
| هویت جاری | GetUserID(), GetRoleID(), GetOrganizationId() | getUserID(), getRoleID(), getOrganizationId() |
| شناسهٔ خانواده | GetProcessGroupID(processID) | در نمونهٔ JS همراه ذکر نشده است. |
| خواندن با مسیر | GetVariableValue<T>(path,processID,creationID) | getVariableValue(path,processID,creationID) |
| خواندن خام | GetVariableRawValue<T>(name,processID,creationID) | getVariableRawValue(name,processID,creationID) |
| نوشتن خام | SetVariable(destVar,value,creationID) | setVariable(destVar,value,creationID) |
| نوشتن ServiceData | SetVariableAsServiceData(dest,object\[\],creationID) | setVariableAsServiceData(dest,recordsArray,creationID) |
| نوشتن FormData | SetVariableAsFormData(dest,formId,formData,creationID) | setVariableAsFormData(dest,formId,formData,creationID) |
| کپی | CopyVariable(src,dest,processID,creationID) | copyVariable(src,dest,processID,creationID) |
| Session | SetSession(name,value) / GetSession(name) | setSession(name,value) / getSession(name) |
| تنظیم مجاز سیستم | در نمونهٔ C# همراه ذکر نشده است. | getConfig(key) |
| خروجی | return از Main | قرار دادن مقدار در result |

برای خواندن خام نامی مانند `@@VarName` و برای انتخاب عضو مسیرهایی مانند `@@VarName:ServiceData[0]['FieldName']` یا `@@VarName:FormData['FieldName']` استفاده می‌شود. نتیجهٔ خواندن خام را با نتیجهٔ مسیر‌یابی یک فیلد یکسان نگیرید. در C# مقدارهای args از نوع object هستند و قبل از مصرف به نوع مناسب تبدیل می‌شوند.

```csharp
using System;
using System.Collections.Generic;

namespace RTC
{
    public class MyClass : ProcessRunTimePlugin
    {
        public override object Main(IDictionary<string, object> args)
        {
            var processId = (int)args["ProcessID"];
            var creationId = (int)args["ProcessCreationID"];
            var currentStep = (int)args["CurrentElementID"];
            var code = GetVariableRawValue<string>("@@RequestCode", processId, creationId);
            SetVariable("@@CheckedCode", code, creationId);
            return true;
        }
    }
}
```

```javascript
const processId = args.ProcessID;
const creationId = args.ProcessCreationID;
const currentStep = args.CurrentElementID;
const requestCode = getVariableRawValue("@@RequestCode", processId, creationId);
setVariable("@@CheckedCode", requestCode, creationId);

const rows = [{ ItemId: 101, Title: "مورد آموزشی", Quantity: 2 }];
setVariableAsServiceData("@@Rows", rows, creationId);
// نمونهٔ مجزا برای FormData:
// setVariableAsFormData("@@FormResult", 1001, { Id: 42, Title: "نمونه" }, creationId);
result = true;
```

در توضیح قدیمی C# امضای CopyVariable با نوع `number` نوشته شده است؛ number نوع C# نیست. در فراخوانی نمونه، شناسه‌ها int هستند. برای تعریف امضای دقیق افزونهٔ سمت سرور، مرجع Runtime همان استقرار لازم است؛ امضای مستند را به کد کلاس جدید تبدیل نکنید.

### نمونهٔ کامل C# همراه Script Task

متن زیر، فایل کامل نمونه همراه پنل است و توضیحات داخل آن نیز حفظ شده‌اند. برای خواندن قرارداد ورودی، خروجی و توابع به [قرارداد Script Task](#technical-script) مراجعه کنید. نام Variableها، شناسه‌ها و داده‌های آن مقادیر آموزشی‌اند و باید با مدل مقصد تطبیق داده شوند.

واژه `number` در توضیح امضای `CopyVariable`، عین متن راهنمای همراه است و نوع معتبر C# نیست؛ خود مثال فراخوانی، شناسه‌ها را به `int` تبدیل می‌کند. بدنه نمونه مقدار `1019,1019` را برمی‌گرداند؛ این رشته، شناسه آماده برای استفاده در محیط مقصد نیست.

```csharp
using System;
using System.Linq;

namespace RTC
{
    public class MyClass : ProcessRunTimePlugin
    {
        /// <summary>
        /// Important: args structure based on name|value format,
        ///            name is incoming variable name,
        ///            value is object and you need to cast to RAW value type like INT, STRING, DOUBLE, FLOAT or etc...
        /// args param items:
        ///     {int} args["ProcessID"] ==> ProcessID شناسه فرآیند
        ///     Exapmle: var processId = (int)args["ProcessID"];
        ///     {int} args["ProcessCreationID"] ==> ProcessCreationID شناسه فرآیند ایجاد شده
        ///     Exapmle: var processCreationId = (int)args["ProcessCreationID"];
        ///     {int} args["CurrentElementID"] ==> CurrentElementID شناسه المان جاری یا (Script task) = (Step ID)
        ///     Exapmle: var currentElementId = (int)args["CurrentElementID"];
        ///     {any} args["YOUR_VARIABLE_NAME"] ==> ورودی مشخص شده توسط کاربر
        ///     Exapmle: var variableName = args["YOUR_VARIABLE_NAME"];
        /// -------------------------------------------------------------------------------------
        /// global functions:
        ///     int GetRoleID() دریافت نقش جاری
        ///     int GetUserID() دریافت کاربر جاری
  ///     int? GetOrganizationId() دریافت سازمان جاری
        ///     int GetProcessGroupID(int processID) دریافت شناسه گروه فرآیندی جاری
        ///     T GetVariableValue<T>(string variable, int processID, int processCreationID) دریافت مقدار متغیر
        ///     Exapmle:  var fromGVar = GetVariableValue<string>("@@VarName:ServiceData[0]['ColName']", (int)args["ProcessID"], (int)args["ProcessCreationID"]);
  ///     T GetVariableRawValue<T>(string variableName, int processID, int processCreationID) دریافت مقدار خام متغیر
        ///     Exapmle:  var fromRawGVar = GetVariableRawValue<string>("@@VarName", (int)args["ProcessID"], (int)args["ProcessCreationID"]);
        ///     void SetVariable(string destVar, string value, int processCreationID) ست کردن مقدار جدید در متغیر
        ///     Exapmle:  var result ="This is test"; SetVariable("@@VarName", result, (int)args["ProcessCreationID"]);
  ///     void SetVariableAsServiceData(string dest, object[] values, int processCreationID) ست کردن مقدار جدید در متغیر به صورت سرویس دیتایی
        ///     Exapmle:  var records = new object[] { new { Id = 0, Name = "Amir" }, new { Id = 1, Name = "Ahmad" } }; SetVariableAsServiceData("@@VarName", records, (int)args["ProcessCreationID"]);
  ///     void SetVariableAsFormData(string dest, int formId, object formData, int processCreationID) ست کردن مقدار جدید در متغیر به صورت فرم دیتایی
        ///     Exapmle:  var formData = new { Id = 0, Name = "Amir" }; SetVariableAsFormData("@@VarName" ,1100 ,formData ,(int)args["ProcessCreationID"]);
        ///     bool CopyVariable(string srcVar, string destVar, number processID, number processCreationID) کپی مقدار متغیر در متغیر دیگر
        ///     Exapmle:  var successful = CopyVariable("@@VarName:ServiceData[0]['ColName']", "@@CopyVarName", (int)args["ProcessID"], (int)args["ProcessCreationID"]);
 ///     void SetSession(string name, object value) ذخیره سشن مورد نظر
        ///     Exapmle:  SetSession("session_key", "value");
        ///     object GetSession(string name) دریافت سشن مورد نظر
        ///     Exapmle:  var sessionValue = GetSession("session_key");
        /// </summary>
        public override object Main(System.Collections.Generic.IDictionary<string,object> args)
        {
            return "1019,1019";
        }
    }
}
```

### نمونهٔ کامل JavaScript همراه Script Task

متن زیر، فایل کامل نمونه همراه پنل است و توضیحات داخل آن نیز حفظ شده‌اند. برای خواندن قرارداد ورودی، خروجی و توابع به [قرارداد Script Task](#technical-script) مراجعه کنید. نام Variableها، شناسه‌ها و داده‌های آن مقادیر آموزشی‌اند و باید با مدل مقصد تطبیق داده شوند.

```javascript
/*
====================================================================================================
📘 راهنمای جامع Script Task (جاوا اسکریپت - JS)
====================================================================================================

۱. متغیرهای سیستمی ورودی (در شیء args):
   - args.ProcessID         : شناسه مدل فرآیند
   - args.ProcessCreationID : شناسه نمونه/درخواست جاری (پرونده)
   - args.CurrentElementID  : شناسه گام جاری (Step / Element ID)
   - args.نام_پارامتر        : پارامترهای ورودی تعریف شده در تنظیمات اسکریپت

۲. اطلاعات هویتی جاری:
   - getUserID()            : شناسه کاربر جاری (int)
   - getRoleID()            : شناسه نقش کاربر جاری (int)
   - getOrganizationId()    : شناسه سازمان جاری (int?)

۳. نحوه خواندن متغیرها (Read / Get):
   قاعده پیشوندها:
   • مقدار خام (Raw):             getVariableRawValue("@@VarName", processId, creationId);
   • خروجی سرویس (ServiceData):   getVariableValue("@@VarName:ServiceData[0]['FieldName']", processId, creationId);
   • داده فرم (FormData):         getVariableValue("@@VarName:FormData['FieldName']", processId, creationId);

۴. نحوه ذخیره و ویرایش متغیرها (Write / Set):
   • ذخیره مقدار خام/متنی:       setVariable("@@TargetVar", "مقدار متنی یا جیسون", creationId);
   • ذخیره به صورت ServiceData:   setVariableAsServiceData("@@TargetVar", recordsArray, creationId);
   • ذخیره به صورت FormData:      setVariableAsFormData("@@TargetVar", formId, formDataObject, creationId);
   • کپی مقدار متغیر:            copyVariable("@@SourceVar", "@@TargetVar", processId, creationId);

۵. توابع کمکی دیگر:
   - setSession(name, value) / getSession(name) : ذخیره و خواندن از Session
   - getConfig(key)                             : خواندن تنظیمات مجاز سیستم

۶. خروجی اسکریپت:
   - مقدار نهایی را در متغیر result قرار دهید: result = true;
====================================================================================================
*/

// دریافت شناسه‌های پایه فرآیند
const processId   = args.ProcessID;
const creationId  = args.ProcessCreationID;
const currentStep = args.CurrentElementID;

// -------------------------------------------------------------
// ۱) نمونه خواندن داده‌ها:
// -------------------------------------------------------------
// الف) خواندن داده خام (Raw Value):
// const rawValue = getVariableRawValue("@@CustomerCode", processId, creationId);

// ب) خواندن از فیلد سرویس با پیشوند ServiceData:
// const token = getVariableValue("@@AuthResult:ServiceData[0]['AccessToken']", processId, creationId);

// ج) خواندن از فیلد فرم با پیشوند FormData:
// const nationalCode = getVariableValue("@@UserForm:FormData['NationalCode']", processId, creationId);

// -------------------------------------------------------------
// ۲) نمونه ذخیره اطلاعات:
// -------------------------------------------------------------
// الف) ذخیره مقدار خام:
// setVariable("@@IsApproved", "true", creationId);

// ب) ذخیره خروجی جدولی با فرمت استاندارد ServiceData:
// const tableData = [
//     { ItemId: 101, Title: "مورد اول", Price: 50000 },
//     { ItemId: 102, Title: "مورد دوم", Price: 75000 }
// ];
// setVariableAsServiceData("@@OrderList", tableData, creationId);

// -------------------------------------------------------------
// منطق محاسباتی و شرطی خود را اینجا بنویسید:
// -------------------------------------------------------------

result = true;
```

## یک متغیر در چهار Context

| محل استفاده | الگوی نام | نکته |
| --- | --- | --- |
| بدنهٔ API / helper | Name: "VarName" | پیشوند @@ ارسال نمی‌شود. |
| توابع Script Task | @@VarName:ServiceData\[0\]\['Field'\] | مسیر کامل با نام متغیر و نوع ظرف |
| عنوان و شرح ارجاع | |V\_VarName:FormData\['Field'\]| | پایپ ابتدا و انتها ضروری است. عنوان اختصاصی پس از پایان گام اعمال می‌شود، اما فقط از متغیرهای ثبت‌شدهٔ گام‌های قبلی می‌خواند؛ نتیجهٔ تازهٔ Script یا Service را در تنظیم عنوان گام بعد مصرف کنید. حالت افزودن، متن جدید را بدون جداکنندهٔ خودکار به عنوان قبلی متصل می‌کند؛ فاصله، خط جدید یا جداکنندهٔ لازم را طراح باید در خود متن تعیین کند. |
| ویرایشگر شرط Gateway | @VarName:ServiceData\[Index\]\['Field'\] | راهنمای جدید کنترل این الگو را نشان می‌دهد؛ توضیح قدیمی V\_ دارد. از راهنمای همان کنترل استفاده کنید. |

در مسیرهای داده، `*` مرز تبدیل یک رشتهٔ JSON به ساختار داخلی آن است؛ برای Object واقعی ستارهٔ اضافی نگذارید. برای مثال `ServiceData[0]['Content']*['Rows'][0]['Code']`. Index از صفر شروع می‌شود. قواعد ساده/پیشرفتهٔ متغیر، گیرندهٔ Lane و TaskId موبایل در مقالهٔ متغیرها و Taskهای مربوط شرح داده شده‌اند.

در دسترسی سادهٔ Lane، دادهٔ نقش باید RoleID و دادهٔ کاربر باید UserID داشته باشد؛ روش پیشرفته برای نقش یا کاربر مسیر مقدار تک‌شناسه یا رشتهٔ شناسه‌های جداشده با کاما را می‌خواند. برای Graph Mobile Task نیز چند شناسهٔ وظیفهٔ Limited با کاما جدا می‌شوند. در شرط Gateway از 0/1 طبق راهنمای کنترل استفاده کنید، نه True/False.

## آزمون قرارداد و مدیریت خطا

1.  با متغیر ساده، JSON معتبر، خروجی null و دادهٔ چندردیفی، شکل واقعی پاسخ را ثبت کنید.
2.  شروع helper، ادامهٔ helper و API مستقیم را جدا آزمایش کنید؛ wrapperهایشان متفاوت است.
3.  برای آغاز، نسخهٔ فعال و گیرندهٔ مجاز؛ برای ادامه، جفت Creation/Step؛ و برای Signal، تمام گیرنده‌های کلید را کنترل کنید.
4.  پس از موفقیت HTTP، HasError و وضعیت تخصصی مانند ReferenceState یا IsEndEvent را بررسی کنید.
5.  نمونهٔ فاقد فرم را به formView ندهید و قبل از destructuring وجود شیء را کنترل کنید.
6.  خطای احراز هویت، عدم دسترسی، دادهٔ نامعتبر و گام مصرف‌شده را در تست منفی بگنجانید.

## انتخاب پاسخ مناسب برای مصرف‌کنندهٔ End

متغیرهای خروجی End و سطح جزئیات پاسخ API دو تنظیم مکمل‌اند. End نام و مقدار خروجی‌های فرآیند را مشخص می‌کند؛ `verboseLevel` در متدهای دارای Verbosity تعیین می‌کند چه بخش‌هایی از نتیجه و با چه شکل ساختاری به فراخواننده برسند. پیش از نوشتن مسیر دسترسی JavaScript، سطح انتخاب‌شده را مشخص کنید؛ مسیری که در Full به متغیرهای انتهایی می‌رسد با ساختار Lite یا BoostLite یکسان نیست.

برای شروع از `runProcessWithVerbosityLevel` و برای ادامهٔ انتظار پیام از `continueProcessByMessageWithVerbosityLevel` مطابق قراردادهای همین مرجع استفاده کنید. ارسال Signal با `throwProcessSignal` عملیات جداگانه‌ای است. نام گفتاری «ContinueProcess» را به امضای دلخواه تبدیل نکنید؛ نام و پارامترهای دقیق تابع را از کد helperِ همان نسخه بگیرید.

## فراخوانی RunProcess از Provider REST

در مدیریت منبع دادهٔ REST، محل پارامتر، نام، مقدار ثابت و نوع داده در ستون‌های مستقل قرار دارند. نمونهٔ فراخوانی شروع فرآیند، `privateToken` را در `header` و `ProcessGroup` را در `body` تعریف می‌کند. نوع محتوا `application/json; charset=utf-8` است. منوی نوع، گزینه‌هایی مانند String، Int32، Boolean، DateTime، Guid، Complex و File دارد؛ نوع نهایی باید از قرارداد سرویس گرفته شود.

[![تعریف پارامترهای REST برای فراخوانی سرویس؛ privateToken در header و ProcessGroup در body.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=F970AD83-27FF-4C80-B6D4-3C2DA0B87CF1)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=F970AD83-27FF-4C80-B6D4-3C2DA0B87CF1 "بازکردن تصویر در اندازه اصلی")

تعریف پارامترهای REST برای فراخوانی سرویس؛ privateToken در header و ProcessGroup در body.

## پارامترهای قرارداد RunProcess

در نمونهٔ آموزشی، مسیر `/Process/RunProcess` با نام‌های `ProcessGroup`، `Process`، `RoleID`، `UserID`، `RunType`، `Variables` و `ProcessTitle` آمده است. مقدار نمونهٔ `RunType` برابر `500002` است. این تصویر نمونهٔ مسیر داخلی `/Process/RunProcess` را نشان می‌دهد. برای آغاز پیام از مقدار نام‌دار `RunProcessType.MessageStartEvent` استفاده می‌شود. شروع با گروه، نسخهٔ فعال همان گروه را هدف می‌گیرد و Variables باید صریح ارسال شود. قرارداد `/SharedApi` و helper فرم را با این نمونه یکی نگیرید؛ [مرجع شروع در Context فرم](#technical-helper-start) و [سرویس‌های عمومی](#technical-public-api)، نام ورودی‌ها، احراز هویت و سطوح خروجی جاری را جدا توضیح می‌دهند. شناسه‌های عددی تصویر متعلق به نمونهٔ آموزشی‌اند.

[![نمونهٔ یادداشت قرارداد فراخوانی RunProcess با پارامترهای فرآیند، کاربر، نقش، متغیرها و عنوان.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=E3F13320-161B-4961-9348-749B871B253E)](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=E3F13320-161B-4961-9348-749B871B253E "بازکردن تصویر در اندازه اصلی")

نمونهٔ یادداشت قرارداد فراخوانی RunProcess با پارامترهای فرآیند، کاربر، نقش، متغیرها و عنوان.
