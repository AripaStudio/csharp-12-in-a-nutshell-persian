##  📝مبانی زبان #C
در این فصل، مبانی پایه‌ای زبان سی‌شارپ را معرفی می‌کنیم.
تقریباً تمامی لیست‌های کد ارائه‌شده در این کتاب، به صورت نمونه‌های تعاملی در LINQPad قابل دسترسی هستند. کار کردن با این نمونه‌ها در کنار مطالعهٔ کتاب، روند یادگیری را تسریع می‌کند، چرا که می‌توانید کدها را ویرایش کرده و بلافاصله نتایج را مشاهده کنید بدون نیاز به تنظیم پروژه‌ها یا Solutionها در ویژوال استودیو.

برای دانلود نمونه‌ها، در LINQPad روی تب Samples کلیک کنید، سپس گزینهٔ "Download more samples" را انتخاب نمایید. LINQPad رایگان است — برای دریافت به آدرس زیر مراجعه کنید:
http://www.linqpad.net

## اولین برنامه سی‌شارپ
برنامه زیر عدد ۱۲ را در ۳۰ ضرب کرده و نتیجه (۳۶۰) را روی صفحه نمایش می‌دهد. علامت // نشان‌دهنده توضیحات (کامنت) است:

```C#
int x = 12 * 30;                  // عبارت ۱  
System.Console.WriteLine(x);      // عبارت ۲
```
برنامه ما از دو عبارت تشکیل شده است. عبارات در سی‌شارپ به ترتیب اجرا شده و با سمیکولن (;) پایان می‌یابند.

عبارت اول حاصل ضرب 12 * 30 را محاسبه کرده و نتیجه را در متغیری به نام x از نوع int (عدد صحیح ۳۲ بیتی) ذخیره می‌کند.

عبارت دوم متد WriteLine را از کلاس Console (تعریف‌شده در فضای نام System) فراخوانی می‌کند که مقدار x را در پنجره خروجی نمایش می‌دهد.

توضیح مفاهیم کلیدی
متد (Method): یک تابع که وظیفه خاصی را انجام می‌دهد (مثل WriteLine).

کلاس (Class): بلوک سازنده شیءگرایی که شامل متدها و داده‌هاست (مثل Console).

فضای نام (Namespace): راهی برای سازماندهی انواع (Types) در سطوح بالاتر. بسیاری از کلاس‌های پرکاربرد (مثل Console) در فضای نام System قرار دارند.
مثال:

```C#System.Text برای کار با متن.

System.IO برای عملیات ورودی/خروجی.
```

بهینه‌سازی کد با using
برای جلوگیری از تکرار System.Console می‌توان از دستور using استفاده کرد:

```C#
using System;  // وارد کردن فضای نام System  
Console.WriteLine(x);  // دیگر نیازی به نوشتن System. نیست  
```
استفاده مجدد از کد با متدها
می‌توان با تعریف متدهای سطح بالا (مثل تبدیل فوت به اینچ) کد را بهینه‌تر کرد:

```C#
Console.WriteLine(FeetToInches(30));  // خروجی: 360  
Console.WriteLine(FeetToInches(100)); // خروجی: 1200  

int FeetToInches(int feet)  
{  
    int inches = feet * 12;  
    return inches;  
}  
```
بلوک عبارت: مجموعه‌ای از دستورات داخل آکولاد {}.

پارامتر ورودی و خروجی: متد FeetToInches یک پارامتر (feet) و مقدار بازگشتی (inches) دارد.

متدهای بدون ورودی/خروجی
اگر متدی ورودی نداشته باشد، از پرانتز خالی استفاده می‌کنیم.

اگر خروجی نداشته باشد، از void استفاده می‌شود:

```C#
SayHello();  

void SayHello()  
{  
    Console.WriteLine("Hello, world");  
}
```
انواع توابع در سی‌شارپ
متدها (مثل FeetToInches).

عملگرها (مثل * برای ضرب).

سایر موارد: سازنده‌ها (Constructors)، ویژگی‌ها (Properties)، رویدادها (Events)، ایندکسرها (Indexers) و فاینالایزرها (Finalizers).

### کامپایل

کامپایلر سی‌شارپ کد منبع (مجموعه‌ای از فایل‌ها با پسوند .cs) را به یک اَسمبلی (assembly) کامپایل می‌کند. یک اَسمبلی، واحد بسته‌بندی و استقرار در .NET است. یک اَسمبلی می‌تواند یک برنامه یا یک کتابخانه باشد. یک برنامه کنسول یا ویندوز معمولی دارای یک نقطه ورود است، در حالی که یک کتابخانه اینگونه نیست. هدف یک کتابخانه این است که توسط یک برنامه یا توسط کتابخانه‌های دیگر فراخوانی (ارجاع) شود. خود .NET مجموعه‌ای از کتابخانه‌ها (و همچنین یک محیط زمان اجرا) است.

هر یک از برنامه‌های بخش قبلی مستقیماً با مجموعه‌ای از دستورات (به نام دستورات سطح بالا) آغاز شدند. وجود دستورات سطح بالا به طور ضمنی یک نقطه ورود برای یک برنامه کنسول یا ویندوز ایجاد می‌کند. (بدون دستورات سطح بالا، متد Main نقطه ورود یک برنامه را نشان می‌دهد—به "انواع سفارشی" در صفحه ۳۷ مراجعه کنید.)

برخلاف .NET Framework، اَسمبلی‌های .NET 8 هرگز پسوند .exe ندارند. فایل .exe که پس از ساخت یک برنامه .NET 8 می‌بینید، یک لودر بومی (native loader) مخصوص پلتفرم است که مسئول راه‌اندازی اسمبلی .dll برنامه شما می‌باشد.

.NET 8 همچنین به شما اجازه می‌دهد یک استقرار مستقل (self-contained deployment) ایجاد کنید که شامل لودر، اسمبلی‌های شما، و بخش‌های مورد نیاز از زمان اجرای .NET باشد—همه در یک فایل .exe واحد. .NET 8 همچنین امکان کامپایل پیش از موعد (AOT) را فراهم می‌کند، که در آن فایل اجرایی حاوی کد بومی از پیش کامپایل شده برای راه‌اندازی سریع‌تر و کاهش مصرف حافظه است.

ابزار dotnet (در ویندوز dotnet.exe) به شما کمک می‌کند کدهای منبع و باینری‌های .NET را از طریق خط فرمان مدیریت کنید. می‌توانید از آن برای ساخت و اجرای برنامه خود استفاده کنید، به عنوان جایگزینی برای استفاده از یک محیط توسعه یکپارچه (IDE) مانند ویژوال استودیو یا ویژوال استودیو کد.

می‌توانید ابزار dotnet را یا با نصب .NET 8 SDK یا با نصب ویژوال استودیو به دست آورید. مکان پیش‌فرض آن در ویندوز ‎%ProgramFiles%\dotnet و در اوبونتو لینوکس ‎/usr/bin/dotnet است.
برای کامپایل یک برنامه، ابزار dotnet به یک فایل پروژه و همچنین یک یا چند فایل سی‌شارپ نیاز دارد. دستور زیر یک پروژه کنسول جدید را راه‌اندازی می‌کند (ساختار پایه آن را ایجاد می‌کند):

```c#

dotnet new Console -n MyFirstProgram
```
این دستور یک زیرپوشه به نام MyFirstProgram ایجاد می‌کند که شامل یک فایل پروژه به نام MyFirstProgram.csproj و یک فایل سی‌شارپ به نام Program.cs است که عبارت "Hello world." را چاپ می‌کند.

برای ساخت و اجرای برنامه خود، دستور زیر را از پوشه MyFirstProgram اجرا کنید:

```c#

dotnet run MyFirstProgram
```
یا، اگر فقط می‌خواهید بدون اجرا بسازید:

```c#

dotnet build MyFirstProgram.csproj
```
خروجی اسمبلی در یک زیردایرکتوری تحت bindebug نوشته خواهد شد. ما در فصل 17 به تفصیل درباره اسمبلی‌ها توضیح می‌دهیم.

## Syntax

نحو سی‌شارپ از نحو زبان‌های C و C++ الهام گرفته شده است. در این بخش، ما عناصر نحو سی‌شارپ را با استفاده از برنامه زیر توضیح می‌دهیم:

```C#

using System;
int x = 12 * 30;
Console.WriteLine (x);
```
### شناسه‌ها و کلمات کلیدی

شناسه‌ها (Identifiers) نام‌هایی هستند که برنامه‌نویسان برای کلاس‌ها، متدها، متغیرها و غیره انتخاب می‌کنند. در برنامه مثال ما، شناسه‌ها به ترتیب ظاهر شدنشان عبارتند از:

```System   x   Console   WriteLine
```
یک شناسه باید یک کلمه کامل باشد که اساساً از کاراکترهای یونیکد تشکیل شده و با یک حرف یا زیرخط شروع می‌شود. شناسه‌های سی‌شارپ به حروف حساس هستند (case sensitive). طبق قرارداد، پارامترها، متغیرهای محلی، و فیلدهای خصوصی باید به کمل کِیس (camel case) باشند (مثلاً myVariable)، و سایر شناسه‌ها باید به پاسکال کِیس (Pascal case) باشند (مثلاً MyMethod).

کلمات کلیدی (Keywords) نام‌هایی هستند که برای کامپایلر معنای خاصی دارند. در برنامه مثال ما دو کلمه کلیدی وجود دارد: using و int.

بیشتر کلمات کلیدی رزرو شده هستند، به این معنی که نمی‌توانید از آن‌ها به عنوان شناسه استفاده کنید. در اینجا لیست کامل کلمات کلیدی رزرو شده سی‌شارپ آمده است:

```abstract    do          protected     sbyte
as          double      public        sealed
base        else        readonly      short
bool        enum        record        sizeof
break       event       ref           stackalloc
byte        explicit    return        static
case        extern      float         string
catch       false       for           struct
char        finally     foreach       switch
checked     fixed       goto          this
class       if          throw         true
const       implicit    try           typeof
continue    in          uint          ulong
decimal     int         unchecked     unsafe
default     interface   ushort        using
delegate    internal    virtual       void
            is          volatile      while
            lock
            long
            namespace
            new
            null
            object
            operator
            out
            override
            params
            private
```
اگر واقعاً می‌خواهید از شناسه‌ای استفاده کنید که با یک کلمه کلیدی رزرو شده تداخل دارد، می‌توانید با استفاده از پیشوند @ این کار را انجام دهید. برای مثال:

```C#

int using = 123;      // غیرقانونی
int @using = 123;     // قانونی
```
نماد @ بخشی از خود شناسه را تشکیل نمی‌دهد. بنابراین، @myVariable همان myVariable است.

### کلمات کلیدی متنی

برخی از کلمات کلیدی متنی (contextual) هستند، به این معنی که می‌توانید از آن‌ها به عنوان شناسه نیز استفاده کنید—بدون نماد @:

```add         descending  global        notnull     remove      var
alias       dynamic     group         nuint       required    with
and         equals      init          on          select      when
ascending   file        into          or          set         where
async       from        join          orderby     unmanaged   yield
await       get         let           partial     value
by          managed     nameof
```
با کلمات کلیدی متنی، ابهام نمی‌تواند در متنی که در آن استفاده می‌شوند، ایجاد شود.

### ثابت‌ها، نشانه‌گذارها و عملگرها

