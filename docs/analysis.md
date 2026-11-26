# System Analysis
## Support Request Tracking System

این سند شامل تحلیل رفتاری و ساختاری **سیستم پیگیری درخواست‌های پشتیبانی (Support Request Tracking System)** است و مطابق با مستندات نیازمندی‌ها (`requirements.md`) و چشم‌انداز پروژه (`vision.md`) تدوین شده است.

---

## 1. نمودارهای فعالیت (Activity Diagrams)

با توجه به تمرکز آموزشی پروژه بر مدیریت چرخه حیات درخواست‌ها، قوانین تخصیص کارشناس و کنترل دسترسی، فرآیندهای کلیدی سیستم در قالب سه نمودار فعالیت مدل‌سازی شده‌اند.

### 1.1. نمودار فعالیت چرخه حیات کامل درخواست پشتیبانی (End-to-End Request Lifecycle)
این نمودار فرآیند کامل از لحظه ثبت درخواست توسط مشتری تا تخصیص توسط مدیر، بررسی و پاسخ‌دهی توسط کارشناس، و در نهایت بستن یا بازگشایی مجدد درخواست را نمایش می‌دهد.

```mermaid
flowchart TD
    Start((شروع)) --> Submit[مشتری: ثبت درخواست پشتیبانی<br/>عنوان، توضیحات، دسته‌بندی، فوریت و پیوست اختیاری]
    Submit --> ValidateReq{اطلاعات اجباری<br/>کامل است؟}
    ValidateReq -- خیر --> ShowErr[نمایش پیام خطای اعتبارسنجی]
    ShowErr --> Submit
    
    ValidateReq -- بله --> SetOpen[سیستم: ذخیره درخواست با وضعیت Open]
    SetOpen --> Assign[مدیر: تخصیص درخواست به یک کارشناس<br/>یا تخصیص به خود]
    Assign --> LogAssign[سیستم: ثبت تاریخچه تخصیص<br/>Assignment History]
    LogAssign --> AgentView[کارشناس: مشاهده صف درخواست‌ها<br/>بر اساس اولویت و فوریت]
    AgentView --> StartWork[کارشناس: تغییر وضعیت به In Progress]
    
    StartWork --> HandleLoop{نیاز به اطلاعات بیشتر<br/>از مشتری؟}
    HandleLoop -- بله --> AddInfo[مشتری: افزودن اطلاعات تکمیلی<br/>به درخواست باز]
    AddInfo --> HandleLoop
    
    HandleLoop -- خیر --> WriteNote[کارشناس: ثبت یادداشت پاسخ<br/>Response Note]
    WriteNote --> CheckNote{حداقل یک یادداشت پاسخ<br/>ثبت شده است؟}
    CheckNote -- خیر --> BlockTransition[سیستم: جلوگیری از تغییر وضعیت<br/>قانون کسب‌وکار #7]
    BlockTransition --> WriteNote
    
    CheckNote -- بله --> SetAnswered[کارشناس/مدیر: تغییر وضعیت به Answered]
    SetAnswered --> CustomerCheck{آیا مشکل مشتری<br/>حل شده است؟}
    
    CustomerCheck -- خیر --> ReHandle[ادامه پیگیری و ثبت پاسخ جدید]
    ReHandle --> WriteNote
    
    CustomerCheck -- بله --> SetClosed[کارشناس/مدیر: تغییر وضعیت به Closed<br/>قفل شدن ویرایش درخواست]
    SetClosed --> ReopenCheck{آیا درخواست مجدداً<br/>بازگشایی می‌شود؟}
    ReopenCheck -- خیر --> End((پایان))
    ReopenCheck -- بله --> ReopenEvent[رویداد صریح بازگشایی<br/>Explicit Reopening Event]
    ReopenEvent --> SetOpen
```

---

### 1.2. نمودار فعالیت صف درخواست‌های کارشناس (Agent Request Queue Logic)
این نمودار منطق دقیق نمایش صف کارشناس بر اساس قوانین کسب‌وکار شماره ۵، ۶ و ۸ (فیلتر کردن درخواست‌های بسته، اولویت‌بندی بر اساس سطح فوریت و نمایش انحصاری درخواست‌های با فوریت بالا در صورت وجود) را نشان می‌دهد.

