# آموزش ساخت View و Layout

## 1. آشنایی با View در معماری MVC

در معماری MVC، بخش View وظیفه نمایش اطلاعات به کاربر را بر عهده دارد.

در واقع، پس از اینکه درخواست کاربر توسط Router به کنترلر مناسب هدایت شد و کنترلر عملیات مورد نیاز را انجام داد، می‌تواند اطلاعات مورد نظر را به یک فایل View ارسال کند تا خروجی HTML به کاربر نمایش داده شود.

برای مثال، فرض کنید کاربر صفحه اصلی سایت را درخواست می‌کند. در این حالت:

1. درخواست کاربر توسط Router دریافت می‌شود.
2. Router کنترلر و متد مربوطه را اجرا می‌کند.
3. کنترلر اطلاعات مورد نیاز را آماده می‌کند.
4. کنترلر فایل View مناسب را فراخوانی می‌کند.
5. فایل View خروجی HTML را تولید می‌کند.

## 2. ایجاد کلاس View

برای مدیریت فایل‌های View، ابتدا دایرکتوری `Libraries` را در مسیر `core` ایجاد کرده و سپس کلاس `View` را در آن قرار می‌دهیم.

مسیر فایل:

```text
core/Libraries/View.php
```

محتوای کلاس:

```php
<?php

namespace Core\Libraries;

class View
{
    public function render(string $view, array $data = []): void
    {
        $viewFile = str_replace('.', '/', $view) . '.php';

        extract($data, EXTR_SKIP);
        require BASE_DIR . '/app/Views/' . $viewFile;
    }
}
```

### بررسی کد کلاس View

**متد render**

این متد وظیفه دریافت نام فایل View و اطلاعات مورد نیاز آن را بر عهده دارد.

```php
public function render(string $view, array $data = []): void
```

این متد دو پارامتر دریافت می‌کند:

- `$view`: نام فایل View مورد نظر.
- `$data`: آرایه‌ای از اطلاعاتی که قرار است به فایل View ارسال شوند. این پارامتر اختیاری است.

**تبدیل نقطه به اسلش**

```php
$viewFile = str_replace('.', '/', $view) . '.php';
```

برای سازماندهی بهتر فایل‌های View، از دایرکتوری‌های تو در تو استفاده می‌کنیم.

به همین دلیل، به جای اینکه مسیر کامل فایل را ارسال کنیم، فقط نام View را به شکل زیر مشخص می‌کنیم:

```php
home.index
```

دستور `str_replace` نقطه را با اسلش جایگزین می‌کند و در نهایت پسوند `.php` را به آن اضافه می‌کند.

نتیجه:

```text
home/index.php
```

**تبدیل آرایه اطلاعات به متغیرهای قابل استفاده در View**

```php
extract($data, EXTR_SKIP);
```

تابع `extract` کلیدهای آرایه را به متغیرهای مستقل تبدیل می‌کند.

برای مثال، اگر آرایه زیر را داشته باشیم:

```php
$data = [
    'name' => 'Ali',
    'age' => 25
];
```

با اجرای `extract` دو متغیر زیر ایجاد می‌شوند:

```php
$name = 'Ali';
$age = 25;
```

پارامتر `EXTR_SKIP` باعث می‌شود اگر متغیری با همان نام از قبل وجود داشته باشد، مقدار آن بازنویسی نشود.

**بارگذاری فایل View**

```php
require BASE_DIR . '/app/Views/' . $viewFile;
```

در نهایت با استفاده از `require` فایل View مورد نظر را بارگذاری می‌کنیم.

ثابت `BASE_DIR` مسیر اصلی پروژه را مشخص می‌کند.

## 3. ساختار دایرکتوری Views

برای نگهداری فایل‌های View، یک دایرکتوری به نام `Views` در مسیر `app` ایجاد می‌کنیم.

ساختار پیشنهادی:

```text
app/
└── Views/
    └── home/
        ├── index.php
        └── hello.php
```

برای مثال، محتوای فایل `index.php`:

```php
<h1>صفحه اصلی سایت</h1>
```

و محتوای فایل `hello.php`:

```php
<h1>Hello <?= $name ?></h1>
```

همان‌طور که مشاهده می‌کنید، متغیر `$name` مستقیماً در فایل View قابل استفاده است؛ زیرا با استفاده از تابع `extract` اطلاعات ارسالی از کنترلر به متغیرهای مستقل تبدیل شده‌اند.

## 4. استفاده از View در کنترلرها

اکنون می‌توانیم کلاس View را در کنترلرهای برنامه استفاده کنیم.

برای مثال، در کنترلر `HomeController`:

مسیر فایل:

```text
app/Controllers/HomeController.php
```

محتوای کلاس:

```php
<?php

namespace App\Controllers;

class HomeController extends Controller
{
    public function index(): void
    {
        $this->view('home.index');
    }

    public function sayHello(string $name): void
    {
        $this->view('home.hello', ['name' => $name]);
    }
}
```