ثابت‌ها (Literals) قطعات داده اولیه‌ای هستند که به صورت لغوی در برنامه جاسازی شده‌اند. ثابت‌هایی که در برنامه مثال ما استفاده کردیم ۱۲ و ۳۰ هستند.

نشانه‌گذارها (Punctuators) به جداسازی ساختار برنامه کمک می‌کنند. یک مثال سمی‌کولن است که یک دستور را به پایان می‌رساند. دستورات می‌توانند چندین خط را در بر گیرند:

```C#

Console.WriteLine
  (1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10);
```
یک عملگر (Operator) عبارت‌ها را تبدیل و ترکیب می‌کند. بیشتر عملگرها در سی‌شارپ با یک نماد نشان داده می‌شوند، مانند عملگر ضرب، *. ما عملگرها را بعداً در این فصل با جزئیات بیشتری بحث خواهیم کرد. این‌ها عملگرهایی هستند که در برنامه مثال ما استفاده کردیم:

```=  * .  ()
```
یک نقطه، عضویت چیزی را (یا نقطه اعشار را در ثابت‌های عددی) نشان می‌دهد. پرانتزها هنگام اعلان یا فراخوانی یک متد استفاده می‌شوند؛ پرانتزهای خالی زمانی استفاده می‌شوند که متد هیچ آرگومانی را نمی‌پذیرد. (پرانتزها اهداف دیگری نیز دارند که بعداً در این فصل خواهید دید.) یک علامت مساوی عمل انتساب را انجام می‌دهد. (علامت مساوی دوبل، ==، مقایسه برابری را انجام می‌دهد، همانطور که بعداً خواهید دید.)

### Comments

سی‌شارپ دو سبک مختلف مستندسازی کد منبع را ارائه می‌دهد: نظرات تک‌خطی و نظرات چندخطی. یک نظر تک‌خطی با دو اسلش رو به جلو آغاز می‌شود و تا پایان خط ادامه می‌یابد؛ برای مثال:

```C#

int x = 3;   // Comment about assigning 3 to x
```
یک نظر چندخطی با /* شروع شده و با */ به پایان می‌رسد؛ برای مثال:

```C#

int x = 3;   /* This is a comment that
                spans two lines */
```
نظرات می‌توانند شامل تگ‌های مستندسازی XML باشند، که ما در "مستندسازی XML" در صفحه ۲۷۲ توضیح می‌دهیم.

## مبانی Types

یک Type، طرح کلی (blueprint) برای یک value را تعریف می‌کند. در این مثال، ما از دو Literals از نوع int با مقادیر ۱۲ و ۳۰ استفاده می‌کنیم. همچنین یک variable از نوع int با نام x اعلان می‌کنیم:

```C#

int x = 12 * 30;
Console.WriteLine (x);
```
از آنجایی که بیشتر لیست‌های کد در این کتاب به Types از Namespace System نیاز دارند، از این پس "using System" را حذف خواهیم کرد، مگر اینکه مفهومی مرتبط با Namespaces را نشان دهیم.

یک variable نشان‌دهنده یک مکان ذخیره‌سازی است که می‌تواند در طول زمان حاوی مقادیر مختلفی باشد. در مقابل، یک Constant همیشه همان value را نمایش می‌دهد (در ادامه بیشتر در مورد آن صحبت خواهیم کرد):

```C#

const int y = 360;
```
تمام Values در C#، Instances یک Type هستند. معنای یک value و مجموعه مقادیر ممکن که یک variable می‌تواند داشته باشد، توسط Type آن تعیین می‌شود.

### نمونه‌های Predefined Type

Predefined Types انواعی هستند که به طور خاص توسط Compiler پشتیبانی می‌شوند. Type int یک Predefined Type برای نمایش مجموعه اعداد صحیح است که در ۳۲ بیت حافظه جای می‌گیرند، از 
2 
31
 
- تا 
2 
31
 
-1، و Type پیش‌فرض برای Literals عددی در این محدوده است. می‌توانید با Instances از Type int توابعی مانند عملیات حسابی را به صورت زیر انجام دهید:

```C#

int x = 12 * 30;
```
یک Predefined Type دیگر C#، string است. Type string یک توالی از Characterها را نمایش می‌دهد، مانند ".NET" یا http://oreilly.com. می‌توانید با فراخوانی توابع روی Strings با آن‌ها کار کنید، به صورت زیر:

```C#

string message = "Hello world";
string upperMessage = message.ToUpper();
Console.WriteLine (upperMessage);               // HELLO WORLD
int x = 2022;
message = message + x.ToString();
Console.WriteLine (message);                    // Hello world2022
```
در این مثال، ما x.ToString() را فراخوانی کردیم تا یک نمایش رشته‌ای از عدد صحیح x به دست آوریم. می‌توانید ToString() را روی یک variable از تقریباً هر Type فراخوانی کنید.

Type Predefined bool دقیقاً دو value ممکن دارد: true و false. Type bool معمولاً با یک if statement برای شاخه‌بندی شرطی جریان اجرا استفاده می‌شود:

```C#

bool simpleVar = false;
if (simpleVar)
  Console.WriteLine ("This will not print");
int x = 5000;
bool lessThanAMile = x < 5280;
if (lessThanAMile)
  Console.WriteLine ("This will print");
```
### Custom Types

در C#، Predefined Types (که به آن‌ها Built-in Types نیز گفته می‌شود) با یک C# Keyword شناخته می‌شوند. Namespace System در .NET حاوی بسیاری از Types مهم است که توسط C# Predefined نیستند (مثلاً DateTime).

همانطور که می‌توانیم Methodهای خودمان را بنویسیم، می‌توانیم Types خودمان را نیز بنویسیم. در مثال بعدی، ما یک Custom Type به نام UnitConverter تعریف می‌کنیم—یک Class که به عنوان طرح کلی برای تبدیل واحدها عمل می‌کند:

```C#

UnitConverter feetToInchesConverter = new UnitConverter (12);
UnitConverter milesToFeetConverter  = new UnitConverter (5280);
Console.WriteLine (feetToInchesConverter.Convert(30));    // 360
Console.WriteLine (feetToInchesConverter.Convert(100));   // 1200
Console.WriteLine (feetToInchesConverter.Convert(
                   milesToFeetConverter.Convert(1)));     // 63360
public class UnitConverter
{
 int ratio;                              // Field
 public UnitConverter (int unitRatio)    // Constructor
 {
 ratio = unitRatio;
 } 
public int Convert (int unit)           // Method
{
 return unit * ratio;
 } 
}
```
در این مثال، تعریف Class ما در همان فایل دستورات سطح بالای ما ظاهر می‌شود. این قانونی است—تا زمانی که دستورات سطح بالا ابتدا ظاهر شوند—و هنگام نوشتن برنامه‌های آزمایشی کوچک قابل قبول است. در برنامه‌های بزرگ‌تر، رویکرد استاندارد این است که تعریف Class را در یک فایل جداگانه مانند UnitConverter.cs قرار دهیم.

### اعضای یک نوع (Members of a type)
یک نوع (Type) شامل اعضای داده‌ای (data members) و اعضای تابعی (function members) است.
عضو داده‌ای کلاس UnitConverter فیلدی به نام ratio است.
اعضای تابعی آن نیز شامل متد Convert و سازنده‌ی (constructor) کلاس UnitConverter می‌باشند.

### تقارن بین نوع‌های از پیش تعریف‌شده و نوع‌های سفارشی
(Symmetry of predefined types and custom types)

یکی از جنبه‌های زیبای #C این است که بین نوع‌های از پیش تعریف‌شده (مثل int) و نوع‌هایی که خودمان تعریف می‌کنیم (custom types) تفاوت چندانی وجود ندارد.

برای مثال، نوع int به عنوان یک الگو برای عددهای صحیح عمل می‌کند.
این نوع داده ذخیره می‌کند — یعنی ۳۲ بیت اطلاعات — و اعضای تابعی دارد که از این داده استفاده می‌کنند، مانند متد ToString.

به‌طور مشابه، نوع سفارشی UnitConverter که خودمان تعریف کردیم، الگوی تبدیل واحد است.
این نوع هم داده‌ای نگه می‌دارد — نسبت یا همان ratio — و اعضای تابعی‌ای دارد که از این داده استفاده می‌کنند.

### سازنده‌ها و ایجاد نمونه (Constructors and instantiation)
داده‌ها از طریق ایجاد نمونه‌ای از یک نوع (instantiating a type) ساخته می‌شوند.

نوع‌های از پیش تعریف‌شده را می‌توان تنها با استفاده از لیترال‌ها (literals) ایجاد کرد؛ مانند 12 یا "Hello world".

اما برای ایجاد نمونه‌ای از یک نوع سفارشی، باید از عملگر new استفاده کنیم.
مثلاً این دستور یک نمونه از کلاس UnitConverter ایجاد می‌کند:

```c#
UnitConverter feetToInchesConverter = new UnitConverter(12);
```
بلافاصله بعد از اینکه new یک شیء را ایجاد کرد، سازنده‌ی آن شیء فراخوانی می‌شود تا مقداردهی اولیه انجام شود.

سازنده‌ها مشابه متدها تعریف می‌شوند، با این تفاوت که نام متد همان نام کلاس است و نوع بازگشتی (return type) ندارد:

```c#
public UnitConverter(int unitRatio) {
    ratio = unitRatio;
}
```
### اعضای نمونه‌ای در برابر اعضای ایستا
(Instance versus static members)

اعضای داده‌ای و تابعی که روی یک نمونه از نوع عمل می‌کنند، اعضای نمونه‌ای (instance members) نامیده می‌شوند.

برای مثال، متد Convert در کلاس UnitConverter و متد ToString در int اعضای نمونه‌ای هستند.
به‌طور پیش‌فرض، تمام اعضا، نمونه‌ای هستند.

اما اگر عضوهایی وجود داشته باشند که مستقیماً به نمونه‌ای نیاز نداشته باشند، می‌توان آن‌ها را به عنوان static (ایستا) علامت‌گذاری کرد.

برای استفاده از عضو ایستا از بیرون، باید نام نوع را مشخص کنیم نه نام نمونه.

مثال: متد WriteLine در کلاس Console یک متد ایستا است، بنابراین آن را این‌گونه صدا می‌زنیم:

```c#
Console.WriteLine();
```
نه به صورت:

```c#
new Console().WriteLine(); // نادرست
```
در واقع، کلاس Console به‌طور کامل به‌صورت static تعریف شده است.
یعنی تمام اعضای آن ایستا هستند و شما هرگز نمی‌توانید یک شیء از Console بسازید.

مثال از تفاوت عضو نمونه‌ای و ایستا:
در کد زیر، فیلد Name مربوط به نمونه خاصی از پاندا است،
در حالی که فیلد Population مربوط به تمام پاندای ساخته‌شده است:

```c#
Panda p1 = new Panda("Pan Dee");
Panda p2 = new Panda("Pan Dah");

Console.WriteLine(p1.Name);           // Pan Dee
Console.WriteLine(p2.Name);           // Pan Dah
Console.WriteLine(Panda.Population);  // 2
```
کلاس Panda به صورت زیر تعریف شده:

```c#
public class Panda
{
    public string Name;             // فیلد نمونه‌ای
    public static int Population;   // فیلد ایستا (مشترک بین همه)

    public Panda(string n)          // سازنده
    {
        Name = n;
        Population = Population + 1;
    }
}
```
اگر سعی کنیم p1.Population یا Panda.Name را فراخوانی کنیم، خطای زمان کامپایل خواهیم گرفت،
چون هر کدام فقط به شیوه خاص خود قابل دسترسی هستند.

