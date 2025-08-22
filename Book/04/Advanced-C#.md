# فصل چهارم: سی شارپ پیشرفته

در این فصل، به سراغ مباحث پیشرفته زبان C# می‌رویم که بر پایه مفاهیمی بنا شده‌اند که در فصل‌های 2 و 3 بررسی کردیم.
چهار بخش اول را باید به صورت پیوسته و پشت سر هم مطالعه کنید؛ اما بخش‌های باقی‌مانده را می‌توانید به هر ترتیبی بخوانید.

### Delegates (نمایندگان) 📨

یک delegate یک شیء است که می‌داند چگونه یک متد را فراخوانی کند.

نوع delegate تعیین می‌کند که نمونه‌های delegate می‌توانند چه نوع متدهایی را فراخوانی کنند. به طور مشخص، یک delegate نوع بازگشتی متد و انواع پارامترهای متد را تعریف می‌کند.

مثال: تعریف یک نوع delegate به نام Transformer:
```c#
delegate int Transformer(int x);
```

این delegate با هر متدی که بازگشتی از نوع int داشته باشد و یک پارامتر int بگیرد سازگار است. مثل این:
```c#
int Square(int x) { return x * x; }
```

یا به صورت کوتاه‌تر (expression-bodied):
```c#
int Square(int x) => x * x;
```
**ساختن و استفاده از یک delegate 🛠️**

اختصاص یک متد به یک متغیر delegate، باعث ایجاد یک نمونه delegate می‌شود:
```c#
Transformer t = Square;
```

فراخوانی یک نمونه delegate دقیقاً مثل فراخوانی یک متد است:
```c#
int answer = t(3);   // answer برابر با 9
```

مثال کامل:
```c#
Transformer t = Square;   // ایجاد نمونه delegate
int result = t(3);        // فراخوانی delegate
Console.WriteLine(result); // خروجی: 9

int Square(int x) => x * x;

delegate int Transformer(int x);  // تعریف نوع delegate
```
**مفهوم اصلی delegate 🎯**

یک نمونه delegate واقعاً به عنوان نماینده‌ی caller عمل می‌کند:
caller (فراخواننده) delegate را فراخوانی می‌کند و سپس delegate، متد هدف را فراخوانی می‌کند.

این واسطه‌گری (indirection) باعث می‌شود که caller از متد هدف جدا و مستقل باشد.

**نکته‌های مهم ✅**

دستور:
```
Transformer t = Square;
```

در واقع یک میان‌بُر (shorthand) برای این است:
```c#
Transformer t = new Transformer(Square);
```

عبارت:
```
t(3)
```

میان‌بُری است برای:
```
t.Invoke(3)
```

از نظر فنی، وقتی به Square بدون پرانتز و آرگومان اشاره می‌کنیم، در حال مشخص کردن یک method group هستیم.
اگر متد overload شده باشد، کامپایلر C# بر اساس امضای delegate انتخاب می‌کند که کدام overload مناسب است.

**Delegate شبیه به Callback 📞**


یک delegate مشابه چیزی است که در اصطلاح عمومی callback نامیده می‌شود.
این مفهوم سازه‌هایی مثل function pointers در زبان C را نیز در بر می‌گیرد.

### نوشتن متدهای پلاگین با استفاده از Delegateها ✨

یک متغیر از نوع delegate در زمان اجرا (runtime) به یک متد نسبت داده می‌شود. این ویژگی برای نوشتن متدهای پلاگین (plug-in methods) بسیار کاربردی است.

در مثال زیر، ما یک متد کمکی (utility method) به نام Transform داریم که یک عمل transform را روی هر عضو از یک آرایه‌ی عددی (integer array) اعمال می‌کند. این متد Transform یک پارامتر از نوع delegate دارد که می‌توانید برای مشخص کردن پلاگین transform از آن استفاده کنید:
```c#
int[] values = { 1, 2, 3 };
Transform (values, Square);      // اتصال متد Square به Transform
foreach (int i in values)
  Console.Write (i + "  ");      // خروجی: 1   4   9

void Transform (int[] values, Transformer t)
{
  for (int i = 0; i < values.Length; i++)
    values[i] = t (values[i]);
}

int Square (int x) => x * x;
int Cube (int x) => x * x * x;

delegate int Transformer (int x);
```

🔹 در اینجا اگر در خط دوم به‌جای Square از Cube استفاده کنیم، تبدیل روی اعداد به‌صورت مکعب (توان سوم) انجام می‌شود.

🔹 متد Transform یک higher-order function است، چون یک تابع (delegate) را به‌عنوان آرگومان می‌گیرد. (هر متدی که یک delegate را برگرداند نیز یک higher-order function محسوب می‌شود.)

### اهداف متد در Delegateها: Instance و Static ⚡

یک متد هدف (target method) برای یک delegate می‌تواند محلی (local)، ایستا (static) یا نمونه‌ای (instance) باشد.

مثال یک متد static به‌عنوان هدف delegate:
```c#
Transformer t = Test.Square;
Console.WriteLine (t(10));      // 100

class Test 
{ 
    public static int Square (int x) => x * x; 
}

delegate int Transformer (int x);
```
مثال یک متد instance به‌عنوان هدف delegate:
```c#
Test test = new Test();
Transformer t = test.Square;
Console.WriteLine (t(10));      // 100

class Test 
{ 
    public int Square (int x) => x * x; 
}

delegate int Transformer (int x);
```

🔑 زمانی که یک متد instance به یک delegate اختصاص داده می‌شود، آن delegate فقط به خود متد اشاره نمی‌کند، بلکه نمونه‌ای از کلاس که متد به آن تعلق دارد را هم نگه‌داری می‌کند.

**خاصیت Target در کلاس System.Delegate 🎯**

کلاس System.Delegate یک ویژگی به نام Target دارد که این نمونه (instance) را نشان می‌دهد (و اگر delegate به یک متد static اشاره کند، مقدارش null خواهد بود).

مثال:
```c#
MyReporter r = new MyReporter();
r.Prefix = "%Complete: ";

ProgressReporter p = r.ReportProgress;
p(99);                                 // %Complete: 99

Console.WriteLine (p.Target == r);     // True
Console.WriteLine (p.Method);          // Void ReportProgress(Int32)

r.Prefix = "";
p(99);                                 // 99

public delegate void ProgressReporter (int percentComplete);

class MyReporter
{
  public string Prefix = "";
  public void ReportProgress (int percentComplete)
    => Console.WriteLine (Prefix + percentComplete);
}
```

✅ چون نمونه (instance) در خاصیت Target ذخیره می‌شود، طول عمر آن حداقل به اندازه طول عمر delegate گسترش می‌یابد.

🔖 جمع‌بندی:

+ Delegateها به ما اجازه می‌دهند که متدها را مثل متغیرها پاس بدهیم.

+ می‌توانند به متدهای static یا instance متصل شوند.

+ خاصیت Target تضمین می‌کند که نمونه مربوط به متد instance تا وقتی delegate زنده است، باقی بماند.

💡 این مفهوم پایه‌ای در طراحی پلاگین‌ها، کال‌بک‌ها و برنامه‌نویسی شیءگراست.
### نمایندگان چندپخشی (Multicast Delegates) 🎯

تمام نمونه‌های delegate در سی‌شارپ دارای قابلیت چندپخشی (multicast) هستند. این یعنی یک نمونه‌ی delegate می‌تواند نه فقط به یک متد، بلکه به یک لیست از متدها اشاره کند.

عملگرهای + و += برای ترکیب کردن نمونه‌های delegate استفاده می‌شوند:
```c#
SomeDelegate d = SomeMethod1;
d += SomeMethod2;
```

خط آخر از نظر عملکرد دقیقاً معادل این است:
```c#
d = d + SomeMethod2;
```

اکنون وقتی d فراخوانی شود، هم SomeMethod1 و هم SomeMethod2 اجرا می‌شوند.
✅ توجه داشته باشید که متدها به ترتیبی که اضافه شده‌اند فراخوانی می‌شوند.

عملگرهای - و -= هم برای حذف یک متد از لیست متدهای یک delegate استفاده می‌شوند:
```
d -= SomeMethod1;
```

اکنون فراخوانی d باعث می‌شود فقط SomeMethod2 اجرا شود.

📌 نکته مهم:

+ وقتی روی یک متغیر delegate که مقدارش null است، عمل + یا += انجام دهید، باز هم کار می‌کند و معادل این است که به آن مقدار جدید بدهید:
```c#
SomeDelegate d = null;
d += SomeMethod1;   // معادل با: d = SomeMethod1 وقتی d برابر null است
```

+ به طور مشابه، اگر روی یک delegate که فقط یک متد را نگه داشته باشد عمل -= کنید، نتیجه‌اش معادل اختصاص مقدار null به آن متغیر خواهد بود.

⚡ یک نکته مهم دیگر:
Delegates در سی‌شارپ immutable هستند. یعنی وقتی شما += یا -= را استفاده می‌کنید، در واقع یک نمونه‌ی جدید از delegate ساخته می‌شود و به متغیر موجود اختصاص داده می‌شود.

📢 اگر یک delegate چندپخشی (multicast delegate) دارای نوع بازگشتی غیر از void باشد، مقدار بازگشتی که شما دریافت می‌کنید مربوط به آخرین متدی است که اجرا شده است. متدهای قبلی همچنان اجرا می‌شوند، اما مقدار بازگشتی آن‌ها نادیده گرفته می‌شود.
(به همین دلیل، در بیشتر مواردی که از multicast delegate استفاده می‌شود، آن‌ها void برمی‌گردانند.)

🧩 تمام انواع delegate به طور ضمنی از System.MulticastDelegate ارث‌بری می‌کنند که خودش از System.Delegate ارث‌بری می‌کند.
کامپایلر سی‌شارپ تمام عملیات +، -، += و -= روی delegateها را به متدهای استاتیک Combine و Remove از کلاس System.Delegate تبدیل می‌کند.
#### مثال Multicast Delegate 🛠️

فرض کنید یک متدی نوشته‌اید که اجرای آن زمان زیادی می‌برد. این متد می‌تواند به طور منظم میزان پیشرفت کار را به فراخواننده گزارش دهد؛ این کار با فراخوانی یک delegate انجام می‌شود.