در اینجا از متد `view` استفاده کرده‌ایم که در کلاس والد کنترلر پیاده‌سازی خواهد شد.

برای نمایش صفحه اصلی:

```php
$this->view('home.index');
```

و برای نمایش صفحه خوشامدگویی همراه با ارسال اطلاعات:

```php
$this->view('home.hello', ['name' => $name]);
```

در مثال دوم، مقدار متغیر `$name` به فایل View ارسال می‌شود.

## 5. پیاده‌سازی متد View در کلاس والد Controller

اگر قرار باشد در تمام کنترلرها از کلاس View استفاده کنیم، بهتر است به جای ساختن یک نمونه از این کلاس در هر کنترلر، آن را در کلاس والد `Controller` ایجاد کنیم.

به این ترتیب تمام کنترلرهایی که از کلاس `Controller` ارث‌بری می‌کنند، می‌توانند از متد `view` استفاده کنند.

محتوای کلاس:

مسیر فایل:

```text
app/Controllers/Controller.php
```

```php
<?php

namespace App\Controllers;

use Core\Libraries\View;

class Controller
{
    private View $view;

    public function __construct()
    {
        $this->view = new View();
    }

    protected function view(string $template, array $data = []): void
    {
        $this->view->render($template, $data);
    }
}
```

### بررسی کد کلاس Controller

**تعریف ویژگی View**

```php
private View $view;
```

یک ویژگی از نوع `View` تعریف کرده‌ایم تا نمونه کلاس View را در آن نگهداری کنیم.

**ساخت نمونه کلاس View در سازنده**

```php
public function __construct()
{
    $this->view = new View();
}
```

با ایجاد نمونه‌ای از هر کنترلر، سازنده کلاس والد اجرا شده و یک نمونه از کلاس View ساخته می‌شود.

**تعریف متد view**

```php
protected function view(string $template, array $data = []): void
{
    $this->view->render($template, $data);
}
```

این متد یک واسطه بین کنترلرها و کلاس View است.

در واقع، کنترلرها به جای اینکه مستقیماً با کلاس View کار کنند، متد `view` را فراخوانی می‌کنند.

از آنجایی که این متد با سطح دسترسی `protected` تعریف شده است، تمام کنترلرهای فرزند می‌توانند از آن استفاده کنند؛ اما خارج از کلاس و فرزندان آن قابل دسترسی نیست.

**نکته:** ویژگی `$view` را با سطح دسترسی `private` تعریف کرده‌ایم؛ زیرا فقط خود کلاس Controller مسئول مدیریت نمونه کلاس View است و کنترلرهای فرزند نیازی به دسترسی مستقیم به آن ندارند.

## 6. آشنایی با Layout در برنامه

تا این مرحله، توانستیم فایل‌های View را به صورت مستقل نمایش دهیم.

اما در یک وب‌سایت واقعی، معمولاً بخش‌هایی از صفحات در تمام صفحات مشترک هستند.

برای مثال:

- هدر سایت (Header)
- منوی اصلی (Navigation)
- فوتر سایت (Footer)
- فایل‌های CSS و JavaScript مشترک

اگر قرار باشد این بخش‌ها را در تمام فایل‌های View تکرار کنیم، با مشکل تکرار کد مواجه می‌شویم.

برای حل این مشکل از مفهومی به نام Layout استفاده می‌کنیم.

Layout در واقع قالب اصلی صفحات سایت است که بخش‌های مشترک صفحات را در خود نگهداری می‌کند.

در این پروژه، برای پیاده‌سازی ساده Layout، از دو فایل Header و Footer استفاده می‌کنیم.

## 7. ایجاد دایرکتوری Public و فایل‌های CSS و JavaScript

فایل‌هایی مانند CSS، JavaScript، تصاویر و فونت‌ها جزو فایل‌های عمومی سایت هستند و باید از طریق مرورگر قابل دسترسی باشند.

به همین دلیل، یک دایرکتوری به نام `public` در ریشه پروژه ایجاد می‌کنیم.

ساختار پیشنهادی:

```text
public/
└── assets/
    ├── css/
    │   └── main.css
    ├── js/
    │   └── main.js
    └── images/
```

در این مثال، فایل `main.css` را در مسیر زیر ایجاد می‌کنیم:

```text
public/assets/css/main.css
```

برای نمونه، محتوای این فایل می‌تواند به شکل زیر باشد:

```css
body {
    font-family: sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f5f5f5;
}

.container {
    width: 90%;
    margin: 0 auto;
}
```

از آنجایی که دایرکتوری `public` محل قرارگیری فایل‌های عمومی سایت است، مرورگر می‌تواند فایل‌های CSS و JavaScript را مستقیماً از این مسیر دریافت کند.

برای مثال، در فایل HTML می‌توانیم فایل CSS را به شکل زیر فراخوانی کنیم:

```html
<link rel="stylesheet" href="/assets/css/main.css">
```

