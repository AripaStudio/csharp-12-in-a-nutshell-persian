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
وقتی از دلیگیت‌ها استفاده می‌کنید، معمولاً دو نقش جدید پدیدار می‌شود: پخش‌کننده (**broadcaster**) و مشترک (**subscriber**).

پخش‌کننده نوعی است که یک فیلد دلیگیت دارد. پخش‌کننده با فراخوانی دلیگیت، زمان پخش (broadcast) را تعیین می‌کند.

مشترکین گیرندگان متد هدف هستند. یک مشترک با فراخوانی += و -= روی دلیگیت پخش‌کننده، تصمیم می‌گیرد چه زمانی به گوش دادن (**listening**) شروع یا پایان دهد. یک مشترک از وجود سایر مشترکین خبر ندارد و در کار آن‌ها دخالتی نمی‌کند.

رویدادها (**Events**) یک ویژگی زبانی هستند که این الگو را رسمی می‌کنند. یک رویداد ساختاری است که فقط زیرمجموعه‌ای از ویژگی‌های دلیگیت را که برای مدل پخش‌کننده/مشترک لازم است، آشکار می‌کند. هدف اصلی رویدادها این است که از تداخل مشترکین با یکدیگر جلوگیری کنند.

ساده‌ترین راه برای اعلان یک رویداد، قرار دادن کلمه کلیدی `event` قبل از یک عضو دلیگیت است:

```C#

// Delegate definition
public delegate void PriceChangedHandler (decimal oldPrice,
                                         decimal newPrice);

public class Broadcaster
{
  // Event declaration
  public event PriceChangedHandler PriceChanged;
}
```
کد درون نوع `Broadcaster` دسترسی کامل به `PriceChanged` دارد و می‌تواند با آن به عنوان یک دلیگیت رفتار کند. اما کدی که در خارج از `Broadcaster` قرار دارد، تنها می‌تواند عملیات += و -= را روی رویداد `PriceChanged` انجام دهد.

<div align="center">
    
![Conventions-UsedThis-Book](../../assets/image/04/Table-2-1.jpeg) 
</div>