در مثال زیر، متد HardWork یک پارامتر از نوع delegate به نام ProgressReporter دریافت می‌کند و از آن برای نشان دادن پیشرفت استفاده می‌کند:
```c#
public delegate void ProgressReporter(int percentComplete);

public class Util
{
    public static void HardWork(ProgressReporter p)
    {
        for (int i = 0; i < 10; i++)
        {
            p(i * 10);                           // فراخوانی delegate
            System.Threading.Thread.Sleep(100);  // شبیه‌سازی کار سنگین
        }
    }
}
```

برای مانیتور کردن میزان پیشرفت، می‌توانیم یک نمونه‌ی Multicast Delegate به نام p بسازیم تا پیشرفت هم‌زمان توسط دو متد مستقل بررسی شود:
```c#
ProgressReporter p = WriteProgressToConsole;
p += WriteProgressToFile;

Util.HardWork(p);

void WriteProgressToConsole(int percentComplete)
    => Console.WriteLine(percentComplete);

void WriteProgressToFile(int percentComplete)
    => System.IO.File.WriteAllText("progress.txt",
                                    percentComplete.ToString());
```

🔍 در اینجا چه اتفاقی می‌افتد؟

1. ابتدا یک delegate از نوع ProgressReporter ساخته می‌شود و به متد WriteProgressToConsole اشاره می‌کند.

2. سپس با عملگر +=، متد WriteProgressToFile هم به آن اضافه می‌شود.
یعنی delegate p اکنون یک multicast delegate است.

3. هر بار که متد HardWork پیشرفت را گزارش می‌دهد (p(i * 10))، هر دو متد اجرا می‌شوند:

+ WriteProgressToConsole مقدار پیشرفت را روی کنسول چاپ می‌کند.

+ WriteProgressToFile مقدار پیشرفت را در یک فایل progress.txt ذخیره می‌کند.

📊 نتیجه نهایی:

+ شما همزمان در کنسول می‌بینید که پیشرفت کار چند درصد است.

+ یک فایل متنی هم دارید که همان اطلاعات را ذخیره می‌کند.

### انواع Delegate عمومی (Generic Delegate Types) ⚡

یک delegate می‌تواند شامل پارامترهای عمومی (Generic Type Parameters) باشد:
```c#
public delegate T Transformer<T>(T arg);
```

با چنین تعریفی می‌توانیم یک متد ابزار عمومی (Utility Method) بنویسیم که روی هر نوع داده‌ای کار کند:
```c#
int[] values = { 1, 2, 3 };
Util.Transform(values, Square);      // اتصال متد Square

foreach (int i in values)
    Console.Write(i + "  ");         // خروجی: 1   4   9

int Square(int x) => x * x;

public class Util
{
    public static void Transform<T>(T[] values, Transformer<T> t)
    {
        for (int i = 0; i < values.Length; i++)
            values[i] = t(values[i]);
    }
}
```
### Delegates آماده: Func و Action ✅

با معرفی generic delegates، این امکان فراهم شد که مجموعه‌ای کوچک از delegateها طراحی شوند که آن‌قدر عمومی و انعطاف‌پذیر باشند که برای متدهایی با هر نوع خروجی و هر تعداد (معقول) آرگومان قابل استفاده باشند.

این delegateها همان Func و Action هستند که در فضای نام System تعریف شده‌اند:
```c#
delegate TResult Func<out TResult>();                
delegate TResult Func<in T, out TResult>(T arg);          
delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
// ... ادامه دارد تا 16 پارامتر

delegate void Action();
delegate void Action<in T>(T arg);
delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
// ... ادامه دارد تا 16 پارامتر
```


📌 این‌ها بسیار عمومی و قدرتمند هستند.

**جایگزینی Transformer با Func 🔄**

در مثال قبلی، به جای تعریف delegate اختصاصی Transformer، می‌توانستیم از Func استفاده کنیم:
```c#
public static void Transform<T>(T[] values, Func<T, T> transformer)
{
    for (int i = 0; i < values.Length; i++)
        values[i] = transformer(values[i]);
}
```

در اینجا:

+ Func<T, T> یعنی یک delegate که یک ورودی از نوع T می‌گیرد و خروجی هم از همان نوع T برمی‌گرداند.

+ دقیقاً همان چیزی است که قبلاً با Transformer<T> ساخته بودیم.

**محدودیت‌ها 🚧**

تنها سناریوهای عملی که Func و Action پوشش نمی‌دهند، مواردی هستند که شامل پارامترهای ref/out یا pointer باشند.

**نکته تاریخی 🕰️**

زمانی که C# برای اولین بار معرفی شد، هنوز generics وجود نداشت. به همین دلیل، Func و Action هم وجود نداشتند.
به همین خاطر، بخش زیادی از کتابخانه‌ی .NET از delegateهای سفارشی استفاده می‌کند، نه از Func و Action.

### مقایسه Delegate‌ها و Interface‌ها ⚔️

هر مسئله‌ای که با یک delegate حل می‌شود، می‌تواند با یک interface هم حل شود.

به‌عنوان مثال، می‌توانیم نمونه‌ی اولیه‌مان را با استفاده از یک interface به نام ITransformer بازنویسی کنیم، به جای اینکه از delegate استفاده کنیم:
```c#
int[] values = { 1, 2, 3 };
Util.TransformAll(values, new Squarer());

foreach (int i in values)
    Console.WriteLine(i);

public interface ITransformer
{
    int Transform(int x);
}

public class Util
{
    public static void TransformAll(int[] values, ITransformer t)
    {
        for (int i = 0; i < values.Length; i++)
            values[i] = t.Transform(values[i]);
    }
}

class Squarer : ITransformer
{
    public int Transform(int x) => x * x;
}
```
چه زمانی delegate انتخاب بهتری از interface است؟ ✅

طراحی با delegate ممکن است انتخاب بهتری باشد اگر یک یا چند مورد زیر برقرار باشند:

+ 🔹 interface فقط یک متد تعریف کرده باشد.

+ 🔹 نیاز به multicast (اتصال چند متد به یک delegate) داشته باشیم.

+ 🔹 یک subscriber مجبور باشد چند بار یک interface را پیاده‌سازی کند.

**بررسی مثال 🔍**

در مثال ITransformer:

+ ما نیازی به multicast نداریم.

+ اما interface فقط یک متد (Transform) تعریف کرده است.

+ از طرفی ممکن است بخواهیم چندین تبدیل مختلف (مثلاً مربع یا مکعب) را پیاده‌سازی کنیم.

در چنین حالتی، اگر از interface استفاده کنیم، مجبور می‌شویم برای هر تبدیل یک کلاس جداگانه بسازیم، چون یک کلاس فقط یک بار می‌تواند ITransformer را پیاده‌سازی کند. این کار دست‌وپاگیر و تکراری می‌شود.

مثلاً:
```c#
int[] values = { 1, 2, 3 };
Util.TransformAll(values, new Cuber());

foreach (int i in values)
    Console.WriteLine(i);

class Squarer : ITransformer
{
    public int Transform(int x) => x * x;
}

class Cuber : ITransformer
{
    public int Transform(int x) => x * x * x;
}
```

📌 نتیجه:

+ با delegateها، پیاده‌سازی ساده‌تر و منعطف‌تر است (می‌توانیم به‌راحتی متدهای مختلفی مثل مربع یا مکعب را پاس بدهیم).

+ با interfaceها، مجبوریم برای هر عملگر یک کلاس جدید بسازیم.
  
### سازگاری Delegate ها ⚖️

#### سازگاری نوع (Type Compatibility) 🏷️

تمام نوع‌های delegate با هم ناسازگار هستند، حتی اگر امضا (signature) آن‌ها یکی باشد:
```c#
D1 d1 = Method1;
D2 d2 = d1;   // ❌ خطای زمان کامپایل

void Method1() { }

delegate void D1();
delegate void D2();
```

🔹 اما این حالت مجاز است:
```
D2 d2 = new D2(d1);   // ✅ درست
```
**مقایسه‌ی برابری (Equality) ✅**

دو نمونه‌ی delegate وقتی برابر محسوب می‌شوند که به یک متد یکسان اشاره کنند:
```c#
D d1 = Method1;
D d2 = Method1;

Console.WriteLine(d1 == d2);   // True

void Method1() { }

delegate void D();
```

🔹 در مورد multicast delegate‌ها هم، برابری وقتی برقرار است که به همان متدها و به همان ترتیب اشاره کنند.

#### سازگاری پارامتر (Parameter Compatibility) 🔄

وقتی یک متد را فراخوانی می‌کنید، می‌توانید آرگومانی بدهید که از نوع خاص‌تر از پارامتر متد باشد. این رفتار معمولی polymorphism است.

به همین دلیل، یک delegate هم می‌تواند پارامترهایی خاص‌تر از متد هدف خود داشته باشد. این ویژگی را contravariance می‌نامند.

مثال:
```c#
StringAction sa = new StringAction(ActOnObject);
sa("hello");

void ActOnObject(object o) => Console.WriteLine(o);   // خروجی: hello

delegate void StringAction(string s);
```

🔹 در اینجا StringAction یک متد را با پارامتر string فراخوانی می‌کند.
🔹 اما متدی که واقعا اجرا می‌شود (ActOnObject) پارامترش از نوع object است.
🔹 در این حالت، آرگومان string به‌طور خودکار تبدیل به object (upcast) می‌شود.

**3. ارتباط با الگوی استاندارد Event 🎯**

الگوی استاندارد event در C# طوری طراحی شده است که از contravariance استفاده کند.

+ همه‌ی کلاس‌های رویداد از کلاس پایه‌ی مشترک EventArgs ارث‌بری می‌کنند.

+ بنابراین می‌توانید یک متد داشته باشید که توسط دو delegate مختلف فراخوانی شود:

     + یکی رویداد MouseEventArgs پاس بدهد 🖱️

     + دیگری KeyEventArgs پاس بدهد ⌨️

این کار انعطاف‌پذیری بالایی در مدیریت رویدادها به شما می‌دهد.

📌 خلاصه:

+ delegate type ها حتی با امضای یکسان هم ناسازگار هستند.

+ multicast delegateها برابرند اگر دقیقا همان متدها و ترتیب را داشته باشند.

+ contravariance اجازه می‌دهد متد هدف، پارامتر عام‌تر از delegate داشته باشد.

+ event pattern از همین ویژگی برای پشتیبانی از انواع مختلف رویدادها استفاده می‌کند.

#### سازگاری نوع بازگشتی در Delegateها (Return Type Compatibility) 🔄