```mermaid
flowchart TD
    Start((شروع)) --> Fetch[سیستم: واکشی درخواست‌های تخصیص‌یافته<br/>به کارشناس جاری]
    Fetch --> ExcludeClosed[سیستم: حذف درخواست‌های با وضعیت Closed<br/>قانون کسب‌وکار #8]
    ExcludeClosed --> CheckHighUrgency{آیا درخواستی با فوریت بالا<br/>High Urgency وجود دارد؟}
    
    CheckHighUrgency -- بله --> FilterHigh[سیستم: فیلتر کردن و نمایش فقط<br/>درخواست‌های با فوریت بالا<br/>قانون کسب‌وکار #6]
    CheckHighUrgency -- خیر --> SortAll[سیستم: مرتب‌سازی تمام درخواست‌های فعال<br/>بر اساس سطح فوریت و زمان ثبت]
    
    FilterHigh --> DisplayQueue[نمایش صف درخواست‌ها در رابط کاربری]
    SortAll --> DisplayQueue
    DisplayQueue --> End((پایان))
```

---

### 1.3. نمودار فعالیت احراز هویت و ورود به سیستم (Authentication & Login Security)
این نمودار فرآیند ثبت‌نام، تایید ایمیل و مکانیزم امنیتی مسدودسازی موقت حساب پس از ۵ بار تلاش ناموفق را مدل‌سازی می‌کند.

```mermaid
flowchart TD
    Start((شروع)) --> InputCreds[کاربر: وارد کردن ایمیل و رمز عبور]
    InputCreds --> CheckLock{آیا حساب به دلیل ۵ تلاش ناموفق<br/>قفل است؟}
    
    CheckLock -- بله --> CheckTime{آیا ۱۵ دقیقه<br/>گذشته است؟}
    CheckTime -- خیر --> DenyLock[سیستم: نمایش خطای مسدودسازی موقت]
    DenyLock --> End((پایان))
    CheckTime -- بله --> Unlock[سیستم: رفع محدودیت و صفر کردن شمارنده]
    Unlock --> VerifyCreds
    
    CheckLock -- خیر --> VerifyCreds{اطلاعات ورود<br/>صحیح است؟}
    VerifyCreds -- خیر --> IncFail[سیستم: افزایش شمارنده تلاش ناموفق]
    IncFail --> CheckCount{تعداد تلاش ناموفق<br/>به ۵ رسید؟}
    CheckCount -- بله --> LockAccount[سیستم: مسدودسازی ورود به مدت ۱۵ دقیقه]
    LockAccount --> End
    CheckCount -- خیر --> ShowInvalid[سیستم: نمایش خطای نامعتبر بودن اطلاعات]
    ShowInvalid --> InputCreds
    
    VerifyCreds -- بله --> CheckVerified{آیا ایمیل کاربر<br/>تایید شده است؟}
    CheckVerified -- خیر --> PromptVerify[سیستم: عدم اجازه ورود و نمایش پیام تایید ایمیل]
    PromptVerify --> End
    
    CheckVerified -- بله --> ResetCounter[سیستم: صفر کردن شمارنده خطا و ایجاد نشست]
    ResetCounter --> Dashboard[هدایت کاربر به داشبورد متناسب با نقش]
    Dashboard --> End
```

---

## 2. مدل مفهومی دامنه (Conceptual Domain Model)

مدل مفهومی دامنه، موجودیت‌های اصلی کسب‌وکار، ویژگی‌های کلیدی آن‌ها و روابط بین آن‌ها را مستقل از جزئیات پیاده‌سازی پایگاه‌داده نمایش می‌دهد.

### 2.1. نمودار کلاس مفهومی (Conceptual Class Diagram)