### کلیدواژه‌ی public
کلیدواژه‌ی public اعضا را برای سایر کلاس‌ها قابل مشاهده و دسترسی می‌کند.

اگر در کلاس Panda، فیلد Name را به صورت public تعریف نکنیم، به صورت پیش‌فرض private خواهد بود
و از بیرون کلاس قابل دسترسی نخواهد بود.

استفاده از public یعنی:

«من می‌خوام این عضو برای سایر کلاس‌ها قابل دیدن و استفاده باشه؛ باقی چیزها جزئیات داخلی خودمن.»

در مفاهیم شی‌ء‌گرایی (OOP) می‌گوییم اعضای public، اعضای private را کپسوله‌سازی (encapsulate) می‌کنند.

### تعریف فضای نام (Defining namespaces)
در برنامه‌های بزرگ، منطقیه که کلاس‌ها رو داخل namespace‌های مشخص قرار بدیم.

مثال:

```c#
using System;
using Animals;

Panda p = new Panda("Pan Dee");
Console.WriteLine(p.Name);

namespace Animals
{
    public class Panda
    {
        ...
    }
}
```
در اینجا، ما فضای نام Animals رو وارد کردیم، تا نیازی به استفاده کامل از اسم نباشه.

اگه اون using رو نمی‌نوشتیم، باید این‌طور می‌نوشتیم:

```c#
Animals.Panda p = new Animals.Panda("Pan Dee");
```
در ادامه‌ی فصل، بحث فضای نام به‌صورت کامل در صفحه ۹۵ بررسی می‌شه.

### تعریف متد Main
(Defining a Main method)

تا اینجای کار، تمام مثال‌های ما از دستورات سطح بالا (top-level statements) استفاده می‌کردند —
ویژگی‌ای که در C# 9 معرفی شد.

اما بدون استفاده از دستورات سطح بالا، ساختار یک برنامه ساده کنسولی یا ویندوزی به این صورت خواهد بود:

```c#
using System;

class Program
{
    static void Main()   // نقطه ورود برنامه
    {
        int x = 12 * 30;
        Console.WriteLine(x);
    }
}
```
در صورت نبود دستورات سطح بالا، کامپایلر #C به دنبال یک متد ایستا به نام Main می‌گردد
که نقش نقطه‌ی ورود (entry point) برنامه را ایفا می‌کند.

این متد Main می‌تواند در هر کلاسی تعریف شود، اما فقط یک Main در برنامه مجاز است.

برگشت مقدار از Main (اختیاری)
متد Main می‌تواند به‌جای void، یک عدد صحیح (int) برگرداند.
این عدد می‌تواند به محیط اجرایی برگردانده شود تا وضعیت اجرای برنامه مشخص شود:

اگر مقدار بازگشتی 0 باشد، یعنی اجرا موفق بوده؛

اگر مقدار بازگشتی غیراز صفر باشد، معمولاً نشان‌دهنده‌ی یک خطا است.

دریافت آرگومان‌های خط فرمان (Command Line Arguments)
متد Main می‌تواند آرایه‌ای از رشته‌ها (string[]) به عنوان ورودی بگیرد.
این آرایه، شامل آرگومان‌هایی است که هنگام اجرای فایل اجرایی به برنامه پاس داده شده‌اند.

مثال:

```c#
static int Main(string[] args)
{
    // استفاده از args[0] و غیره
}
```
#### توضیحی درباره‌ی آرایه‌ها
یک آرایه (Array) مثل string[] نشان‌دهنده‌ی تعدادی مقدار از یک نوع خاص است.
برای تعریف آرایه، از علامت براکت [] بعد از نوع داده استفاده می‌کنیم.

آرایه‌ها به طور کامل در بخش "آرایه‌ها" در صفحه ۶۱ توضیح داده می‌شوند.

#### پشتیبانی از برنامه‌نویسی ناهمگام (Async Main)
متد Main همچنین می‌تواند به صورت async (ناهمگام) تعریف شود
و مقدار بازگشتی آن می‌تواند از نوع Task یا Task<int> باشد.

این قابلیت، به برنامه‌نویسی ناهمگام (asynchronous programming) کمک می‌کند
و به‌طور کامل در فصل ۱۴ بررسی خواهد شد.