همان‌طور که وقتی یک متد را فراخوانی می‌کنید ممکن است نوعی خاص‌تر از چیزی که انتظار داشتید برگردد (رفتار معمولی polymorphism)؛ در مورد delegate‌ها هم همین موضوع صادق است.

یعنی متد هدف یک delegate می‌تواند نوع بازگشتی خاص‌تر از چیزی که delegate تعریف کرده داشته باشد.
این ویژگی را covariance می‌نامند.

مثال: Covariance در نوع بازگشتی 📝
```c#
ObjectRetriever o = new ObjectRetriever(RetrieveString);
object result = o();
Console.WriteLine(result);   // hello

string RetrieveString() => "hello";

delegate object ObjectRetriever();
```

🔹 اینجا ObjectRetriever انتظار دارد متدی که به آن متصل است، یک object برگرداند.
🔹 اما در واقعیت، متد RetrieveString یک string برمی‌گرداند.
🔹 چون string زیرکلاس object است، این مجاز است ✅.

📌 پس: نوع بازگشتی delegate ها covariant است.

#### واریانس در Delegateهای جنریک ⚙️

در فصل ۳ گفتیم که اینترفیس‌های جنریک می‌توانند از covariant و contravariant استفاده کنند.
این قابلیت برای delegate‌های جنریک هم وجود دارد.

**قوانین خوب برای تعریف delegate جنریک:**

اگر پارامتر نوع (Type Parameter) فقط در خروجی استفاده می‌شود → با out (covariant) علامت‌گذاری شود.

اگر پارامتر نوع فقط در ورودی استفاده می‌شود → با in (contravariant) علامت‌گذاری شود.

**مثال ۱: Covariance در جنریک‌ها (با Func) 📤**

در فضای نام System داریم:
```c#
delegate TResult Func<out TResult>();
```

این باعث می‌شود بتوانیم بنویسیم:
```c#
Func<string> x = () => "Hello!";
Func<object> y = x;   // ✅ مجاز به خاطر covariance
```

🔹 یعنی می‌توانیم Func<string> را به Func<object> تبدیل کنیم چون string زیرکلاس object است.

**مثال ۲: Contravariance در جنریک‌ها (با Action) 📥**

باز هم در فضای نام System:
```c#
delegate void Action<in T>(T arg);
```

این باعث می‌شود:
```c#
Action<object> x = obj => Console.WriteLine(obj);
Action<string> y = x;   // ✅ مجاز به خاطر contravariance
```

🔹 یعنی می‌توانیم Action<object> را به Action<string> تبدیل کنیم چون متدی که انتظار دریافت object دارد، می‌تواند string هم بگیرد.

📌 خلاصه:

+ نوع بازگشتی delegate می‌تواند خاص‌تر از نوع تعریف‌شده باشد → این covariance است.

+ در delegate جنریک:

     + نوع خروجی (out) → covariant 📤

     + نوع ورودی (in) → contravariant 📥

این باعث می‌شود تبدیل‌ها بین delegateها به‌صورت طبیعی و بر اساس ارث‌بری انجام شوند.
### رویدادها (Events)

هنگام استفاده از **delegates**، دو نقش معمولاً ظاهر می‌شوند: **broadcaster** و **subscriber**.

* **Broadcaster** نوعی است که دارای یک فیلد delegate می‌باشد. این نوع تصمیم می‌گیرد که چه زمانی پیام را پخش کند، با **invoke** کردن delegate.
* **Subscribers** دریافت‌کنندگان هدف متد هستند. یک subscriber تصمیم می‌گیرد چه زمانی شروع به گوش دادن کند و چه زمانی آن را متوقف کند، با استفاده از عملگرهای `+=` و `-=` روی delegate مربوط به broadcaster. یک subscriber درباره دیگر subscribers اطلاعی ندارد و در عملکرد آن‌ها دخالتی نمی‌کند.

### رویدادها در C# پیشرفته ⚡

رویدادها یک ویژگی زبانی هستند که این الگو را رسمی می‌کنند. یک **event** یک ساختار است که تنها زیرمجموعه‌ای از قابلیت‌های delegate را که برای مدل **broadcaster/subscriber** لازم است، در معرض قرار می‌دهد. هدف اصلی رویدادها جلوگیری از دخالت subscribers در عملکرد یکدیگر است.

ساده‌ترین روش برای تعریف یک رویداد، استفاده از کلیدواژه **event** قبل از یک عضو delegate است:

```csharp
// تعریف delegate
public delegate void PriceChangedHandler(decimal oldPrice, decimal newPrice);

public class Broadcaster
{
    // تعریف رویداد
    public event PriceChangedHandler PriceChanged;
}
```

کدی که داخل نوع **Broadcaster** قرار دارد، دسترسی کامل به **PriceChanged** دارد و می‌تواند آن را مانند یک delegate معمولی مدیریت کند. کدهای خارج از **Broadcaster** تنها می‌توانند عملیات `+=` و `-=` را روی رویداد **PriceChanged** انجام دهند.

✅ ادامه‌ی متن را ارسال کنید تا ترجمه بعدی را آماده کنم.


### چگونه رویدادها در درون کار می‌کنند؟ 🔍

وقتی یک رویداد به شکل زیر تعریف می‌کنید:

```csharp
public class Broadcaster
{
    public event PriceChangedHandler PriceChanged;
}
```

سه اتفاق در پشت صحنه رخ می‌دهد:

1️⃣ ابتدا، **کامپایلر** تعریف رویداد را به چیزی شبیه به کد زیر ترجمه می‌کند:

```csharp
PriceChangedHandler priceChanged;   // delegate خصوصی

public event PriceChangedHandler PriceChanged
{
    add    { priceChanged += value; }
    remove { priceChanged -= value; }
}
```

کلیدواژه‌های **add** و **remove** نشان‌دهنده **accessorهای رویداد صریح** هستند—که عملکردی شبیه به **accessorهای property** دارند. بعداً درباره نحوه نوشتن این‌ها توضیح خواهیم داد.

2️⃣ دوم، کامپایلر داخل کلاس **Broadcaster** به دنبال ارجاعات به **PriceChanged** می‌گردد که عملیات دیگری غیر از `+=` یا `-=` انجام می‌دهند و آن‌ها را به فیلد delegate زیرین یعنی **priceChanged** هدایت می‌کند.

3️⃣ سوم، کامپایلر عملیات `+=` و `-=` روی رویداد را به فراخوانی **add** و **remove** accessorهای رویداد ترجمه می‌کند. جالب است بدانید که این کار باعث می‌شود رفتار `+=` و `-=` وقتی روی رویدادها اعمال می‌شوند، منحصربه‌فرد باشد: برخلاف سایر سناریوها، این‌ها تنها یک میانبر برای `+` و `-` به همراه انتساب نیستند.

مثالی را در نظر بگیرید 📈

کلاس **Stock** رویداد **PriceChanged** خود را هر بار که قیمت سهام تغییر می‌کند، فعال می‌کند:

```csharp
public delegate void PriceChangedHandler(decimal oldPrice, decimal newPrice);

public class Stock
{
    string symbol;
    decimal price;
    public Stock(string symbol) => this.symbol = symbol;
    public event PriceChangedHandler PriceChanged;
    public decimal Price
    {
        get => price;
        set
        {
            if (price == value) return;   // خروج اگر چیزی تغییر نکرده
            decimal oldPrice = price;
            price = value;
            if (PriceChanged != null)      // اگر لیست فراخوانی خالی نیست
                PriceChanged(oldPrice, price); // رویداد فعال شود
        }
    }
}
```

اگر کلیدواژه **event** را حذف کنیم و **PriceChanged** به یک فیلد معمولی delegate تبدیل شود، باز هم مثال ما همان نتایج را خواهد داد. اما کلاس **Stock** کمتر مقاوم خواهد بود، زیرا subscribers می‌توانند با یکدیگر تداخل ایجاد کنند، مثلاً:

* جایگزین کردن دیگر subscribers با انتساب مجدد **PriceChanged** به جای استفاده از عملگر `+=`.
* پاک کردن تمام subscribers با انتساب **PriceChanged** به `null`.
* ارسال پیام به subscribers دیگر با invoke کردن delegate.

### الگوی استاندارد رویدادها 🛠️

در تقریباً همه مواردی که رویدادها در کتابخانه‌های .NET تعریف می‌شوند، تعریف آن‌ها مطابق یک الگوی استاندارد است تا یکپارچگی بین کدهای کتابخانه و کاربر حفظ شود. در مرکز این الگو، کلاس **System.EventArgs** قرار دارد؛ یک کلاس از پیش تعریف‌شده در .NET که هیچ عضوی ندارد (به جز فیلد استاتیک **Empty**). **EventArgs** کلاس پایه‌ای برای انتقال اطلاعات مربوط به یک رویداد است.

در مثال Stock ما، برای انتقال قیمت قدیمی و جدید هنگام فعال شدن رویداد **PriceChanged**، کلاس EventArgs را به صورت زیر subclass می‌کنیم:

```csharp
public class PriceChangedEventArgs : System.EventArgs
{
    public readonly decimal LastPrice;
    public readonly decimal NewPrice;

    public PriceChangedEventArgs(decimal lastPrice, decimal newPrice)
    {
        LastPrice = lastPrice;
        NewPrice = newPrice;
    }
}
```

برای استفاده مجدد، subclass EventArgs معمولاً بر اساس اطلاعاتی که شامل می‌شود نامگذاری می‌شود، نه بر اساس رویدادی که برای آن استفاده می‌شود. داده‌ها معمولاً به صورت property یا فیلد فقط‌خواندنی ارائه می‌شوند.

### انتخاب یا تعریف delegate برای رویداد 🎯

سه قاعده وجود دارد:

1️⃣ نوع بازگشتی باید **void** باشد.
2️⃣ دو آرگومان دریافت کند: آرگومان اول از نوع **object** و آرگومان دوم یک subclass از **EventArgs**. آرگومان اول نشان‌دهنده broadcaster و آرگومان دوم شامل اطلاعات اضافی برای انتقال است.
3️⃣ نام delegate باید با **EventHandler** پایان یابد.

.NET یک delegate عمومی به نام **System.EventHandler<>** برای کمک به این کار تعریف کرده است:

```csharp
public delegate void EventHandler<TEventArgs>(object source, TEventArgs e);
```