```mermaid
classDiagram
    class User {
        +UUID id
        +String email
        +String passwordHash
        +String fullName
        +UserRole role
        +Boolean isEmailVerified
        +Int failedLoginAttempts
        +DateTime lockedUntil
        +DateTime createdAt
    }

    class SupportRequest {
        +UUID id
        +String title
        +String description
        +String category
        +UrgencyLevel urgency
        +RequestStatus status
        +DateTime createdAt
        +DateTime updatedAt
        +DateTime closedAt
    }

    class ResponseNote {
        +UUID id
        +String content
        +Boolean isAdditionalCustomerInfo
        +DateTime createdAt
    }

    class AssignmentHistory {
        +UUID id
        +DateTime assignedAt
        +String reason
    }

    class Attachment {
        +UUID id
        +String fileName
        +String fileType
        +Int fileSize
        +String filePath
        +DateTime uploadedAt
    }

    class VerificationToken {
        +UUID id
        +String tokenHash
        +TokenType type
        +DateTime expiresAt
        +Boolean isUsed
    }

    %% Enumerations
    class UserRole {
        <<enumeration>>
        CUSTOMER
        AGENT
        MANAGER
    }

    class RequestStatus {
        <<enumeration>>
        OPEN
        IN_PROGRESS
        ANSWERED
        CLOSED
    }

    class UrgencyLevel {
        <<enumeration>>
        LOW
        MEDIUM
        HIGH
    }

    class TokenType {
        <<enumeration>>
        EMAIL_VERIFICATION
        PASSWORD_RESET
    }

    %% Relationships
    User "1" --> "0..*" SupportRequest : submits (Customer)
    User "0..1" --> "0..*" SupportRequest : actively assigned to (Agent/Manager)
    User "1" --> "0..*" ResponseNote : writes
    User "1" --> "0..*" VerificationToken : owns
    
    SupportRequest "1" *-- "0..*" ResponseNote : contains
    SupportRequest "1" *-- "0..*" Attachment : includes
    SupportRequest "1" *-- "1..*" AssignmentHistory : tracks
    
    User "1" --> "0..*" AssignmentHistory : assigned to (Assignee)
    User "1" --> "0..*" AssignmentHistory : assigned by (Manager)
```

---

### 2.2. تشریح موجودیت‌های دامنه (Domain Entities Dictionary)

| موجودیت (Entity) | نقش و مسئولیت در دامنه سیستم | قوانین کلیدی مرتبط |
| :--- | :--- | :--- |
| **User** | نماینده تمام کاربران سیستم (مشتری، کارشناس پشتیبانی، مدیر پشتیبانی). تفکیک دسترسی‌ها از طریق صفت `role` انجام می‌شود. | - ایمیل باید یکتا باشد.<br>- مشتری نمی‌تواند نقش خود را تغییر دهد.<br>- مدیر می‌تواند حساب کارشناس بسازد یا مشتری را ارتقا دهد. |
| **SupportRequest** | موجودیت محوری سیستم که درخواست پشتیبانی ثبت‌شده توسط مشتری را مدل می‌کند. | - در هر لحظه حداکثر **یک** مسئول فعال (`actively assigned to`) دارد.<br>- درخواست در وضعیت `Closed` قابل ویرایش نیست. |
| **ResponseNote** | پیام‌ها، پاسخ‌های کارشناسان و همچنین اطلاعات تکمیلی ثبت‌شده توسط مشتری در طول بررسی درخواست. | - انتقال از `In Progress` به `Answered` نیازمند وجود حداقل یک یادداشت پاسخ از سمت کارشناس/مدیر است. |
| **AssignmentHistory** | ثبت تاریخچه کامل تخصیص‌ها و تغییر مسئولیت هر درخواست به همراه زمان و شخص تخصیص‌دهنده. | - هرگونه تغییر در مسئول فعلی درخواست باید در این موجودیت ثبت و نگهداری شود. |
| **Attachment** | فایل‌ها و تصاویر پیوست‌شده توسط مشتری برای توضیح بهتر مشکل. | - به یک درخواست مشخص وابسته است (رابطه Composition). |
| **VerificationToken** | توکن‌های امنیتی یک‌بارمصرف و دارای زمان انقضا برای تایید ایمیل و بازیابی رمز عبور. | - پس از استفاده یا انقضای زمان، نامعتبر می‌شود. |