### دستورات سطح بالا (Top-Level Statements)
دستورات سطح بالا (که در #C نسخه ۹ معرفی شدند) به شما اجازه می‌دهند تا از نوشتن متد Main به صورت ایستا و کلاس نگهدارنده‌ی آن صرف‌نظر کنید.
یک فایل که از دستورات سطح بالا استفاده می‌کند، شامل سه بخش به ترتیب زیر است:

(اختیاری) دستورات using

مجموعه‌ای از دستورات که می‌تواند شامل تعریف متدها نیز باشد (اختیاری)

(اختیاری) تعریف نوع‌ها و فضای نام‌ها

مثال:

```c#
using System;                           // بخش ۱
Console.WriteLine("Hello, world");      // بخش ۲
void SomeMethod1() { ... }              // بخش ۲
Console.WriteLine("Hello again!");      // بخش ۲
void SomeMethod2() { ... }              // بخش ۲
class SomeClass { ... }                 // بخش ۳
namespace SomeNamespace { ... }         // بخش ۳
```
از آنجا که CLR (Common Language Runtime) به‌طور صریح از دستورات سطح بالا پشتیبانی نمی‌کند،
کامپایلر کد شما را به چیزی مانند زیر ترجمه می‌کند:

```c#
using System;                           // بخش ۱

static class Program$   // نام ویژه‌ای که توسط کامپایلر تولید شده
{
    static void Main$ (string[] args)   // نام تولیدشده توسط کامپایلر
    {
        Console.WriteLine("Hello, world");     // بخش ۲
        void SomeMethod1() { ... }             // بخش ۲
        Console.WriteLine("Hello again!");     // بخش ۲
        void SomeMethod2() { ... }             // بخش ۲
    }
}

class SomeClass { ... }                 // بخش ۳
namespace SomeNamespace { ... }         // بخش ۳
```
توجه کنید که تمام محتوای بخش ۲ درون متد Main قرار می‌گیرد.
این یعنی SomeMethod1 و SomeMethod2 به‌عنوان متدهای محلی (local methods) عمل می‌کنند.

ما در بخش «متدهای محلی» در صفحه ۱۰۶ به‌طور کامل در مورد این موضوع صحبت خواهیم کرد.
مهم‌ترین نکته این است که متدهای محلی (مگر اینکه به صورت static تعریف شده باشند)
می‌توانند به متغیرهایی که درون متد احاطه‌کننده تعریف شده‌اند، دسترسی داشته باشند:

```c#
int x = 3;
LocalMethod();
void LocalMethod() { Console.WriteLine(x); }   // می‌توانیم به x دسترسی داشته باشیم
```
یک نتیجه دیگر این است که متدهای سطح بالا قابل دسترسی از سایر کلاس‌ها یا نوع‌ها نیستند.

دستورات سطح بالا می‌توانند به‌صورت اختیاری یک مقدار عدد صحیح (int) به فراخواننده بازگردانند
و به یک متغیر خاص از نوع string[] به نام args دسترسی داشته باشند،
که معادل آرگومان‌های خط فرمانی است که توسط فراخواننده به برنامه داده شده‌اند.

از آنجا که فقط یک نقطه ورود برای برنامه می‌تواند وجود داشته باشد،
در یک پروژه #C حداکثر فقط یک فایل می‌تواند شامل دستورات سطح بالا باشد.

### نوع‌ها و تبدیل‌ها
(Types and Conversions)

زبان C# می‌تواند بین نمونه‌هایی از نوع‌های سازگار، تبدیل انجام دهد.
هر تبدیل، همیشه یک مقدار جدید از روی یک مقدار موجود می‌سازد.

تبدیل‌ها می‌توانند ضمنی (implicit) یا صریح (explicit) باشند:

تبدیل ضمنی به صورت خودکار انجام می‌شود.

تبدیل صریح نیاز به عملگر تبدیل (cast) دارد.

در مثال زیر، ما به‌طور ضمنی یک int را به long (که دو برابر ظرفیت بیتی دارد) تبدیل می‌کنیم،
و سپس به‌طور صریح یک int را به short (که نصف ظرفیت بیتی دارد) تبدیل می‌کنیم:

```c#
int x = 12345;       // int یک عدد صحیح ۳۲ بیتی است
long y = x;          // تبدیل ضمنی به عدد صحیح ۶۴ بیتی
short z = (short)x;  // تبدیل صریح به عدد صحیح ۱۶ بیتی
```
تبدیل‌های ضمنی در صورتی مجاز هستند که هر دو شرط زیر برقرار باشد:

کامپایلر می‌تواند تضمین کند که تبدیل همیشه موفق خواهد بود.

هیچ اطلاعاتی در طول تبدیل از دست نمی‌رود.¹

در مقابل، تبدیل‌های صریح زمانی مورد نیاز هستند که یکی از شرایط زیر وجود داشته باشد:

کامپایلر نمی‌تواند تضمین کند که تبدیل همیشه موفق خواهد بود.

ممکن است اطلاعاتی در طول تبدیل از دست برود.

اگر کامپایلر تشخیص دهد که یک تبدیل همیشه شکست می‌خورد،
هر دو نوع تبدیل (ضمنی و صریح) ممنوع خواهند بود.

تبدیل‌هایی که شامل نوع‌های generic هستند هم ممکن است تحت شرایط خاصی شکست بخورند
— به بخش «Type Parameters and Conversions» در صفحه ۱۶۶ مراجعه کنید.

تبدیل‌های عددی که در بالا دیدیم، به‌صورت ذاتی در زبان C# تعریف شده‌اند.
C# همچنین از موارد زیر پشتیبانی می‌کند:

تبدیل ارجاعی (reference conversions)

تبدیل باکسینگ (boxing conversions)
(در فصل ۳ توضیح داده می‌شوند)

و همچنین تبدیل‌های سفارشی (custom conversions)
(در بخش «Operator Overloading» در صفحه ۲۵۶ توضیح داده شده)

کامپایلر، قوانین بالا را برای تبدیل‌های سفارشی تضمین نمی‌کند؛
پس ممکن است نوع‌هایی که بد طراحی شده‌اند، رفتار غیرمنتظره‌ای داشته باشند.

¹ یک نکته‌ی جزئی: مقادیر long بسیار بزرگ، هنگام تبدیل به double، ممکن است کمی دقت (precision) را از دست بدهند.

### نوع‌های مقداری در برابر نوع‌های ارجاعی
(Value Types Versus Reference Types)

تمام نوع‌های C# در یکی از دسته‌های زیر قرار می‌گیرند:

نوع‌های مقداری (Value types)

نوع‌های ارجاعی (Reference types)

پارامترهای نوعی (Generic type parameters)

نوع‌های اشاره‌گر (Pointer types)

در این بخش، ما درباره‌ی نوع‌های مقداری و نوع‌های ارجاعی صحبت می‌کنیم.

پارامترهای نوعی در بخش «Generics» در صفحه ۱۵۹
و نوع‌های اشاره‌گر در بخش «Unsafe Code and Pointers» در صفحه ۲۶۳ پوشش داده می‌شوند.

نوع‌های مقداری شامل بیشتر نوع‌های داخلی هستند؛
به‌ویژه:

تمام نوع‌های عددی

نوع char

نوع bool

و همچنین نوع‌های سفارشی مانند struct و enum

نوع‌های ارجاعی شامل موارد زیر می‌شوند:

تمام کلاس‌ها (class)

آرایه‌ها (array)

نماینده‌ها (delegate)

رابط‌ها (interface)
(شامل نوع string که به صورت داخلی تعریف شده نیز می‌شود)

تفاوت اصلی بین نوع‌های مقداری و ارجاعی، نحوه‌ی مدیریت آن‌ها در حافظه است.
### نوع‌های مقداری (Value Types)
محتوای یک متغیر یا ثابت از نوع مقداری، صرفاً یک مقدار است.
برای مثال، محتوای نوع int (که یکی از نوع‌های داخلی مقداری است)،
فقط شامل ۳۲ بیت داده است.

می‌توانی یک نوع مقداری سفارشی را با استفاده از کلیدواژه‌ی struct تعریف کنی
(به شکل زیر که در شکل ۲-۱ نمایش داده شده):


```c#
public struct Point { public int X; public int Y; }
```
یا به‌شکل مختصرتر:

```c#
public struct Point { public int X, Y; }
```
📌 شکل ۲-۱. نمونه‌ای از یک نوع مقداری در حافظه
<div align="center">
  
![Conventions-UsedThis-Book](../../assets/image/02/Figure-2-1.png) 
  
</div>

وقتی یک نمونه از نوع مقداری را به متغیر دیگری اختصاص می‌دهی (assign)،
تمام مقدار آن کپی می‌شود. برای مثال:

```C#
Point p1 = new Point();
p1.X = 7;

Point p2 = p1;             // انتساب باعث کپی کامل می‌شود

Console.WriteLine(p1.X);   // 7
Console.WriteLine(p2.X);   // 7

p1.X = 9;                  // تغییر مقدار p1.X

Console.WriteLine(p1.X);   // 9
Console.WriteLine(p2.X);   // 7
```
📌 شکل ۲-۲ نشان می‌دهد که p1 و p2 فضای ذخیره‌سازی مستقلی دارند.
<div align="center">
    
![Conventions-UsedThis-Book](../../assets/image/02/Figure-2-2.png) 
  
</div>

📌 یعنی هر کدام در حافظه جداگانه نگهداری می‌شوند و تغییر یکی روی دیگری اثری ندارد.

### نوع‌های ارجاعی (Reference Types)
یک نوع ارجاعی از نوع‌های مقداری پیچیده‌تر است و از دو بخش تشکیل شده:

یک شیء (object)

و یک ارجاع (reference) به آن شیء

محتوای یک متغیر یا ثابت از نوع ارجاعی، ارجاعی به یک شیء است که آن مقدار را در خود دارد.

در اینجا، نوع Point را که قبلاً به‌صورت struct داشتیم، این بار به‌صورت class بازنویسی می‌کنیم:
(در شکل ۲-۳ نشان داده شده)

```c#
public class Point { public int X, Y; }
```
📌 شکل ۲-۳. نمونه‌ای از نوع ارجاعی در حافظه

<div align="center">
  
![Conventions-UsedThis-Book](../../assets/image/02/Figure-2-3.png) 
  
</div>

زمانی که یک متغیر از نوع ارجاعی را به متغیر دیگری اختصاص می‌دهیم،
فقط ارجاع کپی می‌شود، نه خود شیء.

این موضوع باعث می‌شود که چندین متغیر به یک شیء واحد اشاره کنند —
چیزی که در نوع‌های مقداری امکان‌پذیر نیست.

اگر همان مثال قبلی را با Point به‌صورت class اجرا کنیم، عملیات روی p1 روی p2 نیز اثر می‌گذارد:

```c#
Point p1 = new Point();
p1.X = 7;

Point p2 = p1;             // کپی شدن ارجاع (نه شیء)

Console.WriteLine(p1.X);   // 7
Console.WriteLine(p2.X);   // 7

p1.X = 9;                  // تغییر مقدار X از طریق p1

Console.WriteLine(p1.X);   // 9
Console.WriteLine(p2.X);   // 9
```
📌 شکل ۲-۴ نشان می‌دهد که p1 و p2 دو ارجاع هستند که به یک شیء مشترک اشاره می‌کنند.

<div align="center">
  
![Conventions-UsedThis-Book](../../assets/image/02/Figure-2-4.png) 
  
</div>

📌 در نتیجه تغییر یکی، روی دیگری هم تأثیر دارد.

### Null

یک Reference را می‌توان با Literal null مقداردهی کرد، که نشان می‌دهد Reference به هیچ Objectی اشاره نمی‌کند:

```C#

Point p = null;
Console.WriteLine (p == null);   // True
// The following line generates a runtime error
// (a NullReferenceException is thrown):
Console.WriteLine (p.X);
class Point {...}
```
در "Nullable Reference Types" در صفحه ۲۱۵، ما ویژگی‌ای از C# را شرح می‌دهیم که به کاهش خطاهای تصادفی NullReferenceException کمک می‌کند.

در مقابل، یک Value Type به طور معمول نمی‌تواند مقدار null داشته باشد:

```C#

Point p = null;  // Compile-time error
int x = null;    // Compile-time error
struct Point {...}
```
C# همچنین ساختاری به نام Nullable Value Types برای نمایش nullهای Value Type دارد. برای اطلاعات بیشتر، به "Nullable Value Types" در صفحه ۲۱۰ مراجعه کنید.

### Storage Overhead

Instances انواع Value Type دقیقاً حافظه مورد نیاز برای ذخیره Fields خود را اشغال می‌کنند. در این مثال، Point، ۸ بایت حافظه می‌گیرد:

```C#

struct Point
{
  int x;  // 4 bytes
  int y;  // 4 bytes
}
```
از نظر فنی، CLR Fields را در Type در آدرسی قرار می‌دهد که مضربی از اندازه Fields است (حداکثر ۸ بایت). بنابراین، مثال زیر در واقع ۱۶ بایت حافظه مصرف می‌کند (با ۷ بایت پس از Field اول "تلف شده"):

```C#

struct A { byte b; long l; }
```
می‌توانید این رفتار را با اعمال Attribute StructLayout نادیده بگیرید (به "Mapping a Struct to Unmanaged Memory" در صفحه ۹۹۷ مراجعه کنید).

Reference Types نیاز به تخصیص‌های جداگانه حافظه برای Reference و Object دارند. Object به اندازه Fields خود به علاوه سربار اداری اضافی، بایت مصرف می‌کند. سربار دقیقاً به طور ذاتی برای پیاده‌سازی .NET runtime خصوصی است، اما حداقل، این سربار ۸ بایت است، که برای ذخیره یک کلید به Type Object و همچنین اطلاعات موقت مانند وضعیت Lock آن برای Multithreading و یک پرچم برای نشان دادن اینکه آیا از حرکت توسط Garbage Collector ثابت شده است، استفاده می‌شود. هر Reference به یک Object به ۴ یا ۸ بایت اضافی نیاز دارد، بسته به اینکه .NET runtime روی پلتفرم ۳۲ یا ۶۴ بیتی در حال اجرا باشد.

### Predefined Type Taxonomy

Predefined Types در C# به شرح زیر هستند:

* Value Types

     * Numeric

           Signed integer (sbyte, short, int, long)

           Unsigned integer (byte, ushort, uint, ulong)

           Real number (float, double, decimal)

    * Logical (bool)

    * Character (char)

* Reference Types

    * String (string)

    * Object (object)

Predefined Types در C# در واقع Alias برای .NET Types در Namespace System هستند. تنها یک تفاوت Syntactic بین این دو دستور وجود دارد:

```C#

int i = 5;
System.Int32 i = 5;
```
مجموعه Predefined Value Types به استثنای decimal به عنوان Primitive Types در CLR شناخته می‌شوند. Primitive Types به این دلیل نام‌گذاری شده‌اند که مستقیماً از طریق Instructions در کد Compile شده پشتیبانی می‌شوند، و این معمولاً به پشتیبانی مستقیم در Processor زیربنایی ترجمه می‌شود؛ برای مثال:

```C#

                   // Underlying hexadecimal representation
int i = 7;         // 0x7
bool b = true;     // 0x1
char c = 'A';      // 0x41
float f = 0.5f;    // uses IEEE floating-point encoding
```
Types System.IntPtr و System.UIntPtr نیز Primitive هستند (به Chapter 24 مراجعه کنید).

Numeric Types

C# دارای Predefined Numeric Types است که در Table 2-1 نشان داده شده‌اند.

Table 2-1. Predefined numeric types in C#

<div align="center">
  
![Conventions-UsedThis-Book](../../assets/image/02/Table-2-1.png) 
  
</div>
از بین Integral Types، int و long، First-class Citizens محسوب می‌شوند و مورد توجه C# و Runtime هستند. سایر Integral Types معمولاً برای Interoperability یا زمانی که کارایی فضا (space efficiency) در اولویت است، استفاده می‌شوند. Native-sized Integer Types یعنی nint و nuint، بیشتر در کار با Pointers مفید هستند، بنابراین این‌ها را در یک Chapter بعدی توضیح خواهیم داد (به "Native-Sized Integers" در صفحه ۲۶۶ مراجعه کنید).


از بین Real Number Types، float و double را Floating-Point Types2 می‌نامند و معمولاً برای محاسبات علمی و گرافیکی استفاده می‌شوند. Type decimal معمولاً برای محاسبات مالی به کار می‌رود، که در آن‌ها محاسبات با دقت Base-10 و دقت بالا مورد نیاز است.

.NET این لیست را با چندین Specialized Numeric Type تکمیل می‌کند، از جمله Int128 و UInt128 برای ۱۲۸-bit Signed و Unsigned Integers، BigInteger برای Integers با اندازه‌های دلخواه بزرگ، و Half برای ۱۶-bit Floating Point Numbers. Half عمدتاً برای Interoperability با Processors کارت گرافیک در نظر گرفته شده است و در بیشتر CPUs پشتیبانی Native ندارد، که float و double را به گزینه‌های بهتری برای استفاده عمومی تبدیل می‌کند.

### Numeric Literals

Literals از نوع Integral می‌توانند از Decimal یا Hexadecimal Notation استفاده کنند؛ Hexadecimal با پیشوند 0x نشان داده می‌شود. برای مثال:

```C#

int x = 127;
long y = 0x7F;
```
می‌توانید یک Underscore را در هر کجای یک Numeric Literal قرار دهید تا خواناتر شود:

```C#

int million = 1_000_000;
```
می‌توانید اعداد را به صورت Binary با پیشوند 0b مشخص کنید:

```C#

var b = 0b1010_1011_1100_1101_1110_1111;
Real Literals می‌توانند از Decimal و/یا Exponential Notation استفاده کنند:
```
```C#

double d = 1.5;
double million = 1E06;
```
### Numeric Literal Type Inference


به طور پیش‌فرض، Compiler یک Numeric Literal را به صورت double یا یک Integral Type استنباط می‌کند:

* اگر Literal شامل یک Decimal Point یا نماد Exponential (E) باشد، یک double است.

* در غیر این صورت، Type Literal اولین Type در این لیست است که می‌تواند Value Literal را در خود جای دهد: int, uint, long, و ulong.

برای مثال:

 از نظر فنی، decimal نیز یک Floating-Point Type است، اگرچه در Specification زبان C# به این نام از آن یاد نمی‌شود.


```C#

Console.WriteLine (        1.0.GetType());  // Double  (double)
Console.WriteLine (       1E06.GetType());  // Double  (double)
Console.WriteLine (          1.GetType());  // Int32   (int)
Console.WriteLine ( 0xF0000000.GetType());  // UInt32  (uint)
Console.WriteLine (0x100000000.GetType());  // Int64   (long)
```

### پسوندهای Numeric

Numeric Suffixes به طور صریح Type یک Literal را تعریف می‌کنند. Suffixes می‌توانند حروف کوچک یا بزرگ باشند، و به شرح زیر هستند:

<div align="center">
  
![Conventions-UsedThis-Book](../../assets/image/02/Table-2-2.png) 
  
</div>
پسوندهای U و L به ندرت ضروری هستند، زیرا Types uint، long و ulong تقریباً همیشه می‌توانند از int استنباط شوند یا به طور Implicit به آن تبدیل شوند:

```C#

long i = 5;     // Implicit lossless conversion from int literal to long
```
پسوند D از نظر فنی اضافی است، زیرا تمام Literals دارای Decimal Point به صورت double استنباط می‌شوند. و شما همیشه می‌توانید یک Decimal Point به یک Numeric Literal اضافه کنید:

```C#

double x = 4.0;
```
پسوندهای F و M مفیدترین هستند و همیشه باید هنگام مشخص کردن Literals از نوع float یا decimal اعمال شوند. بدون پسوند F، خط زیر کامپایل نمی‌شود، زیرا 4.5 به عنوان Type double استنباط می‌شود، که هیچ Implicit Conversion به float ندارد:

```C#

float f = 4.5F;
```
همین اصل برای یک decimal Literal نیز صادق است:

```C#

decimal d = -1.23M;     // Will not compile without the M suffix.
```
ما Semantics مربوط به Numeric Conversions را با جزئیات در بخش بعدی شرح می‌دهیم.

## Numeric Conversions

### تبدیل بین Integral Types

Integral Type Conversions زمانی Implicit هستند که Type مقصد بتواند هر Value ممکن از Type منبع را نمایش دهد. در غیر این صورت، یک Explicit Conversion مورد نیاز است؛ برای مثال:

```C#

int x = 12345;       // int is a 32-bit integer
long y = x;          // Implicit conversion to 64-bit integral type
short z = (short)x;  // Explicit conversion to 16-bit integral type
```
### تبدیل بین Floating-Point Types

یک float می‌تواند به طور Implicit به یک double تبدیل شود، با توجه به اینکه یک double می‌تواند هر Value ممکن از یک float را نمایش دهد. تبدیل معکوس باید Explicit باشد.

### تبدیل بین Floating-Point و Integral Types

تمام Integral Types می‌توانند به طور Implicit به تمام Floating-Point Types تبدیل شوند:

```C#

int i = 1;
float f = i;
```
تبدیل معکوس باید Explicit باشد:

```C#
int i2 = (int)f;
```
هنگامی که از یک Floating-Point Number به یک Integral Type Cast می‌کنید، هر بخش کسری Truncated می‌شود؛ هیچ Rounding انجام نمی‌شود. Static Class System.Convert متدهایی را فراهم می‌کند که هنگام تبدیل بین انواع Numeric مختلف Rounding را انجام می‌دهند (به Chapter 6 مراجعه کنید).

تبدیل Implicit یک Integral Type بزرگ به یک Floating-Point Type، Magnitude را حفظ می‌کند اما گاهی اوقات می‌تواند Precision را از دست بدهد. این به این دلیل است که Floating-Point Types همیشه Magnitude بیشتری نسبت به Integral Types دارند اما می‌توانند Precision کمتری داشته باشند. بازنویسی مثال ما با یک عدد بزرگ‌تر این را نشان می‌دهد:

```C#

int i1 = 100000001;
float f = i1;          // Magnitude preserved, precision lost
int i2 = (int)f;       // 100000000
```
### تبدیل‌های Decimal

تمام Integral Types می‌توانند به طور Implicit به Type decimal تبدیل شوند، با توجه به اینکه یک decimal می‌تواند هر C# Integral-Type Value ممکن را نمایش دهد. تمام Numeric Conversions دیگر به و از یک Type decimal باید Explicit باشند، زیرا آن‌ها امکان خارج از محدوده بودن Value یا از دست رفتن Precision را معرفی می‌کنند.

## Arithmetic Operators

Arithmetic Operators (+, -, *, /, %) برای تمام Numeric Types به جز Integral Types 8 و 16 بیتی تعریف شده‌اند:

+ Addition

- Subtraction

* Multiplication

/ Division

% Remainder after division

## Increment و Decrement Operators

Increment و Decrement Operators (به ترتیب ++، --) Numeric Types را به اندازه ۱ واحد افزایش و کاهش می‌دهند. Operator می‌تواند هم بعد و هم قبل از Variable قرار گیرد، بسته به اینکه Value آن را قبل یا بعد از Increment/Decrement می‌خواهید؛ برای مثال:

```C#

int x = 0, y = 0;
Console.WriteLine (x++);   // Outputs 0; x is now 1
Console.WriteLine (++y);   // Outputs 1; y is now 1
```
## عملیات تخصصی بر روی Integral Types

Integral Types عبارتند از int، uint، long، ulong، short، ushort، byte و sbyte.

### Division

عملیات Division بر روی Integral Types همیشه Remainder را حذف می‌کنند (به سمت صفر Round می‌کنند). تقسیم بر یک Variable که Value آن صفر است، یک Runtime Error (DivideByZeroException) ایجاد می‌کند:

```C#

int a = 2 / 3;      // 0
int b = 0;
int c = 5 / b;      // throws DivideByZeroException
```
تقسیم بر Literal یا Constant 0 یک Compile-Time Error ایجاد می‌کند.

### Overflow

در Runtime، عملیات Arithmetic بر روی Integral Types می‌توانند Overflow کنند. به طور پیش‌فرض، این اتفاق به طور Silent رخ می‌دهد—هیچ Exceptionی پرتاب نمی‌شود، و نتیجه رفتار "wraparound" را نشان می‌دهد، گویی که محاسبه بر روی یک Integer Type بزرگ‌تر انجام شده و Bitهای Significant اضافی دور ریخته شده‌اند. برای مثال، کاهش حداقل Value ممکن int منجر به حداکثر Value ممکن int می‌شود:

``` C#

int a = int.MinValue;
a--;
Console.WriteLine (a == int.MaxValue); // True
```
### Overflow Check Operators


Operator checked به Runtime دستور می‌دهد که به جای Overflow Silent، یک OverflowException ایجاد کند، زمانی که یک Integral-Type Expression یا Statement از محدودیت‌های Arithmetic آن Type فراتر رود. Operator checked بر Expressions با ++، --، +، - (Binary و Unary)، *، /، و Explicit Conversion Operators بین Integral Types تأثیر می‌گذارد. بررسی Overflow هزینه Performance کمی دارد.

Operator checked بر Types double و float (که به Values "Infinite" خاص Overflow می‌کنند، همانطور که به زودی خواهید دید) و بر Type decimal (که همیشه checked است) تأثیری ندارد.


می‌توانید checked را هم در اطراف یک Expression و هم در اطراف یک Statement Block استفاده کنید:

```C#

int a = 1000000;
int b = 1000000;
int c = checked (a * b);      // Checks just the expression.
checked                           // Checks all expressions
{                                      // in statement block
 ...                                
  c = a * b;
  ...
}
```
                      
                           .
می‌توانید بررسی Arithmetic Overflow را به طور پیش‌فرض برای تمام Expressions در یک برنامه با انتخاب گزینه "checked" در سطح Project (در Visual Studio، به Advanced Build Settings بروید) فعال کنید. اگر سپس نیاز به غیرفعال کردن بررسی Overflow فقط برای Expressions یا Statements خاصی دارید، می‌توانید این کار را با Operator unchecked انجام دهید. برای مثال، کد زیر Exception پرتاب نخواهد کرد—حتی اگر گزینه "checked" Project انتخاب شده باشد:

```C#

int x = int.MaxValue;
int y = unchecked (x + 1);
unchecked { int z = x + 1; }
```
### بررسی Overflow برای Constant Expressions

صرف‌نظر از تنظیمات "checked" در Project، Expressions که در Compile Time ارزیابی می‌شوند، همیشه Overflow-checked هستند—مگر اینکه از Operator unchecked استفاده کنید:

```C#

int x = int.MaxValue + 1;               // Compile-time error
int y = unchecked (int.MaxValue + 1);   // No errors
Bitwise Operators
```

### Bitwise Operators 
C# از Bitwise Operators زیر پشتیبانی می‌کند:

![Conventions-UsedThis-Book](../../assets/image/02/Table-2-3.png) 

عملگر shift-right (>>) هنگام کار با signed integers، high-order bit را تکرار می‌کند، در حالی که عملگر unsigned shift-right (>>>) این کار را نمی‌کند.

عملیات bitwise اضافی از طریق یک class به نام BitOperations در namespace System.Numerics در دسترس هستند (برای جزئیات بیشتر به "BitOperations" در صفحه ۳۴۰ مراجعه کنید).

### ۸  و ۱۶-Bit Integral Types

Integral Types هشت و شانزده بیتی عبارتند از byte, sbyte, short, و ushort. این types فاقد arithmetic operators خود هستند، بنابراین C# به صورت implicitly آن‌ها را در صورت نیاز به types بزرگتر تبدیل می‌کند. این می‌تواند هنگام تلاش برای انتساب نتیجه به یک integral type کوچک، منجر به compile-time error شود:

```C#

short x = 1, y = 1;
short z = x + y;          // Compile-time error
```
در این حالت، x و y به صورت implicitly به int تبدیل می‌شوند تا عملیات addition انجام شود. این بدان معناست که نتیجه نیز یک int است، که نمی‌تواند به صورت implicitly به short cast شود (زیرا می‌تواند باعث از دست رفتن data شود). برای اینکه این کد compile شود، باید یک explicit cast اضافه کنید:

```C#

short z = (short) (x + y);   // OK
```
### Special Float و Double Values

برخلاف integral types، floating-point types دارای values هستند که برخی عملیات با آن‌ها به صورت ویژه رفتار می‌کنند. این special values عبارتند از NaN (Not a Number)، +∞، −∞ و −0. Classهای float و double دارای constants برای NaN، +∞ و −∞، و همچنین values دیگر (MaxValue, MinValue, و Epsilon) هستند؛ برای مثال:

```C#

Console.WriteLine (double.NegativeInfinity);   // -Infinity
```
Constants که special values را برای double و float نشان می‌دهند، به شرح زیر هستند:

![Conventions-UsedThis-Book](../../assets/image/02/Table-2-4.png) 

تقسیم یک عدد ناصفر بر صفر، منجر به یک value بی‌نهایت می‌شود:

```C#

Console.WriteLine ( 1.0 /  0.0);                  //  Infinity
Console.WriteLine (−1.0 /  0.0);                  // -Infinity
Console.WriteLine ( 1.0 / −0.0);                  // -Infinity
Console.WriteLine (−1.0 / −0.0);                  //  Infinity
```
تقسیم صفر بر صفر، یا تفریق بی‌نهایت از بی‌نهایت، منجر به یک NaN می‌شود:

```C#

Console.WriteLine ( 0.0 /  0.0);                  //  NaN
Console.WriteLine ((1.0 /  0.0) − (1.0 / 0.0));   //  NaN
C# Language Basics
```

هنگام استفاده از ==، یک NaN value هرگز با value دیگری برابر نیست، حتی یک NaN value دیگر:

```C#

Console.WriteLine (0.0 / 0.0 == double.NaN);    // False
```
برای آزمایش اینکه آیا یک value برابر با NaN است، باید از متد float.IsNaN یا double.IsNaN استفاده کنید:

```C#

Console.WriteLine (double.IsNaN (0.0 / 0.0));   // True
```
با این حال، هنگام استفاده از object.Equals، دو NaN value برابر هستند:

```C#

Console.WriteLine (object.Equals (0.0 / 0.0, double.NaN));   // True
```
NaNها گاهی اوقات برای نمایش special values مفید هستند. در Windows Presentation Foundation (WPF)، double.NaN یک اندازه‌گیری را نشان می‌دهد که value آن "خودکار" است. راه دیگری برای نمایش چنین valueای با یک nullable type (Chapter 4) است؛ راه دیگر با یک custom struct است که یک numeric type را wrap می‌کند و یک field اضافی اضافه می‌کند (Chapter 3).

float و double از specification IEEE 754 format types پیروی می‌کنند که به صورت natively توسط تقریباً تمام processors پشتیبانی می‌شود. اطلاعات دقیق در مورد رفتار این types را می‌توانید در http://www.ieee.org بیابید.

### double در مقابل decimal

double برای محاسبات علمی (مانند محاسبه spatial coordinates) مفید است. decimal برای محاسبات مالی و valuesی مفید است که تولید می‌شوند، نه نتیجه اندازه‌گیری‌های دنیای واقعی. در اینجا خلاصه‌ای از تفاوت‌ها آورده شده است.

![Conventions-UsedThis-Book](../../assets/image/02/Table-2-5.png) 

## خطاهای Rounding اعداد حقیقی

float و double به صورت داخلی اعداد را در base 2 نمایش می‌دهند. به همین دلیل، تنها اعدادی که در base 2 قابل بیان هستند، به طور دقیق نمایش داده می‌شوند. در عمل، این بدان معناست که بیشتر literals با جزء کسری (که در base 10 هستند) به طور دقیق نمایش داده نخواهند شد؛ برای مثال:

```C#

float x = 0.1f;  // Not quite 0.1
Console.WriteLine (x + x + x + x + x + x + x + x + x + x);    // 1.0000001
```

به همین دلیل float و double برای محاسبات مالی مناسب نیستند. در مقابل، decimal در base 10 کار می‌کند و بنابراین می‌تواند اعدادی که در base 10 قابل بیان هستند (و همچنین عوامل آن، base 2 و base 5) را به طور دقیق نمایش دهد. از آنجایی که real literals در base 10 هستند، decimal می‌تواند اعدادی مانند 0.1 را به طور دقیق نمایش دهد. با این حال، نه double و نه decimal نمی‌توانند یک عدد کسری را که نمایش base 10 آن تکرار شونده است، به طور دقیق نمایش دهند:

```C#

decimal m = 1M / 6M;               // 0.1666666666666666666666666667M
double  d = 1.0 / 6.0;             // 0.16666666666666666
```
این منجر به خطاهای rounding انباشته می‌شود:

```C#

decimal notQuiteWholeM = m+m+m+m+m+m;  // 1.0000000000000000000000000002M
double  notQuiteWholeD = d+d+d+d+d+d;  // 0.99999999999999989
```
که عملیات equality و comparison را مختل می‌کند:

```C#

Console.WriteLine (notQuiteWholeM == 1M);   // False
Console.WriteLine (notQuiteWholeD < 1.0);   // True
```
## Boolean Type و Operators

bool type در C# (که System.Boolean type را alias می‌کند) یک logical value است که می‌تواند literal true یا false را به خود بگیرد.

اگرچه یک Boolean value فقط به یک bit فضای ذخیره‌سازی نیاز دارد، اما runtime از یک byte حافظه استفاده خواهد کرد زیرا این حداقل قطعه‌ای است که runtime و processor می‌توانند به طور کارآمد با آن کار کنند. برای جلوگیری از ناکارآمدی فضا در مورد arrays، .NET یک BitArray class در namespace System.Collections فراهم می‌کند که برای استفاده تنها یک bit در هر Boolean value طراحی شده است.

## bool Conversions

هیچ casting conversionsای را نمی‌توان از bool type به numeric types، یا بالعکس انجام داد.

## Equality و Comparison Operators

== و != برای equality و inequality هر typeی را آزمایش می‌کنند اما همیشه یک bool value برمی‌گردانند.3 Value types معمولاً مفهوم بسیار ساده‌ای از equality دارند:

```C#

int x = 1;
int y = 2;
int z = 1;
Console.WriteLine (x == y);         // False
Console.WriteLine (x == z);         // True
```


برای reference types، equality، به طور پیش‌فرض، بر اساس reference است، نه بر اساس actual value underlying object (اطلاعات بیشتر در Chapter 6):

```C#

Dude d1 = new Dude ("John");
Dude d2 = new Dude ("John");
Console.WriteLine (d1 == d2);       // False
Dude d3 = d1;
Console.WriteLine (d1 == d3);       // True
public class Dude
{
  public string Name;
  public Dude (string n) { Name = n; }
}
```
Equality و comparison operators، ==, !=, <, >, >=, و <=، برای تمام numeric types کار می‌کنند، اما باید با احتیاط با real numbers از آن‌ها استفاده کنید (همانطور که در "Real Number Rounding Errors" در صفحه ۵۴ دیدیم). Comparison operators همچنین بر روی enum type members با مقایسه underlying integral-type values آن‌ها کار می‌کنند. ما این را در "Enums" در صفحه ۱۵۴ توضیح می‌دهیم.

ما equality و comparison operators را با جزئیات بیشتر در "Operator Overloading" در صفحه ۲۵۶، و در "Equality Comparison" در صفحه ۳۴۴ و "Order Comparison" در صفحه ۳۵۵ توضیح می‌دهیم.

## Conditional Operators

Operators && و || شرایط and و or را آزمایش می‌کنند. آن‌ها اغلب در ترکیب با operator ! که not را بیان می‌کند، استفاده می‌شوند. در مثال زیر، method UseUmbrella در صورتی true را برمی‌گرداند که بارانی یا آفتابی باشد (برای محافظت از ما در برابر باران یا خورشید)، به شرطی که windy هم نباشد (چترها در باد بی‌فایده‌اند):

```C#

static bool UseUmbrella (bool rainy, bool sunny, bool windy)
{
  return !windy && (rainy || sunny);
}
```
Operators && و || در صورت امکان، evaluation را short-circuit می‌کنند. در مثال قبلی، اگر windy باشد، expression (rainy || sunny) حتی evaluated نمی‌شود.

Short-circuiting در اجازه دادن به expressions مانند موارد زیر برای اجرا بدون پرتاب NullReferenceException ضروری است:

```C#

if (sb != null && sb.Length > 0) ...
Operators & و | نیز شرایط and و or را آزمایش می‌کنند:
```
```C#

return !windy & (rainy | sunny);
```

تفاوت این است که آن‌ها short-circuit نمی‌کنند. به همین دلیل، به ندرت به جای conditional operators استفاده می‌شوند.

برخلاف C و C++، operators & و | هنگامی که بر bool expressions اعمال می‌شوند، مقایسات Boolean (غیر short-circuiting) را انجام می‌دهند. Operators & و | عملیات bitwise را فقط هنگامی که بر اعداد اعمال می‌شوند، انجام می‌دهند.

## Conditional operator (Ternary operator)

Conditional operator (که بیشتر به آن Ternary operator گفته می‌شود، زیرا تنها operatorی است که سه operand می‌گیرد) به شکل q ? a : b; است؛ بنابراین، اگر condition q true باشد، a evaluated می‌شود؛ در غیر این صورت b evaluated می‌شود:

```C#

static int Max (int a, int b)
{
  return (a > b) ? a : b;
}
```
Conditional operator به ویژه در Language-Integrated Query (LINQ) expressions (Chapter 8) مفید است.

## Strings و Characters

char type در C# (که System.Char type را alias می‌کند) یک Unicode character را نمایش می‌دهد و ۲ byte (UTF-16) فضا اشغال می‌کند. یک char literal در داخل single quotes مشخص می‌شود:

```C#

char c = 'A';       // Simple character
```
Escape sequences charactersی را بیان می‌کنند که نمی‌توانند به صورت literally بیان یا تفسیر شوند. یک escape sequence شامل یک backslash است که به دنبال آن یک character با معنای خاص می‌آید؛ برای مثال:

```C#

char newLine = '\n';
char backSlash = '\\';
```
Table 2-2 escape sequence characters را نشان می‌دهد.

<div align="center">
    
![Conventions-UsedThis-Book](../../assets/image/02/Table-2-6.png) <br>
![Conventions-UsedThis-Book](../../assets/image/02/Table-2-6-1.png) 
</div>

escape sequence \u (یا \x) به شما اجازه می‌دهد تا هر Unicode character را از طریق four-digit hexadecimal code آن مشخص کنید:

```C#

char copyrightSymbol = '\u00A9';
char omegaSymbol     = '\u03A9';
char newLine         = '\u000A';
```
## Char Conversions

یک implicit conversion از یک char به یک numeric type برای numeric typesی کار می‌کند که می‌توانند یک unsigned short را در خود جای دهند. برای سایر numeric types، یک explicit conversion مورد نیاز است.

## String Type

string type در C# (که System.String type را alias می‌کند و در Chapter 6 به تفصیل پوشش داده شده است) یک immutable (unmodifiable) sequence از Unicode characters را نمایش می‌دهد. یک string literal در داخل double quotes مشخص می‌شود:

```C#

string a = "Heat";
```
string یک reference type است تا یک value type. با این حال، equality operators آن از value-type semantics پیروی می‌کنند:

```C#

string a = "test";
string b = "test";
Console.Write (a == b);  // True
```
escape sequences که برای char literals معتبر هستند، در داخل strings نیز کار می‌کنند:

```C#

string a = "Here's a tab:\t";
```
هزینه این کار این است که هر زمان که به یک literal backslash نیاز دارید، باید آن را دو بار بنویسید:

```C#

string a1 = "\\\\server\\fileshare\\helloworld.cs";
```
برای جلوگیری از این مشکل، C# verbatim string literals را مجاز می‌داند. یک verbatim string literal با @ پیشوند می‌گیرد و از escape sequences پشتیبانی نمی‌کند. verbatim string زیر با مورد قبلی یکسان است:

```C#

string a2 = @"\\server\fileshare\helloworld.cs";
```
یک verbatim string literal می‌تواند چندین خط را نیز شامل شود:

```C#

string escaped  = "First Line\r\nSecond Line";
string verbatim = @"First Line
 Second Line";
// True if your text editor uses CR-LF line separators:
Console.WriteLine (escaped == verbatim);
```
می‌توانید double-quote character را در یک verbatim literal با نوشتن آن دو بار وارد کنید:

```C#

string xml = @"<customer id=""123""></customer>";
```
## Raw string literals (C# 11)

Wrapping یک string در سه یا بیشتر quote characters (""") یک raw string literal ایجاد می‌کند. Raw string literals می‌توانند تقریباً هر character sequenceای را بدون escaping یا doubling up شامل شوند:

```C#

 string raw = """<file path="c:\temp\test.txt"></file>""";
```
Raw string literals نمایش JSON, XML, و HTML literals، و همچنین regular expressions و source code را آسان می‌کنند. اگر نیاز دارید سه (یا بیشتر) quote characters را در خود string وارد کنید، می‌توانید این کار را با wrapping string در چهار (یا بیشتر) quote characters انجام دهید:

```C#

string raw = """"The """ sequence denotes raw string literals."""";
Multiline raw string literals تابع قوانین ویژه‌ای هستند. می‌توانیم string "Line 1\r\nLine 2" را به صورت زیر نمایش دهیم:
```
```C#

string multiLineRaw = """
  Line 1
  Line 2
""";
```
توجه داشته باشید که opening و closing quotes باید در خطوط جداگانه با string content باشند. علاوه بر این:

+ Whitespace پس از opening """ (در همان خط) نادیده گرفته می‌شود.

+ Whitespace قبل از closing """ (در همان خط) به عنوان common indentation در نظر گرفته می‌شود و از هر خط در string حذف می‌شود. این به شما اجازه می‌دهد تا indentation را برای خوانایی source-code وارد کنید بدون اینکه آن indentation بخشی از string شود.

در اینجا یک مثال دیگر برای نشان دادن قوانین multiline raw string literal آورده شده است:

```C#

if (true)
  Console.WriteLine ("""
    {
      "Name" : "Joe"
    }
    """);
Output به شرح زیر است:

{
  "Name" : "Joe"
}
```

Compiler یک error ایجاد خواهد کرد اگر هر خط در یک multiline raw string literal با common indentation مشخص شده توسط closing quotes پیشوند نداشته باشد.

Raw string literals می‌توانند interpolated شوند، تابع قوانین ویژه‌ای که در "String interpolation" در صفحه ۶۰ توضیح داده شده‌اند.

### String concatenation

Operator + دو string را concatenate می‌کند:

```C#

string s = "a" + "b";
```
یکی از operands ممکن است یک nonstring value باشد، در این صورت ToString روی آن value فراخوانی می‌شود:

```C#

string s = "a" + 5;  // a5
```
استفاده مکرر از operator + برای ساخت یک string ناکارآمد است: یک راه حل بهتر استفاده از System.Text.StringBuilder type است (که در Chapter 6 توضیح داده شده است).

### String interpolation

یک string که با character $ پیشوند می‌گیرد، interpolated string نامیده می‌شود. Interpolated strings می‌توانند expressions محصور شده در braces را شامل شوند:

```C#

int x = 4;
Console.Write ($"A square has {x} sides");  // Prints: A square has 4 sides
```
هر valid C# expression از هر typeی می‌تواند در داخل braces ظاهر شود، و C# expression را با فراخوانی ToString method آن یا معادل آن به یک string تبدیل خواهد کرد. می‌توانید formatting را با appending expression با یک colon و یک format string تغییر دهید (format strings در "String.Format and composite format strings" در صفحه ۲۹۶ توضیح داده شده‌اند):

```C#

string s = $"255 in hex is {byte.MaxValue:X2}";  // X2 = 2-digit hexadecimal
// Evaluates to "255 in hex is FF"
```
اگر نیاز به استفاده از colon برای هدف دیگری دارید (مانند ternary conditional operator، که بعداً آن را پوشش خواهیم داد)، باید کل expression را در parentheses wrap کنید:

```C#

bool b = true;
Console.WriteLine ($"The answer in binary is {(b ? 1 : 0)}");
```
از C# 10، interpolated strings می‌توانند constants باشند، تا زمانی که interpolated values constants باشند:

```C#

const string greeting = "Hello";
const string message = $"{greeting}, world";
```
از C# 11، interpolated strings مجاز به شامل شدن در چندین خط هستند (چه standard و چه verbatim):

```C#

string s = $"this interpolation spans {1 +
1} lines";
```

Raw string literals (از C# 11) نیز می‌توانند interpolated شوند:

```C#

string s = $"""The date and time is {DateTime.Now}""";
```
برای وارد کردن یک brace literal در یک interpolated string:

+ با standard و verbatim string literals، brace character مورد نظر را تکرار کنید.

+ با raw string literals، interpolation sequence را با تکرار $ prefix تغییر دهید.

استفاده از دو (یا بیشتر) character $ در یک raw string literal prefix interpolation sequence را از یک brace به دو (یا بیشتر) braces تغییر می‌دهد:

```C#

Console.WriteLine ($$"""{ "TimeStamp": "{{DateTime.Now}}" }""");
// Output: { "TimeStamp": "01/01/2024 12:13:25 PM" }
```
این قابلیت copy-and-paste کردن text به یک raw string literal را بدون نیاز به تغییر string حفظ می‌کند.

### String comparisons

برای انجام equality comparisons با strings، می‌توانید از operator == (یا یکی از string’s Equals methods) استفاده کنید. برای order comparison، باید از string’s CompareTo method استفاده کنید؛ operators < و > پشتیبانی نمی‌شوند. ما equality و order comparison را با جزئیات در "Comparing Strings" در صفحه ۲۹۷ توضیح می‌دهیم.

## UTF-8 Strings

از C# 11، می‌توانید از u8 suffix برای ایجاد string literals encoded شده در UTF-8 به جای UTF-16 استفاده کنید. این ویژگی برای scenarios پیشرفته مانند low-level handling JSON text در performance hotspots در نظر گرفته شده است:

```C#

ReadOnlySpan<byte> utf8 = "ab→cd"u8;  // Arrow symbol consumes 3 bytes
Console.WriteLine (utf8.Length);      // 7
```
underlying type ReadOnlySpan<T> است، که در Chapter 23 آن را پوشش می‌دهیم. می‌توانید این را با فراخوانی ToArray() method به یک array تبدیل کنید.

## Arrays

یک array، تعداد ثابتی از variables (که elements نامیده می‌شوند) از یک type خاص را نمایش می‌دهد. Elements در یک array همیشه در یک contiguous block of memory ذخیره می‌شوند که دسترسی بسیار کارآمدی را فراهم می‌کند.

یک array با square brackets پس از element type مشخص می‌شود:

```C#

char[] vowels = new char[5];    // Declare an array of 5 characters
```
Square brackets همچنین array را index می‌کنند و به یک element خاص بر اساس موقعیت دسترسی پیدا می‌کنند:

```C#

vowels[0] = 'a';
vowels[1] = 'e';
vowels[2] = 'i';
vowels[3] = 'o';
vowels[4] = 'u';
Console.WriteLine (vowels[1]);      // e
```
این "e" را چاپ می‌کند زیرا array indexes از ۰ شروع می‌شوند. می‌توانید از یک for loop statement برای iterate کردن از طریق هر element در array استفاده کنید. for loop در این مثال integer i را از ۰ تا ۴ cycle می‌کند:

```C#

for (int i = 0; i < vowels.Length; i++)
 Console.Write (vowels[i]);            // aeiou
```
Length property یک array، تعداد elements در array را برمی‌گرداند. پس از ایجاد یک array، نمی‌توانید طول آن را تغییر دهید. System.Collection namespace و subnamespaces، ساختارهای داده‌ای سطح بالاتر، مانند dynamically sized arrays و dictionaries را فراهم می‌کنند.

یک array initialization expression به شما امکان می‌دهد یک array را در یک مرحله اعلان و پر کنید:

```C#

char[] vowels = new char[] {'a','e','i','o','u'};
```
یا به سادگی:

```C#

char[] vowels = {'a','e','i','o','u'};
```
از C# 12، می‌توانید به جای curly braces از square brackets استفاده کنید:

```C#

char[] vowels = ['a','e','i','o','u'];
```
این یک collection expression نامیده می‌شود و مزیت کار کردن هنگام فراخوانی methods را نیز دارد:

C#

```
Foo (['a','e','i','o','u']);
void Foo (char[] letters) { ... }
```
Collection expressions با سایر collection types مانند lists و sets نیز کار می‌کنند—به "Collection Initializers and Collection Expressions" در صفحه ۲۰۵ مراجعه کنید.

تمام arrays از System.Array class ارث می‌برند و خدمات مشترک را برای تمام arrays فراهم می‌کنند. این members شامل methodsی برای دریافت و تنظیم elements صرف‌نظر از array type هستند. ما آن‌ها را در "The Array Class" در صفحه ۳۷۷ توضیح می‌دهیم.

### Default Element Initialization

ایجاد یک array همیشه elements را با default values پیش‌تنظیم می‌کند. Default value برای یک type نتیجه bitwise zeroing memory است. برای مثال، ایجاد یک array از integers را در نظر بگیرید. از آنجایی که int یک value type است، این ۱۰۰۰ integers را در یک contiguous block of memory اختصاص می‌دهد. Default value برای هر element 0 خواهد بود:

```C#

int[] a = new int[1000];
Console.Write (a[123]);            // 0
```
#### Value types در مقابل Reference types

اینکه آیا element type یک array یک value type است یا یک reference type، پیامدهای performance مهمی دارد. هنگامی که element type یک value type است، هر element value به عنوان بخشی از array اختصاص داده می‌شود، همانطور که در اینجا نشان داده شده است:

```C#

Point[] a = new Point[1000];
int x = a[500].X;                  // 0
public struct Point { public int X, Y; }
```
اگر Point یک class بود، ایجاد array صرفاً ۱۰۰۰ null references را اختصاص می‌داد:

```C#

Point[] a = new Point[1000];
int x = a[500].X;                  // Runtime error, NullReferenceException
public class Point { public int X, Y; }
```
برای جلوگیری از این error، باید به طور explicitly ۱۰۰۰ Points را پس از instantiating array instantiate کنیم:

```C#

Point[] a = new Point[1000];
for (int i = 0; i < a.Length; i++) // Iterate i from 0 to 999
  a[i] = new Point();             // Set array element i with new point
```
خود array همیشه یک reference type object است، صرف‌نظر از element type. برای مثال، موارد زیر قانونی است:

```C#

int[] a = null;
```
### Indices و Ranges

Indices و ranges (معرفی شده در C# 8) کار با elements یا بخش‌هایی از یک array را ساده می‌کنند.

Indices

Indices و ranges همچنین با CLR types Span و ReadOnlySpan کار می‌کنند (به Chapter 23 مراجعه کنید).

همچنین می‌توانید types خود را با indices و ranges کار کنید، با تعریف یک indexer از type Index یا Range (به "Indexers" در صفحه ۱۱۸ مراجعه کنید).

Indices به شما امکان می‌دهند تا elements را نسبت به انتهای یک array، با operator ^ ارجاع دهید. ^1 به آخرین element، ^2 به element ماقبل آخر، و غیره اشاره دارد:

```C#

char[] vowels = new char[] {'a','e','i','o','u'};
char lastElement  = vowels [^1];   // 'u'
char secondToLast = vowels [^2];   // 'o'
```
(^0 برابر با طول array است، بنابراین vowels[^0] یک error ایجاد می‌کند.)


C# indices را با کمک Index type پیاده‌سازی می‌کند، بنابراین می‌توانید موارد زیر را نیز انجام دهید:

```C#

Index first = 0;
Index last = ^1;
char firstElement = vowels [first];   // 'a'
char lastElement = vowels [last];     // 'u'
```
### Ranges

Ranges به شما امکان می‌دهند تا یک array را با استفاده از operator .. "slice" کنید:

```C#

char[] firstTwo =  vowels [..2];    // 'a', 'e'
char[] lastThree = vowels [2..];    // 'i', 'o', 'u'
char[] middleOne = vowels [2..3];   // 'i'
```
عدد دوم در range exclusive است، بنابراین ..2 elements قبل از vowels[2] را برمی‌گرداند.

می‌توانید از نماد ^ در ranges نیز استفاده کنید. موارد زیر دو character آخر را برمی‌گرداند:

C#

```
char[] lastTwo = vowels [^2..];     // 'o', 'u'
```
C# ranges را با کمک Range type پیاده‌سازی می‌کند، بنابراین می‌توانید موارد زیر را نیز انجام دهید:

```C#

Range firstTwoRange = 0..2;
char[] firstTwo = vowels [firstTwoRange];   // 'a', 'e'
```
### Multidimensional Arrays

Multidimensional arrays در دو نوع ارائه می‌شوند: rectangular و jagged. Rectangular arrays یک n-dimensional block of memory را نمایش می‌دهند، و jagged arrays arrays از arrays هستند.

### Rectangular arrays

Rectangular arrays با استفاده از commas برای جداسازی هر dimension اعلان می‌شوند. موارد زیر یک rectangular two-dimensional array را اعلان می‌کند که dimensions آن ۳ در ۳ است:

```C#

int[,] matrix = new int[3,3];
```
GetLength method یک array، طول یک dimension مشخص را برمی‌گرداند (شروع از ۰):


```C#

for (int i = 0; i < matrix.GetLength(0); i++)
  for (int j = 0; j < matrix.GetLength(1); j++)
    matrix[i,j] = i * 3 + j;
```
می‌توانید یک rectangular array را با explicit values مقداردهی کنید. کد زیر یک array مشابه مثال قبلی ایجاد می‌کند:


```C#

int[,] matrix = new int[,]
 {
  {0,1,2},
  {3,4,5},
  {6,7,8}
 };
```

#### Jagged arrays

Jagged arrays با استفاده از square brackets متوالی برای نمایش هر dimension اعلان می‌شوند. در اینجا مثالی از اعلان یک jagged two-dimensional array آورده شده است که outermost dimension آن ۳ است:

```C#

int[][] matrix = new int[3][];
```
جالب اینجاست که این new int[3][] است و نه new int[][3].
اریک لیپرت (Eric Lippert) مقاله فوق‌العاده‌ای در مورد دلیل این موضوع نوشته است.

Inner dimensions در اعلان مشخص نمی‌شوند زیرا، برخلاف یک rectangular array، هر inner array می‌تواند طول دلخواهی داشته باشد. هر inner array به طور implicitly به null مقداردهی اولیه می‌شود تا یک empty array. شما باید هر inner array را به صورت دستی ایجاد کنید:

```C#

for (int i = 0; i < matrix.Length; i++)
{
  matrix[i] = new int[3];                    // Create inner array
  for (int j = 0; j < matrix[i].Length; j++)
    matrix[i][j] = i * 3 + j;
}
```
می‌توانید یک jagged array را با explicit values مقداردهی اولیه کنید. کد زیر یک array مشابه مثال قبلی با یک element اضافی در انتها ایجاد می‌کند:

```C#

int[][] matrix = new int[][]
{
  new int[] {0,1,2},
  new int[] {3,4,5},
  new int[] {6,7,8,9}
};
```
### Simplified Array Initialization Expressions

دو روش برای کوتاه‌تر کردن array initialization expressions وجود دارد. اولین روش حذف operator new و type qualifications است:

```C#

char[] vowels = {'a','e','i','o','u'};
int[,] rectangularMatrix =
{
  {0,1,2},
  {3,4,5},
  {6,7,8}
};
int[][] jaggedMatrix =
{
  new int[] {0,1,2},
  new int[] {3,4,5},
  new int[] {6,7,8,9}
};
```
(از C# 12، می‌توانید به جای braces با single-dimensional arrays از square brackets استفاده کنید.)

رویکرد دوم استفاده از keyword var است که به compiler دستور می‌دهد تا یک local variable را به طور implicitly type کند. در اینجا مثال‌های ساده‌ای آورده شده است:

```C#

var i = 3;           // i is implicitly of type int
var s = "sausage";   // s is implicitly of type string
```
همین اصل را می‌توان در مورد arrays نیز اعمال کرد، با این تفاوت که می‌توان آن را یک مرحله فراتر برد. با حذف type qualifier پس از keyword new، compiler array type را استنباط می‌کند:

```C#

var vowels = new[] {'a','e','i','o','u'};   // Compiler infers char[]
```
در اینجا نحوه اعمال این بر روی multidimensional arrays آمده است:

```C#

var rectMatrix = new[,]        // rectMatrix is implicitly of type int[,]
{
  {0,1,2},
  {3,4,5},
  {6,7,8}
};
var jaggedMat = new int[][]    // jaggedMat is implicitly of type int[][]
{
  new[] {0,1,2},
  new[] {3,4,5},
  new[] {6,7,8,9}
};
```
برای اینکه این کار کند، تمام elements باید به طور implicitly به یک single type قابل تبدیل باشند (و حداقل یکی از elements باید از آن type باشد، و باید دقیقاً یک best type وجود داشته باشد)، همانطور که در مثال زیر:

```C#

var x = new[] {1,10000000000};   // all convertible to long
```
### Bounds Checking

تمام array indexing توسط runtime bounds checked می‌شوند. اگر از یک invalid index استفاده کنید، یک IndexOutOfRangeException پرتاب می‌شود:

```C#

int[] arr = new int[3];
arr[3] = 1;               // IndexOutOfRangeException thrown
```
Array bounds checking برای type safety ضروری است و debugging را ساده می‌کند.


به طور کلی، performance hit ناشی از bounds checking جزئی است، و Just-In-Time (JIT) compiler می‌تواند optimizations را انجام دهد، مانند تعیین از قبل اینکه آیا تمام indexes قبل از ورود به یک loop ایمن خواهند بود یا خیر، در نتیجه از بررسی در هر iteration جلوگیری می‌کند. علاوه بر این، C# code "unsafe" را فراهم می‌کند که می‌تواند به طور explicitly bounds checking را دور بزند (به "Unsafe Code and Pointers" در صفحه ۲۶۳ مراجعه کنید).
## Variables و Parameters

یک variable نشان‌دهنده یک مکان ذخیره‌سازی است که یک value قابل تغییر دارد. یک variable می‌تواند یک local variable، parameter (value، ref، یا out، یا in)، field (instance یا static)، یا array element باشد.

### The Stack و The Heap

Stack و heap مکان‌هایی هستند که variables در آن‌ها قرار می‌گیرند. هر کدام semantics طول عمر بسیار متفاوتی دارند.

#### Stack

Stack یک block of memory برای ذخیره local variables و parameters است. Stack به صورت منطقی با ورود و خروج یک method یا function رشد و کوچک می‌شود. Method زیر را در نظر بگیرید (برای جلوگیری از حواس‌پرتی، بررسی input argument نادیده گرفته شده است):

```C#

static int Factorial (int x)
{
  if (x == 0) return 1;
  return x * Factorial (x-1);
}
```
این method recursive است، به این معنی که خودش را فراخوانی می‌کند. هر بار که method وارد می‌شود، یک int جدید در stack اختصاص داده می‌شود، و هر بار که method خارج می‌شود، int deallocated می‌شود.

#### Heap

Heap حافظه‌ای است که objects (یعنی reference-type instances) در آن قرار می‌گیرند. هر زمان که یک object جدید ایجاد می‌شود، در heap اختصاص داده می‌شود، و یک reference به آن object برگردانده می‌شود. در طول اجرای یک برنامه، heap با ایجاد objects جدید شروع به پر شدن می‌کند. Runtime دارای یک garbage collector است که به صورت دوره‌ای objects را از heap deallocate می‌کند، بنابراین برنامه شما با کمبود حافظه مواجه نمی‌شود. یک object به محض اینکه توسط چیزی که خود "زنده" است referenced نشود، واجد شرایط deallocation است.

در مثال زیر، ما با ایجاد یک StringBuilder object که توسط variable ref1 ارجاع شده است شروع می‌کنیم و سپس محتوای آن را می‌نویسیم. آن StringBuilder object بلافاصله واجد شرایط garbage collection است زیرا چیزی متعاقباً از آن استفاده نمی‌کند.


سپس، یک StringBuilder دیگر ایجاد می‌کنیم که توسط variable ref2 ارجاع شده و آن reference را به ref3 کپی می‌کنیم. حتی اگر ref2 پس از آن نقطه استفاده نشود، ref3 همان StringBuilder object را زنده نگه می‌دارد—اطمینان حاصل می‌کند که تا زمانی که ما استفاده از ref3 را تمام نکرده‌ایم، واجد شرایط collection نشود:

```C#

using System;
using System.Text;
StringBuilder ref1 = new StringBuilder ("object1");
Console.WriteLine (ref1);
// The StringBuilder referenced by ref1 is now eligible for GC.
StringBuilder ref2 = new StringBuilder ("object2");
StringBuilder ref3 = ref2;
// The StringBuilder referenced by ref2 is NOT yet eligible for GC.
Console.WriteLine (ref3);      // object2
```
Value-type instances (و object references) در هر کجا که variable اعلان شده است، زندگی می‌کنند. اگر instance به عنوان یک field در یک class type، یا به عنوان یک array element اعلان شده باشد، آن instance در heap زندگی می‌کند.

شما نمی‌توانید objects را به طور explicitly در C# حذف کنید، همانطور که در C++ می‌توانید. یک object بدون reference در نهایت توسط garbage collector جمع‌آوری می‌شود.

Heap همچنین static fields را ذخیره می‌کند. برخلاف objects که در heap اختصاص داده می‌شوند (و می‌توانند garbage-collected شوند)، اینها تا پایان process زنده می‌مانند.

### Definite Assignment

C# یک definite assignment policy را اعمال می‌کند. در عمل، این بدان معناست که خارج از یک unsafe یا interop context، نمی‌توانید به طور تصادفی به uninitialized memory دسترسی پیدا کنید. Definite assignment سه پیامد دارد:

+ Local variables باید قبل از خوانده شدن، یک value به آن‌ها اختصاص داده شود.

+ Function arguments باید هنگام فراخوانی یک method ارائه شوند (مگر اینکه به عنوان optional علامت‌گذاری شده باشند؛ به "Optional parameters" در صفحه ۷۴ مراجعه کنید).

+ تمام variables دیگر (مانند fields و array elements) به طور خودکار توسط runtime مقداردهی اولیه می‌شوند.

برای مثال، کد زیر منجر به یک compile-time error می‌شود:

```C#

int x;
Console.WriteLine (x);        // Compile-time error
```
Fields و array elements به طور خودکار با default values برای type خود مقداردهی اولیه می‌شوند. کد زیر 0 را output می‌کند زیرا array elements به طور implicitly به default values خود اختصاص داده می‌شوند:

```C#

int[] ints = new int[2];
Console.WriteLine (ints[0]);    // 0
```
کد زیر 0 را output می‌کند، زیرا fields به طور implicitly یک default value به آن‌ها اختصاص داده می‌شود (چه instance و چه static):

```C#

Console.WriteLine (Test.X);   // 0
class Test { public static int X; }   // field
```
### Default Values

تمام type instances دارای یک default value هستند. Default value برای predefined types نتیجه bitwise zeroing memory است:

<div align="center">
    
![Conventions-UsedThis-Book](../../assets/image/02/Table-2-7.png) <br>
</div>