قبل از وجود genericها در زبان (قبل از C# 2.0)، باید delegate سفارشی را به صورت زیر می‌نوشتیم:

```csharp
public delegate void PriceChangedHandler(object sender, PriceChangedEventArgs e);
```

به دلایل تاریخی، بیشتر رویدادها در کتابخانه‌های .NET از این روش استفاده می‌کنند.

### تعریف رویداد با delegate انتخاب‌شده 🔧

```csharp
public class Stock
{
    ...
    public event EventHandler<PriceChangedEventArgs> PriceChanged;
}
```

### نوشتن متد محافظت‌شده و virtual برای فعال کردن رویداد

نام این متد باید همان نام رویداد باشد، با پیشوند **On**، و یک آرگومان **EventArgs** دریافت کند:

```csharp
public class Stock
{
    ...
    public event EventHandler<PriceChangedEventArgs> PriceChanged;

    protected virtual void OnPriceChanged(PriceChangedEventArgs e)
    {
        if (PriceChanged != null) PriceChanged(this, e);
    }
}
```

برای عملکرد مقاوم در سناریوهای چندنخی (multithreaded)، بهتر است delegate را قبل از تست و فراخوانی به یک متغیر موقت انتساب دهید:

```csharp
var temp = PriceChanged;
if (temp != null) temp(this, e);
```

همچنین می‌توان با استفاده از **null-conditional operator** همان کار را بدون متغیر موقت انجام داد:

```csharp
PriceChanged?.Invoke(this, e);
```

این روش هم **thread-safe** و هم مختصر است و بهترین روش عمومی برای فراخوانی رویدادها محسوب می‌شود.

### مثال کامل 💻

```csharp
using System;

Stock stock = new Stock("THPW");
stock.Price = 27.10M;

// ثبت برای رویداد PriceChanged
stock.PriceChanged += stock_PriceChanged;
stock.Price = 31.59M;

void stock_PriceChanged(object sender, PriceChangedEventArgs e)
{
    if ((e.NewPrice - e.LastPrice) / e.LastPrice > 0.1M)
        Console.WriteLine("Alert, 10% stock price increase!");
}

public class PriceChangedEventArgs : EventArgs
{
    public readonly decimal LastPrice;
    public readonly decimal NewPrice;

    public PriceChangedEventArgs(decimal lastPrice, decimal newPrice)
    {
        LastPrice = lastPrice; NewPrice = newPrice;
    }
}

public class Stock
{
    string symbol;
    decimal price;
    public Stock(string symbol) => this.symbol = symbol;
    public event EventHandler<PriceChangedEventArgs> PriceChanged;

    protected virtual void OnPriceChanged(PriceChangedEventArgs e)
    {
        PriceChanged?.Invoke(this, e);
    }

    public decimal Price
    {
        get => price;
        set
        {
            if (price == value) return;
            decimal oldPrice = price;
            price = value;
            OnPriceChanged(new PriceChangedEventArgs(oldPrice, price));
        }
    }
}
```

### استفاده از EventHandler غیر عمومی

وقتی رویداد نیاز به انتقال اطلاعات اضافی ندارد، می‌توان از **EventHandler** غیر generic استفاده کرد. در این مثال، کلاس **Stock** بازنویسی شده تا رویداد **PriceChanged** پس از تغییر قیمت فعال شود و تنها نیاز است بدانیم رویداد رخ داده است، بدون نیاز به اطلاعات اضافی. همچنین از **EventArgs.Empty** استفاده می‌کنیم تا از ایجاد غیرضروری یک نمونه EventArgs جلوگیری شود:

```csharp
public class Stock
{
    string symbol;
    decimal price;

    public Stock(string symbol) { this.symbol = symbol; }
    public event EventHandler PriceChanged;

    protected virtual void OnPriceChanged(EventArgs e)
    {
        PriceChanged?.Invoke(this, e);
    }

    public decimal Price
    {
        get { return price; }
        set
        {
            if (price == value) return;
            price = value;
            OnPriceChanged(EventArgs.Empty);
        }
    }
}
```

**Accessorهای رویدادها 🔑**

Accessorهای یک رویداد، پیاده‌سازی‌های عملگرهای `+=` و `-=` آن هستند. به طور پیش‌فرض، این accessors به صورت ضمنی توسط کامپایلر پیاده‌سازی می‌شوند. به مثال زیر توجه کنید:

```csharp
public event EventHandler PriceChanged;
```

کامپایلر این را به شکل زیر تبدیل می‌کند:

* یک فیلد delegate خصوصی
* یک جفت تابع accessor عمومی برای رویداد (**add\_PriceChanged** و **remove\_PriceChanged**) که عملیات `+=` و `-=` را به فیلد delegate خصوصی هدایت می‌کنند

شما می‌توانید این روند را با تعریف **explicit event accessors** به دست بگیرید. در اینجا پیاده‌سازی دستی رویداد **PriceChanged** از مثال قبلی آمده است:

```csharp
private EventHandler priceChanged;   // تعریف یک delegate خصوصی
public event EventHandler PriceChanged
{
    add    { priceChanged += value; }
    remove { priceChanged -= value; }
}
```

این مثال از نظر عملکرد با پیاده‌سازی پیش‌فرض C# یکسان است (به جز اینکه C# همچنین ایمنی در برابر چندنخی را با الگوریتم lock-free compare-and-swap تضمین می‌کند). با تعریف دستی accessors، به C# می‌گوییم که منطق پیش‌فرض فیلد و accessor را تولید نکند.

با استفاده از explicit event accessors، می‌توان استراتژی‌های پیچیده‌تری برای ذخیره و دسترسی به delegate زیرین اعمال کرد. سه سناریو که این کاربرد دارد:

1️⃣ وقتی accessors تنها به عنوان واسطه برای کلاس دیگری هستند که رویداد را پخش می‌کند.
2️⃣ وقتی کلاس تعداد زیادی رویداد دارد اما اغلب تنها تعداد کمی subscriber وجود دارد، مثل کنترل‌های ویندوز. در این موارد بهتر است delegateهای subscribers را در یک **dictionary** ذخیره کنیم، زیرا dictionary حافظه کمتری نسبت به ده‌ها فیلد delegate خالی مصرف می‌کند.
3️⃣ هنگام پیاده‌سازی explicit یک interface که رویدادی را تعریف کرده است.

نمونه‌ای برای مورد سوم:

```csharp
public interface IFoo { event EventHandler Ev; }

class Foo : IFoo
{
    private EventHandler ev;
    event EventHandler IFoo.Ev
    {
        add    { ev += value; }
        remove { ev -= value; }
    }
}
```

بخش‌های **add** و **remove** یک رویداد به متدهای **add\_XXX** و **remove\_XXX** کامپایل می‌شوند.

---

**Modifierهای رویدادها ⚡**

مثل متدها، رویدادها می‌توانند **virtual**، **overridden**، **abstract** یا **sealed** باشند. همچنین می‌توانند **static** باشند:

```csharp
public class Foo
{
    public static event EventHandler<EventArgs> StaticEvent;
    public virtual event EventHandler<EventArgs> VirtualEvent;
}
```

---

**Lambda Expressions λ**

یک **lambda expression** متدی بدون نام است که به جای یک instance delegate نوشته می‌شود. کامپایلر بلافاصله آن را به یکی از موارد زیر تبدیل می‌کند:

* یک instance delegate
* یا یک **expression tree** از نوع `Expression<TDelegate>` که کد داخل lambda را به صورت یک مدل شیء قابل پیمایش نمایش می‌دهد. این اجازه می‌دهد lambda بعداً در زمان اجرا تفسیر شود.

مثال:

```csharp
Transformer sqr = x => x * x;
Console.WriteLine(sqr(3));   // خروجی: 9
delegate int Transformer(int i);
```

کامپایلر lambdaهای این نوع را با نوشتن یک متد خصوصی و انتقال کد expression به آن متد حل می‌کند.

فرم کلی یک lambda:

```
(parameters) => expression-or-statement-block
```

اگر فقط یک پارامتر با نوع قابل استنتاج داشته باشیم، می‌توان پرانتزها را حذف کرد.

مثال:

```csharp
x => x * x;
```

هر پارامتر lambda متناظر با پارامتر delegate است و نوع expression (که ممکن است **void** باشد) متناظر با نوع بازگشتی delegate است.

می‌توان expression را به صورت یک بلوک statement نیز نوشت:

```csharp
x => { return x * x; };
```

اغلب lambdaها همراه با **Func** و **Action** استفاده می‌شوند:

```csharp
Func<int,int> sqr = x => x * x;
```

مثال با دو پارامتر:

```csharp
Func<string,string,int> totalLength = (s1, s2) => s1.Length + s2.Length;
int total = totalLength("hello", "world");   // total = 10
```

اگر نیازی به استفاده از پارامترها نیست، می‌توان آن‌ها را با underscore دور انداخت (از C# 9):

```csharp
Func<string,string,int> totalLength = (_,_) => ...
```

مثال بدون آرگومان:

```csharp
Func<string> greeter = () => "Hello, world";
```

از C# 10 به بعد، می‌توان از **implicit typing** برای lambda استفاده کرد:

```csharp
var greeter = () => "Hello, world";
```

### مشخص کردن صریح نوع پارامتر و نوع بازگشتی Lambda 🔧

کامپایلر معمولاً می‌تواند نوع پارامترهای lambda را به صورت زمینه‌ای استنتاج کند. وقتی این امکان وجود نداشته باشد، باید نوع هر پارامتر را به صورت صریح مشخص کنید. به دو متد زیر توجه کنید:

```csharp
void Foo<T>(T x) { }
void Bar<T>(Action<T> a) { }
```

کد زیر **کامپایل نمی‌شود**، زیرا کامپایلر نمی‌تواند نوع `x` را استنتاج کند:

```csharp
Bar(x => Foo(x));   // نوع x چیست؟
```

می‌توان با مشخص کردن صریح نوع `x` مشکل را حل کرد:

```csharp
Bar((int x) => Foo(x));
```

این مثال ساده را می‌توان به دو روش دیگر نیز اصلاح کرد:

```csharp
Bar<int>(x => Foo(x));  // مشخص کردن type parameter برای Bar
Bar<int>(Foo);          // همانند بالا، با استفاده از method group
```

مثالی دیگر از استفاده explicit برای نوع پارامتر (C# 10):

```csharp
var sqr = (int x) => x * x;
```

کامپایلر `sqr` را به نوع `Func<int,int>` استنتاج می‌کند. بدون مشخص کردن `int`، استنتاج نوع شکست می‌خورد: کامپایلر می‌داند که sqr باید از نوع `Func<T,T>` باشد، اما نمی‌داند T چیست.

از C# 10 به بعد می‌توان نوع بازگشتی lambda را نیز مشخص کرد:

```csharp
var sqr = int (int x) => x;
```

مشخص کردن نوع بازگشتی می‌تواند عملکرد کامپایلر را در lambdaهای پیچیده و تو در تو بهبود دهد.

---

### پارامترهای پیش‌فرض Lambda (C# 12) 🎯

مانند متدهای معمولی که می‌توانند پارامتر اختیاری داشته باشند:

```csharp
void Print(string message = "") => Console.WriteLine(message);
```

lambdaها نیز می‌توانند پارامتر اختیاری داشته باشند:

```csharp
var print = (string message = "") => Console.WriteLine(message);
print("Hello");
print();
```

این ویژگی برای کتابخانه‌هایی مانند **ASP.NET Minimal API** مفید است.

---

### دسترسی به متغیرهای خارجی (Outer Variables) 🌐

یک lambda می‌تواند به هر متغیری که در محل تعریفش قابل دسترسی است، ارجاع دهد. این متغیرها **outer variables** نامیده می‌شوند و می‌توانند شامل متغیرهای محلی، پارامترها و فیلدها باشند:

```csharp
int factor = 2;
Func<int, int> multiplier = n => n * factor;
Console.WriteLine(multiplier(3));   // خروجی: 6
```

متغیرهایی که توسط lambda ارجاع می‌شوند، **captured variables** نامیده می‌شوند. lambda که متغیرها را capture می‌کند، **closure** نامیده می‌شود.

متغیرهای capture شده هنگام فراخوانی delegate ارزیابی می‌شوند، نه هنگام capture شدن:

```csharp
int factor = 2;
Func<int, int> multiplier = n => n * factor;
factor = 10;
Console.WriteLine(multiplier(3));  // خروجی: 30
```

lambdaها می‌توانند خودشان متغیرهای capture شده را به‌روزرسانی کنند:

```csharp
int seed = 0;
Func<int> natural = () => seed++;
Console.WriteLine(natural());  // 0
Console.WriteLine(natural());  // 1
Console.WriteLine(seed);       // 2
```

متغیرهای capture شده طول عمرشان تا طول عمر delegate ادامه پیدا می‌کند. مثال:

```csharp
static Func<int> Natural()
{
    int seed = 0;
    return () => seed++;  // برمی‌گرداند یک closure
}

static void Main()
{
    Func<int> natural = Natural();
    Console.WriteLine(natural()); // 0
    Console.WriteLine(natural()); // 1
}
```

اگر متغیر محلی را داخل خود lambda بسازیم، هر فراخوانی delegate یک متغیر جدید ایجاد می‌کند:

```csharp
static Func<int> Natural()
{
    return () => { int seed = 0; return seed++; };
}

static void Main()
{
    Func<int> natural = Natural();
    Console.WriteLine(natural());  // 0
    Console.WriteLine(natural());  // 0
}
```

پیاده‌سازی capture داخلی با **hoisting** انجام می‌شود: متغیرهای capture شده به فیلدهای یک کلاس خصوصی منتقل می‌شوند و هنگام فراخوانی متد، کلاس ایجاد شده و به delegate وابسته می‌شود.

---

### Lambdaهای استاتیک ⚡

زمانی که lambda متغیرهای محلی، پارامترها، فیلدهای instance یا **this** را capture می‌کند، کامپایلر ممکن است کلاس خصوصی ایجاد کند تا ارجاع به داده‌ها ذخیره شود. این باعث مصرف حافظه می‌شود.

از C# 9 به بعد می‌توان با **static** کردن lambda، local function یا anonymous method، از capture شدن state جلوگیری کرد:

```csharp
Func<int, int> multiplier = static n => n * 2;
```

اگر بعداً lambda بخواهد متغیری را capture کند، کامپایلر خطا می‌دهد:

```csharp
int factor = 2;
Func<int, int> multiplier = static n => n * factor;  // کامپایل نمی‌شود
```

lambda بدون capture، یک instance delegate کش شده را مجدداً استفاده می‌کند و هزینه‌ای ندارد.

lambdaهای استاتیک هنوز می‌توانند به متغیرها و ثابت‌های static دسترسی داشته باشند. **static** تنها نقش بررسی دارد و تاثیری بر IL تولیدشده ندارد؛ بدون آن، کامپایلر در صورت نیاز closure تولید می‌کند، اما حتی آن زمان ترفندهایی برای کاهش هزینه دارد.

### گرفتن متغیرهای تکرار (Capturing iteration variables) 🔄

وقتی در یک حلقه‌ی `for` متغیر تکرار (iteration variable) را **Capture** می‌کنید، زبان C# با آن طوری رفتار می‌کند که انگار **بیرون از حلقه تعریف شده باشد**. یعنی در هر تکرار، همان متغیر گرفته می‌شود. به همین دلیل برنامه‌ی زیر به‌جای نمایش `012`، مقدار `333` را چاپ می‌کند:

```csharp
Action[] actions = new Action[3];
for (int i = 0; i < 3; i++)
    actions[i] = () => Console.Write(i);

foreach (Action a in actions) a();     // 333
```

هر **closure** (بخش پررنگ شده) همان متغیر `i` را می‌گیرد. این منطقی است، چون `i` متغیری است که مقدارش بین تکرارهای حلقه باقی می‌ماند؛ حتی می‌توانید داخل بدنه‌ی حلقه، مقدار `i` را تغییر دهید. نتیجه این است که وقتی **delegate**ها بعداً فراخوانی می‌شوند، همگی مقدار `i` در لحظه‌ی فراخوانی را می‌بینند، یعنی `3`. برای درک بهتر، حلقه را این‌طور بازنویسی کنید:

```csharp
Action[] actions = new Action[3];
int i = 0;
actions[0] = () => Console.Write(i);
i = 1;
actions[1] = () => Console.Write(i);
i = 2;
actions[2] = () => Console.Write(i);
i = 3;
foreach (Action a in actions) a();    // 333
```

راه‌حل برای نمایش `012` این است که متغیر تکرار را به یک **متغیر محلی جدید** که در همان محدوده‌ی حلقه قرار دارد، انتساب دهیم:

```csharp
Action[] actions = new Action[3];
for (int i = 0; i < 3; i++)
{
    int loopScopedi = i;
    actions[i] = () => Console.Write(loopScopedi);
}
foreach (Action a in actions) a();     // 012
```

چون در هر تکرار، یک متغیر جدید `loopScopedi` ساخته می‌شود، هر **closure** یک متغیر متفاوت را می‌گیرد. ✨

> **نکته:** قبل از نسخه‌ی C# 5.0، حلقه‌های `foreach` هم همین رفتار را داشتند و باعث سردرگمی می‌شدند. چون متغیر در `foreach` تغییرناپذیر است، انتظار می‌رفت محلی باشد، ولی نبود. خوشبختانه این موضوع اصلاح شده و حالا می‌توانید متغیرهای `foreach` را بدون نگرانی Capture کنید. ✅

---

### مقایسه‌ی **Lambda Expressions** و **Local Methods** 🆚

عملکرد **local methods** (بخش «Local methods» در صفحه 106) شباهت زیادی به **lambda expressions** دارد، اما سه مزیت دارند:

1. می‌توانند **بازگشتی (recursive)** باشند (خودشان را صدا بزنند) بدون نیاز به روش‌های پیچیده.
2. نیاز به مشخص کردن نوع delegate ندارند و کد شلوغ نمی‌شود.
3. **کارایی بیشتری** دارند چون سربار delegate را حذف می‌کنند.

**Local methods** بهینه‌تر هستند چون از واسطه‌ی delegate استفاده نمی‌کنند (این کار باعث صرف مقداری CPU و حافظه می‌شود). همچنین می‌توانند به متغیرهای محلی متد والد دسترسی داشته باشند بدون اینکه کامپایلر مجبور به **hoist کردن** آن‌ها در یک کلاس مخفی باشد.

اما در بسیاری از موارد شما به delegate نیاز دارید؛ مثل وقتی که می‌خواهید متدی با پارامتر از نوع delegate را صدا بزنید:

```csharp
public void Foo(Func<int, bool> predicate) { ... }
```

(نمونه‌های بیشتری در فصل 8 خواهید دید.) در این مواقع، استفاده از **lambda** معمولاً کوتاه‌تر و تمیزتر است.

---

### متدهای ناشناس (Anonymous Methods) 🕶️

**Anonymous methods** قابلیتی در C# 2.0 بودند که تا حد زیادی با **lambda expressions** در C# 3.0 جایگزین شدند. متدهای ناشناس مثل lambda هستند، اما امکانات زیر را ندارند:

* **پارامترهای نوع‌دهی ضمنی (implicitly typed)**
* **نوشتار به شکل expression** (همیشه باید به صورت بلوک بیانیه باشند)
* توانایی **تبدیل به Expression Tree** با `Expression<T>`

مثال:

```csharp
delegate int Transformer(int i);
Transformer sqr = delegate (int x) { return x * x; };
Console.WriteLine(sqr(3));  // 9
```

این معادل نوشتار زیر با lambda است:

```csharp
Transformer sqr = (int x) => { return x * x; };
// یا ساده‌تر:
Transformer sqr = x => x * x;
```

متدهای ناشناس هم مثل lambdaها متغیرهای بیرونی را Capture می‌کنند و حتی می‌توانند با کلمه‌ی کلیدی `static` شبیه lambdaهای استاتیک رفتار کنند.

ویژگی خاص آن‌ها این است که **می‌توان پارامترها را کاملاً حذف کرد**، حتی اگر delegate آن‌ها را انتظار داشته باشد. این برای تعریف event با handler خالی مفید است:

```csharp
public event EventHandler Clicked = delegate { };
```

این کار باعث می‌شود قبل از صدا زدن event نیازی به بررسی null نداشته باشید. همچنین نوشتن زیر هم معتبر است:

```csharp
// پارامترها حذف شده‌اند:
Clicked += delegate { Console.WriteLine("clicked"); };
```

---

### دستورات try و مدیریت استثناها (try Statements and Exceptions) ⚠️

یک دستور `try` یک بلوک کد را مشخص می‌کند که ممکن است خطا رخ دهد یا نیاز به پاکسازی داشته باشد. بلوک `try` باید حداقل با یک `catch` یا یک `finally` (یا هر دو) همراه باشد:

* **catch** زمانی اجرا می‌شود که خطا در بلوک try رخ دهد.
* **finally** همیشه بعد از خروج از try (یا catch) اجرا می‌شود و معمولاً برای کارهای پاکسازی مثل بستن اتصالات شبکه است.

`catch` به شیء `Exception` دسترسی دارد که اطلاعات خطا را نگه می‌دارد. شما می‌توانید خطا را مدیریت کنید یا دوباره پرتاب کنید (برای لاگ کردن یا بالا بردن سطح خطا).

مثال:

```csharp
try
{
    // ممکن است خطا رخ دهد
}
catch (ExceptionA ex)
{
    // مدیریت خطای نوع ExceptionA
}
catch (ExceptionB ex)
{
    // مدیریت خطای نوع ExceptionB
}
finally
{
    // پاکسازی
}
```

برنامه‌ی زیر را در نظر بگیرید:

```csharp
int y = Calc(0);
Console.WriteLine(y);

int Calc(int x) => 10 / x;
```

چون `x` صفر است، خطای `DivideByZeroException` رخ می‌دهد و برنامه متوقف می‌شود. حالا با `try/catch`:

```csharp
try
{
    int y = Calc(0);
    Console.WriteLine(y);
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("x cannot be zero");
}
Console.WriteLine("program completed");

int Calc(int x) => 10 / x;
```

خروجی:

```
x cannot be zero
program completed
```

در عمل، بهتر است **خطاهای قابل پیشگیری را قبل از وقوع بررسی کنید**، مثلاً تقسیم بر صفر را چک کنید، چون **exceptionها هزینه‌بر هستند** (صدها سیکل پردازنده یا بیشتر).

وقتی exception رخ می‌دهد، CLR بررسی می‌کند:

* اگر `catch` سازگار پیدا شد: همان را اجرا می‌کند، سپس `finally` و برنامه ادامه می‌یابد.
* اگر پیدا نشد: `finally` اجرا می‌شود (اگر باشد) و CLR به عقب در stack دنبال `try` می‌گردد.
* اگر هیچ تابعی مسئولیت نگرفت: برنامه متوقف می‌شود.

---

### بخش catch (بخش گرفتن استثناها) 🛡️

یک **catch clause** مشخص می‌کند که چه نوع استثنایی (**Exception**) باید گرفته شود. این نوع باید یا **System.Exception** باشد یا یک زیرکلاس از **System.Exception**.

اگر **System.Exception** را بگیرید، تمام خطاهای ممکن را خواهید گرفت. این کار در شرایط زیر مفید است:

* وقتی برنامه‌تان می‌تواند بدون توجه به نوع خاص استثنا، بهبود یابد.
* وقتی قصد دارید استثنا را دوباره پرتاب کنید (مثلاً بعد از ثبت یا **Log** کردن آن).
* وقتی هندلر خطای شما آخرین خط دفاعی قبل از پایان برنامه است.

اما در حالت معمول، بهتر است نوع‌های خاص استثنا را بگیرید تا مجبور نشوید با شرایطی روبه‌رو شوید که هندلر شما برای آن طراحی نشده است (مثلاً **OutOfMemoryException**).

برای مدیریت چند نوع استثنا، می‌توانید از چندین بخش **catch** استفاده کنید (هرچند در این مثال می‌توان به جای مدیریت استثنا، بررسی ورودی‌ها را انجام داد):

```csharp
class Test
{
  static void Main (string[] args)
  {
    try
    {
      byte b = byte.Parse (args[0]);
      Console.WriteLine (b);
    }
    catch (IndexOutOfRangeException)
    {
      Console.WriteLine ("Please provide at least one argument");
    }
    catch (FormatException)
    {
      Console.WriteLine ("That's not a number!");
    }
    catch (OverflowException)
    {
      Console.WriteLine ("You've given me more than a byte!");
    }
  }
}
```

برای هر استثنا فقط یک **catch** اجرا می‌شود. اگر می‌خواهید یک هندلر کلی مثل **System.Exception** داشته باشید، باید هندلرهای خاص‌تر را قبل از آن قرار دهید.

گاهی نیاز ندارید به ویژگی‌های استثنا دسترسی داشته باشید. در این حالت می‌توانید متغیر را حذف کنید:

```csharp
catch (OverflowException)   // بدون متغیر
{
  ...
}
```

حتی می‌توانید هم متغیر و هم نوع را حذف کنید (به این معنی که همه استثناها گرفته می‌شوند):

```csharp
catch { ... }
```

---

### Exception filters (فیلترهای استثنا) 🔍

می‌توانید در بخش **catch** یک فیلتر استثنا با استفاده از کلمه **when** مشخص کنید:

```csharp
catch (WebException ex) when (ex.Status == WebExceptionStatus.Timeout)
{
  ...
}
```

در این مثال، اگر **WebException** پرتاب شود، عبارت بولی بعد از **when** ارزیابی می‌شود. اگر نتیجه **false** باشد، این **catch** نادیده گرفته شده و به سراغ **catch**های بعدی می‌رود.

با **exception filters** می‌توان یک نوع استثنا را چند بار با شرایط متفاوت گرفت:

```csharp
catch (WebException ex) when (ex.Status == WebExceptionStatus.Timeout)
{ ... }
catch (WebException ex) when (ex.Status == WebExceptionStatus.SendFailure)
{ ... }
```

عبارت بولی در **when** حتی می‌تواند شامل متدهایی باشد که عملیات جانبی انجام می‌دهند، مانند ثبت خطا برای اهداف عیب‌یابی.

---

### بخش finally (بخش پایانی) 🧹

بخش **finally** همیشه اجرا می‌شود—چه استثنا رخ دهد چه نه، و چه **try** به طور کامل اجرا شود یا خیر. معمولاً از **finally** برای کدهای پاک‌سازی استفاده می‌کنیم.

بخش **finally** بعد از هر یک از این حالت‌ها اجرا می‌شود:

* بعد از اتمام یک **catch** (یا زمانی که یک استثنای جدید پرتاب شود).
* بعد از اتمام بلوک **try** (یا زمانی که استثنایی رخ دهد که هندلری برایش وجود ندارد).
* زمانی که کنترل با یک دستور پرش (مثل **return** یا **goto**) از بلوک **try** خارج شود.

تنها مواردی که می‌توانند مانع اجرای **finally** شوند، یک حلقه بی‌نهایت یا پایان ناگهانی پردازش است.

بخش **finally** به برنامه‌تان نظم و قطعیت اضافه می‌کند. در مثال زیر، فایلی که باز شده همیشه بسته می‌شود، صرف‌نظر از اینکه:

* بلوک **try** به طور عادی تمام شود.
* اجرا به دلیل خالی بودن فایل (**EndOfStream**) زودتر بازگردد.
* یک **IOException** هنگام خواندن فایل رخ دهد:

```csharp
void ReadFile()
{
  StreamReader reader = null;    // در فضای نام System.IO
  try
  {
    reader = File.OpenText ("file.txt");
    if (reader.EndOfStream) return;
    Console.WriteLine (reader.ReadToEnd());
  }
  finally
  {
    if (reader != null) reader.Dispose();
  }
}
```

در این مثال، فایل را با فراخوانی **Dispose** روی **StreamReader** بستیم. فراخوانی **Dispose** روی یک شیء در داخل **finally** یک روش استاندارد است و در #C با دستور **using** نیز پشتیبانی می‌شود.

---

### دستور `using` ♻️

بسیاری از کلاس‌ها منابع مدیریت‌نشده (Unmanaged Resources) مانند **دستگیره‌های فایل (File Handles)**، **دستگیره‌های گرافیکی (Graphics Handles)** یا **اتصالات پایگاه داده (Database Connections)** را در خود جای می‌دهند. این کلاس‌ها اینترفیس **`System.IDisposable`** را پیاده‌سازی می‌کنند که تنها یک متد بدون پارامتر به نام **`Dispose`** دارد و برای پاک‌سازی این منابع استفاده می‌شود.

دستور **`using`** یک نگارش ساده و شکیل برای فراخوانی **`Dispose`** روی یک شیء پیاده‌ساز **`IDisposable`**، درون یک بلوک **`finally`** فراهم می‌کند.

به‌عنوان مثال:

```csharp
using (StreamReader reader = File.OpenText("file.txt"))
{
    ...
}
```

این قطعه‌کد دقیقاً معادل زیر است:

```csharp
StreamReader reader = File.OpenText("file.txt");
try
{
    ...
}
finally
{
    if (reader != null)
        ((IDisposable)reader).Dispose();
}
```

---

### اعلان‌های `using` (Using Declarations) ✍️

اگر در **C# 8 و نسخه‌های بالاتر**، براکت‌ها و بلوک کد بعد از دستور **`using`** حذف شوند، این دستور تبدیل به **اعلان using** می‌شود. در این حالت، منبع زمانی آزاد می‌شود که اجرای برنامه از بلوک محصورکننده خارج گردد.

مثال:

```csharp
if (File.Exists("file.txt"))
{
    using var reader = File.OpenText("file.txt");
    Console.WriteLine(reader.ReadLine());
    ...
}
```

در این حالت، **`reader`** زمانی Dispose می‌شود که اجرای برنامه از بلوک **`if`** خارج شود.

---

### پرتاب استثنا (Throwing Exceptions) 🚀

استثناها (Exceptions) می‌توانند هم توسط **زمان اجرا (Runtime)** و هم در **کدهای کاربر** پرتاب شوند. در مثال زیر، متد **`Display`** یک استثنای **`System.ArgumentNullException`** را پرتاب می‌کند:

```csharp
try { Display(null); }
catch (ArgumentNullException ex)
{
    Console.WriteLine("Caught the exception");
}

void Display(string name)
{
    if (name == null)
        throw new ArgumentNullException(nameof(name));
    Console.WriteLine(name);
}
```

از آن‌جایی که بررسی آرگومان برای مقدار **null** و پرتاب **`ArgumentNullException`** بسیار رایج است، از **.NET 6** یک میان‌بر ارائه شده است:

```csharp
void Display(string name)
{
    ArgumentNullException.ThrowIfNull(name);
    Console.WriteLine(name);
}
```

توجه کنید که در این روش، نیازی به مشخص کردن نام پارامتر نداریم. دلیل این موضوع در بخش **CallerArgumentExpression** (صفحه 247) توضیح داده خواهد شد.

---

### عبارت‌های `throw` (Throw Expressions) 🎯

عبارت **`throw`** می‌تواند به‌عنوان یک **عبارت (Expression)** در متدهای **Expression-bodied** استفاده شود:

```csharp
public string Foo() => throw new NotImplementedException();
```

همچنین می‌تواند در یک **عبارت شرطی سه‌تایی (Ternary Conditional Expression)** ظاهر شود:

```csharp
string ProperCase(string value) =>
    value == null ? throw new ArgumentException("value") :
    value == "" ? "" :
    char.ToUpper(value[0]) + value.Substring(1);
```

---

### پرتاب دوباره استثنا (Rethrowing an Exception) 🔄

می‌توانید یک استثنا را گرفته و دوباره پرتاب کنید:

```csharp
try
{
    ...
}
catch (Exception ex)
{
    // Log error
    ...
    throw; // پرتاب دوباره همان استثنا
}
```

اگر به‌جای **`throw`** از **`throw ex`** استفاده کنیم، برنامه همچنان کار می‌کند اما خاصیت **`StackTrace`** دیگر مسیر خطای اصلی را نشان نمی‌دهد.

پرتاب دوباره به شما اجازه می‌دهد خطا را **ثبت (Log)** کنید بدون اینکه آن را نادیده بگیرید، یا زمانی که شرایط فراتر از انتظار است، از ادامه مدیریت خطا صرف‌نظر کنید.

یکی دیگر از سناریوهای رایج، پرتاب یک استثنای **خاص‌تر** است:

```csharp
try
{
    // Parse a DateTime from XML element data
}
catch (FormatException ex)
{
    throw new XmlException("Invalid DateTime", ex);
}
```

دقت کنید که هنگام ساخت **`XmlException`**، استثنای اصلی **`ex`** را به‌عنوان آرگومان دوم پاس دادیم. این آرگومان خاصیت **`InnerException`** را مقداردهی می‌کند و در اشکال‌زدایی کمک زیادی می‌کند. تقریباً همه انواع استثنا چنین سازنده‌ای دارند.

---

### پرتاب یک استثنای کلی‌تر (Less-Specific Exception)

این روش زمانی مفید است که در حال عبور از یک **مرز اعتماد (Trust Boundary)** هستید تا از افشای اطلاعات فنی برای مهاجمان بالقوه جلوگیری کنید.

---

### ویژگی‌های کلیدی **System.Exception** ⚙️

مهم‌ترین ویژگی‌های **System.Exception** به شرح زیر هستند:

* **StackTrace**
  رشته‌ای (**string**) که تمام متدهایی را که از نقطه شروع رخداد استثنا تا بلوک **catch** فراخوانی شده‌اند، نمایش می‌دهد.

* **Message**
  رشته‌ای که توضیح خطا را در خود نگه می‌دارد.

* **InnerException**
  استثنای داخلی (در صورت وجود) که باعث ایجاد استثنای بیرونی شده است. این استثنا خود می‌تواند شامل **InnerException** دیگری نیز باشد.

> در زبان C# تمام استثناها در زمان اجرا (**runtime exceptions**) اتفاق می‌افتند و معادلی برای استثناهای بررسی‌شده در زمان کامپایل (**compile-time checked exceptions**) مانند Java وجود ندارد.

---

### انواع رایج استثناها 🚨

انواع زیر از استثناها به‌طور گسترده در سراسر **CLR** و کتابخانه‌های **.NET** استفاده می‌شوند. شما می‌توانید آن‌ها را خودتان پرتاب کنید یا از آن‌ها به‌عنوان کلاس پایه برای ساخت انواع سفارشی استثنا استفاده نمایید:

* **System.ArgumentException**
  زمانی پرتاب می‌شود که یک تابع با آرگومان نامعتبر فراخوانی شود. معمولاً نشان‌دهنده یک خطای برنامه‌نویسی است.

* **System.ArgumentNullException**
  زیرکلاس **ArgumentException** که وقتی یک آرگومان تابع به‌طور غیرمنتظره **null** باشد، پرتاب می‌شود.

* **System.ArgumentOutOfRangeException**
  زیرکلاس **ArgumentException** که وقتی یک آرگومان (معمولاً عددی) خیلی بزرگ یا خیلی کوچک باشد، پرتاب می‌شود. برای مثال، ارسال یک عدد منفی به تابعی که فقط مقادیر مثبت را می‌پذیرد.

* **System.InvalidOperationException**
  زمانی پرتاب می‌شود که وضعیت یک شیء برای اجرای موفقیت‌آمیز متد مناسب نباشد، بدون توجه به مقدار آرگومان‌ها. مثال‌ها شامل خواندن یک فایل بازنشده یا دریافت عنصر بعدی از یک شمارنده (**Enumerator**) است که لیست زیرین آن در میانه اجرا تغییر کرده است.

* **System.NotSupportedException**
  زمانی پرتاب می‌شود که یک قابلیت خاص پشتیبانی نمی‌شود. مثالی مناسب: فراخوانی متد **Add** روی مجموعه‌ای که **IsReadOnly** آن **true** است.

* **System.NotImplementedException**
  زمانی پرتاب می‌شود که یک تابع هنوز پیاده‌سازی نشده است.

* **System.ObjectDisposedException**
  زمانی پرتاب می‌شود که روی شیئی که قبلاً **Dispose** شده، متدی فراخوانی شود.

یکی دیگر از استثناهای رایج **NullReferenceException** است. این استثنا توسط **CLR** زمانی پرتاب می‌شود که تلاش کنید به عضوی از شیئی که مقدار آن **null** است دسترسی پیدا کنید (که نشان‌دهنده وجود باگ در کد شماست). برای تست، می‌توانید به‌طور مستقیم این استثنا را پرتاب کنید:

```csharp
throw null;
```

---

### الگوی متدهای **TryXXX** 🔄

هنگام نوشتن یک متد، زمانی که مشکلی پیش می‌آید، دو انتخاب دارید: یا یک کد خطا برگردانید یا یک استثنا پرتاب کنید. به‌طور کلی، زمانی که خطا خارج از روند عادی برنامه باشد یا زمانی که انتظار ندارید فراخواننده بتواند با آن مقابله کند، استثنا پرتاب می‌کنید.

بااین‌حال، گاهی بهتر است هر دو انتخاب را به مصرف‌کننده ارائه دهید. مثالی از این مورد نوع **int** است که دو نسخه از متد **Parse** را ارائه می‌دهد:

```csharp
public int Parse(string input);
public bool TryParse(string input, out int returnValue);
```

اگر **Parse** شکست بخورد، یک استثنا پرتاب می‌کند؛ اما **TryParse** در این حالت مقدار **false** برمی‌گرداند.

می‌توانید این الگو را با این روش پیاده‌سازی کنید که متد **XXX** در نهایت متد **TryXXX** را فراخوانی کند:

```csharp
public return-type XXX(input-type input)
{
    return-type returnValue;
    if (!TryXXX(input, out returnValue))
        throw new YYYException(...);
    return returnValue;
}
```

---

### جایگزین‌های استثناها 🛠️

همانند **int.TryParse**، یک تابع می‌تواند با برگرداندن یک کد خطا از طریق مقدار بازگشتی یا پارامتر به تابع فراخواننده، شکست را اطلاع دهد. اگرچه این روش برای خطاهای ساده و قابل پیش‌بینی کارآمد است، اما هنگام مواجهه با خطاهای غیرمعمول یا غیرقابل پیش‌بینی دست‌وپاگیر می‌شود، امضای متدها را شلوغ می‌کند و پیچیدگی‌های غیرضروری ایجاد می‌نماید.

همچنین این روش برای توابعی که متد نیستند (مانند عملگرها مثل عملگر تقسیم یا ویژگی‌ها **Properties**) قابل استفاده نیست. یک جایگزین دیگر قرار دادن خطا در یک محل مشترک است که تمام توابع در زنجیره فراخوانی بتوانند آن را ببینند (مثلاً یک متد **static** که خطای فعلی را در هر نخ ذخیره کند). با این حال، این روش نیازمند مشارکت هر تابع در الگوی انتشار خطا است که هم دست‌وپاگیر و هم مستعد خطا خواهد بود.

---

### شمارش (Enumeration) و پیمایشگرها (Iterators) 🔄

#### شمارش (Enumeration)

**Enumerator (شمارش‌گر)** یک مکان‌نما (cursor) **فقط خواندنی** و **فقط رو به جلو** روی یک دنباله از مقادیر است. در زبان #C، یک نوع (type) به‌عنوان شمارش‌گر شناخته می‌شود اگر یکی از شرایط زیر را داشته باشد:

* یک متد عمومی (public) بدون پارامتر به نام `MoveNext` و یک ویژگی (property) به نام `Current` داشته باشد.
* واسط `System.Collections.Generic.IEnumerator<T>` را پیاده‌سازی کند.
* واسط `System.Collections.IEnumerator` را پیاده‌سازی کند.

**عبارت foreach** روی یک **Enumerable object (شیء شمارش‌پذیر)** پیمایش می‌کند.
یک **Enumerable object** نمایش منطقی یک دنباله است. این شیء خودش مکان‌نما نیست، بلکه **مکان‌نما تولید می‌کند**. در #C، یک نوع به‌عنوان شمارش‌پذیر شناخته می‌شود اگر یکی از شرایط زیر را داشته باشد (بررسی‌ها به همین ترتیب انجام می‌شود):

* یک متد عمومی بدون پارامتر به نام `GetEnumerator` داشته باشد که یک شمارش‌گر برگرداند.
* واسط `System.Collections.Generic.IEnumerable<T>` را پیاده‌سازی کند.
* واسط `System.Collections.IEnumerable` را پیاده‌سازی کند.
* (از #C نسخه 9 به بعد) بتواند به یک متد توسعه‌ای (extension method) به نام `GetEnumerator` که یک شمارش‌گر برمی‌گرداند، متصل شود (بخش **"Extension Methods"** در صفحه 217 را ببینید).

**الگوی شمارش** به شکل زیر است:

```csharp
class Enumerator   // معمولاً واسط IEnumerator یا IEnumerator<T> را پیاده‌سازی می‌کند
{
  public IteratorVariableType Current { get {...} }
  public bool MoveNext() {...}
}

class Enumerable   // معمولاً واسط IEnumerable یا IEnumerable<T> را پیاده‌سازی می‌کند
{
  public Enumerator GetEnumerator() {...}
}
```

**نمونه پیمایش سطح بالا** روی کاراکترهای کلمه `"beer"` با استفاده از `foreach`:

```csharp
foreach (char c in "beer")
  Console.WriteLine(c);
```

**نمونه پیمایش سطح پایین** روی کاراکترهای `"beer"` بدون استفاده از `foreach`:

```csharp
using (var enumerator = "beer".GetEnumerator())
  while (enumerator.MoveNext())
  {
    var element = enumerator.Current;
    Console.WriteLine(element);
  }
```

> اگر شمارش‌گر واسط `IDisposable` را پیاده‌سازی کند، عبارت `foreach` مانند یک عبارت `using` عمل کرده و **به‌طور ضمنی** شیء شمارش‌گر را آزاد (dispose) می‌کند.

جزئیات بیشتر در مورد واسط‌های شمارش در **فصل 7** توضیح داده شده است.

---

#### مقداردهی اولیه مجموعه‌ها (Collection Initializers) و عبارات مجموعه‌ای (Collection Expressions) 📝

شما می‌توانید در یک مرحله، یک شیء شمارش‌پذیر را ایجاد و مقداردهی کنید:

```csharp
using System.Collections.Generic;
var list = new List<int> { 1, 2, 3 };
```

از نسخه #C 12 به بعد، می‌توانید این کار را کوتاه‌تر انجام دهید (با استفاده از **براکت‌ها**):

```csharp
using System.Collections.Generic;
List<int> list = [1, 2, 3];
```

**عبارات مجموعه‌ای** **هدف‌نوعی (target-typed)** هستند؛ یعنی نوع `[1, 2, 3]` به نوع متغیری که به آن انتساب داده می‌شود بستگی دارد. مثال:

```csharp
int[] array = [1, 2, 3];
Span<int> span = [1, 2, 3];
```

حتی می‌توانید هنگام فراخوانی متدها هم نوع را حذف کنید اگر کامپایلر بتواند آن را استنباط کند:

```csharp
Foo([1, 2, 3]);

void Foo(List<int> numbers) { ... }
```

کامپایلر این کد را به این شکل ترجمه می‌کند:

```csharp
using System.Collections.Generic;
List<int> list = new List<int>();
list.Add(1);
list.Add(2);
list.Add(3);
```

این موضوع نیازمند این است که شیء شمارش‌پذیر واسط `System.Collections.IEnumerable` را پیاده‌سازی کند و یک متد `Add` با تعداد پارامتر مناسب داشته باشد. (در عبارات مجموعه‌ای، کامپایلر از الگوهای دیگر هم برای ایجاد مجموعه‌های فقط خواندنی پشتیبانی می‌کند.)

همچنین می‌توانید دیکشنری‌ها را هم به همین شکل مقداردهی کنید (بخش **"Dictionaries"** در صفحه 394 را ببینید):

```csharp
var dict = new Dictionary<int, string>()
{
  { 5, "five" },
  { 10, "ten" }
};
```

یا به شکل کوتاه‌تر:

```csharp
var dict = new Dictionary<int, string>()
{
  [3] = "three",
  [10] = "ten"
};
```

این روش نه تنها برای دیکشنری‌ها، بلکه برای هر نوعی که **Indexer** داشته باشد، معتبر است.

---

#### پیمایشگرها (Iterators) ⚙️

در حالی که عبارت `foreach` **مصرف‌کننده** یک شمارش‌گر است، **Iterator (پیمایشگر)** **تولیدکننده** یک شمارش‌گر است.
مثال زیر یک پیمایشگر است که یک دنباله از اعداد فیبوناچی را تولید می‌کند (هر عدد حاصل جمع دو عدد قبلی است):

```csharp
using System;
using System.Collections.Generic;
foreach (int fib in Fibs(6))
  Console.Write(fib + "  ");

IEnumerable<int> Fibs(int fibCount)
{
  for (int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++)
  {
    yield return prevFib;
    int newFib = prevFib + curFib;
    prevFib = curFib;
    curFib = newFib;
  }
}
```

**خروجی:**

```
1  1  2  3  5  8
```

در حالی که دستور `return` می‌گوید: **"این مقداری است که از این متد خواسته بودی"**، دستور `yield return` می‌گوید: **"این عنصر بعدی است که از این شمارش‌گر خواسته بودی"**.
در هر دستور `yield`، کنترل به فراخواننده برمی‌گردد، اما **وضعیت متد حفظ می‌شود** تا وقتی فراخواننده عنصر بعدی را درخواست کرد، متد از همان‌جا ادامه یابد. این وضعیت به عمر شمارش‌گر وابسته است و بعد از اتمام پیمایش آزاد می‌شود.

کامپایلر متدهای پیمایشگر را به کلاس‌های خصوصی تبدیل می‌کند که واسط‌های `IEnumerable<T>` و/یا `IEnumerator<T>` را پیاده‌سازی می‌کنند.
منطق موجود در بلوک پیمایشگر در متد `MoveNext` و ویژگی `Current` کلاس تولیدشده توسط کامپایلر قرار داده می‌شود. **این یعنی وقتی متد پیمایشگر را صدا می‌زنید، هیچ کدی اجرا نمی‌شود؛ تنها یک نمونه از کلاس ساخته می‌شود!**
کد شما تنها وقتی اجرا می‌شود که پیمایش شروع شود، معمولاً با یک عبارت `foreach`.

> پیمایشگرها می‌توانند متدهای محلی (local methods) هم باشند (بخش **"Local methods"** در صفحه 106 را ببینید).

---

### معنای **Iterator** (تکرارکننده) 🔄

یک **Iterator** یا «تکرارکننده» متدی، ویژگی (Property) یا ایندکسری است که شامل یک یا چند دستور `yield` است. یک **Iterator** باید یکی از چهار رابط (Interface) زیر را برگرداند، در غیر این صورت کامپایلر خطا تولید می‌کند:

```csharp
// رابط‌های Enumerable
System.Collections.IEnumerable
System.Collections.Generic.IEnumerable<T>

// رابط‌های Enumerator
System.Collections.IEnumerator
System.Collections.Generic.IEnumerator<T>
```

**Iterator** بسته به اینکه یک رابط **Enumerable** یا **Enumerator** برمی‌گرداند، رفتار متفاوتی دارد. توضیح کامل این موضوع در فصل ۷ آمده است.

---

### استفاده از چندین دستور `yield`

در یک **Iterator** می‌توان چندین دستور `yield` استفاده کرد:

```csharp
foreach (string s in Foo())
    Console.WriteLine(s); // چاپ می‌کند: "One", "Two", "Three"

IEnumerable<string> Foo()
{
    yield return "One";
    yield return "Two";
    yield return "Three";
}
```

---

### استفاده از `yield break`

در یک بلوک **Iterator** استفاده از دستور `return` مجاز نیست. برای خروج زودهنگام از **Iterator** (بدون برگرداندن عناصر بیشتر) باید از `yield break` استفاده کنید:

```csharp
IEnumerable<string> Foo(bool breakEarly)
{
    yield return "One";
    yield return "Two";
    if (breakEarly)
        yield break;
    yield return "Three";
}
```

---

### **Iteratorها** و بلوک‌های **try/catch/finally** ⚠️

* استفاده از `yield return` در یک بلوک `try` که شامل بخش `catch` باشد، **مجاز نیست**:

```csharp
IEnumerable<string> Foo()
{
    try { yield return "One"; } // غیرمجاز
    catch { ... }
}
```

* همچنین استفاده از `yield return` در بخش‌های `catch` یا `finally` نیز مجاز نیست.
  دلیل این محدودیت‌ها این است که کامپایلر باید **Iteratorها** را به کلاس‌های معمولی با متدهای `MoveNext`، `Current` و `Dispose` تبدیل کند و مدیریت بلاک‌های خطا پیچیدگی زیادی ایجاد می‌کند.

* اما می‌توانید در بلوک `try` که **فقط** شامل یک بخش `finally` است از `yield return` استفاده کنید:

```csharp
IEnumerable<string> Foo()
{
    try { yield return "One"; } // مجاز
    finally { ... }
}
```

کد موجود در بلوک `finally` زمانی اجرا می‌شود که شمارنده (**Enumerator**) مصرف‌کننده به انتهای توالی برسد یا از بین برود. دستور `foreach` به‌صورت ضمنی شمارنده را Dispose می‌کند اگر زودتر از حلقه خارج شوید، بنابراین این روش امنی برای استفاده از شمارنده‌هاست.

---

### احتیاط هنگام استفاده از **Enumeratorها** به‌صورت دستی 🔍

اگر شمارنده را به‌صورت دستی استفاده می‌کنید و قبل از پایان کار آن را رها کنید بدون اینکه Dispose کنید، بلوک `finally` اجرا نمی‌شود. برای جلوگیری از این مشکل، شمارنده‌ها را درون یک دستور `using` قرار دهید:

```csharp
string firstElement = null;
var sequence = Foo();
using (var enumerator = sequence.GetEnumerator())
    if (enumerator.MoveNext())
        firstElement = enumerator.Current;
```

---

### ترکیب توالی‌ها (Composing Sequences) 🧩

**Iteratorها** قابلیت ترکیب بالایی دارند. مثال زیر تنها اعداد **فیبوناچی زوج** را تولید می‌کند:

```csharp
using System;
using System.Collections.Generic;

foreach (int fib in EvenNumbersOnly(Fibs(6)))
    Console.WriteLine(fib);

IEnumerable<int> Fibs(int fibCount)
{
    for (int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++)
    {
        yield return prevFib;
        int newFib = prevFib + curFib;
        prevFib = curFib;
        curFib = newFib;
    }
}

IEnumerable<int> EvenNumbersOnly(IEnumerable<int> sequence)
{
    foreach (int x in sequence)
        if ((x % 2) == 0)
            yield return x;
}
```

نکته مهم این است که **هر عنصر دقیقاً زمانی محاسبه می‌شود که درخواست شود**، یعنی فقط هنگام فراخوانی متد `MoveNext()` مقدار جدید تولید می‌شود. (شکل ۴-۱ فرآیند درخواست و خروجی داده‌ها را در طول زمان نشان می‌دهد.)

<div align="center">
    
![Conventions-UsedThis-Book](../../assets/image/04/Table-4-1.jpeg) 
</div>