**نکته:** فرض بر این است که دایرکتوری `public` به عنوان Document Root وب‌سرور تنظیم شده باشد؛ بنابراین آدرس `/assets/css/main.css` به فایل `public/assets/css/main.css` اشاره می‌کند.

## 8. ایجاد فایل‌های Header و Footer

اکنون باید فایل‌های مشترک صفحات را ایجاد کنیم.

برای این کار، دایرکتوری `layouts` را در مسیر `app/Views` ایجاد می‌کنیم.

ساختار جدید:

```text
app/
└── Views/
    ├── layouts/
    │   ├── header.php
    │   └── footer.php
    │
    └── home/
        ├── index.php
        └── hello.php
```

### فایل Header

در فایل `header.php` ساختار ابتدایی HTML، بخش Head و شروع Body را قرار می‌دهیم.

```php
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>پروژه MVC</title>

    <link rel="stylesheet" href="/assets/css/main.css">
</head>
<body>

<header>
    <h1>پروژه آموزشی MVC</h1>
</header>

<main class="container">
```

### فایل Footer

در فایل `footer.php` بخش پایانی صفحه و فایل‌های JavaScript را قرار می‌دهیم.

```php
</main>

<footer>
    <p>تمامی حقوق محفوظ است.</p>
</footer>

<script src="/assets/js/main.js"></script>

</body>
</html>
```

با این روش، ساختار HTML و فایل‌های مشترک فقط یک بار نوشته می‌شوند و در تمام صفحات قابل استفاده خواهند بود.

## 9. اضافه کردن Layout به کلاس View

در مرحله آخر، باید کلاس View را تغییر دهیم تا قبل و بعد از نمایش محتوای اصلی هر صفحه، فایل‌های Header و Footer را نیز بارگذاری کند.

نسخه جدید کلاس View:

```php
<?php

namespace Core\Libraries;

class View
{
    public function render(string $view, array $data = []): void
    {
        $viewFile = str_replace('.', '/', $view) . '.php';

        extract($data, EXTR_SKIP);

        require BASE_DIR . '/app/Views/layouts/header.php';

        require BASE_DIR . '/app/Views/' . $viewFile;

        require BASE_DIR . '/app/Views/layouts/footer.php';
    }
}
```

در این نسخه سه فایل به ترتیب بارگذاری می‌شوند:

1. فایل `header.php` برای نمایش بخش ابتدایی صفحه.
2. فایل View اصلی برای نمایش محتوای اختصاصی صفحه.
3. فایل `footer.php` برای نمایش بخش پایانی صفحه.

به این ترتیب، هر بار که متد `render` اجرا شود، ابتدا Header، سپس محتوای اصلی صفحه و در نهایت Footer نمایش داده می‌شود.

## 10. نتیجه نهایی

اکنون برای نمایش هر صفحه کافی است در کنترلر مربوطه متد `view` را فراخوانی کنیم.

برای مثال:

```php
public function index(): void
{
    $this->view('home.index');
}
```

یا:

```php
public function sayHello(string $name): void
{
    $this->view('home.hello', ['name' => $name]);
}
```

در هر دو حالت، برنامه به صورت خودکار عملیات زیر را انجام می‌دهد:

1. فایل Header را بارگذاری می‌کند.
2. فایل View مورد نظر را بارگذاری می‌کند.
3. اطلاعات ارسالی از کنترلر را در اختیار فایل View قرار می‌دهد.
4. فایل Footer را بارگذاری می‌کند.

بنابراین دیگر نیازی نیست در هر فایل View کدهای تکراری مربوط به ساختار HTML، فایل‌های CSS و JavaScript را بنویسیم.

### ساختار نهایی دایرکتوری‌های مرتبط با View

```text
project/
│
├── app/
│   ├── Controllers/
│   │   ├── Controller.php
│   │   └── HomeController.php
│   │
│   └── Views/
│       ├── layouts/
│       │   ├── header.php
│       │   └── footer.php
│       │
│       └── home/
│           ├── index.php
│           └── hello.php
│
├── core/
│   └── Libraries/
│       └── View.php
│
└── public/
    └── assets/
        ├── css/
        │   └── main.css
        ├── js/
        │   └── main.js
        └── images/
```

**جمع‌بندی:**

در این مرحله از توسعه مینی فریمورک MVC، توانستیم سیستم نمایش صفحات را پیاده‌سازی کنیم. با ایجاد کلاس View، نمایش فایل‌های مختلف را مدیریت کردیم و با قرار دادن نمونه این کلاس در کنترلر والد، امکان استفاده از آن را در تمام کنترلرها فراهم کردیم.

همچنین با پیاده‌سازی Layout، بخش‌های مشترک صفحات را از محتوای اختصاصی آن‌ها جدا کردیم تا ساختار پروژه منظم‌تر شده و از تکرار کد جلوگیری شود.

---
[صفحه قبلی: routing  ➡️](23-routing.md)
|
[فهرست مطالب](README.md)
|
<!-- [صفحه بعدی: view  ➡️](24-view.md) -->