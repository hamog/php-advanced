# آشنایی با Composer

## فهرست مطالب

1. [آشنایی با Composer](#sec-1)
2. [نصب و بررسی Composer](#sec-2)
3. [آشنایی با فایل `composer.json`](#sec-3)
4. [مفهوم Autoload در PHP](#sec-4)
5. [آشنایی با Namespace](#sec-5)
6. [آشنایی با PSR-4](#sec-6)
7. [ساختار پروژه MVC](#sec-7)
8. [تنظیم Autoload در پروژه MVC](#sec-8)
9. [پیاده‌سازی پروژه MVC](#sec-9)
10. [اجرای پروژه](#sec-10)
11. [نکات مهم و خطاهای رایج](#sec-11)
12. [آشنایی با `composer install` و `composer update`](#sec-12)
13. [جمع‌بندی](#sec-13)
14. [تمرین دانشجویی](#sec-14)

---

<a id="sec-1"></a>

## ۱. آشنایی با Composer

Composer ابزاری برای مدیریت وابستگی‌ها (Dependency Management) در PHP است.

فرض کنید در پروژه خود به یک کتابخانه برای کار با تاریخ، ارسال ایمیل یا تولید فایل PDF نیاز داریم. به‌جای اینکه تمام کدهای آن کتابخانه را دستی دانلود و مدیریت کنیم، می‌توانیم از Composer استفاده کنیم.

Composer امکانات زیر را فراهم می‌کند:

- نصب و مدیریت کتابخانه‌های PHP
- مدیریت نسخه‌های موردنیاز کتابخانه‌ها
- ایجاد و مدیریت فایل‌های مربوط به Autoload
- ساده‌تر کردن استفاده از کلاس‌ها در پروژه‌های بزرگ
- کمک به حفظ ساختار استاندارد پروژه

یکی از قابلیت‌های مهم Composer، Autoload کردن کلاس‌ها است؛ یعنی PHP بتواند کلاس‌های موردنیاز را بدون نوشتن `require` یا `include` برای تک‌تک فایل‌ها بارگذاری کند.

---

<a id="sec-2"></a>

## ۲. نصب و بررسی Composer

ابتدا باید Composer روی سیستم نصب شده باشد.

برای بررسی نصب آن، ترمینال را باز کنید و دستور زیر را اجرا کنید:

```bash
composer --version
```

اگر Composer نصب باشد، نسخه آن نمایش داده می‌شود.

برای نصب Composer می‌توانید از وب‌سایت رسمی آن استفاده کنید:

[https://getcomposer.org](https://getcomposer.org)

> **نکته:** در ادامه فرض می‌کنیم Composer نصب شده و دستورات را در ترمینال اجرا می‌کنیم.

---

<a id="sec-3"></a>

## ۳. آشنایی با فایل composer.json

فایل `composer.json` فایل اصلی تنظیمات Composer است.

این فایل مشخص می‌کند که پروژه به چه کتابخانه‌هایی نیاز دارد و چه تنظیماتی برای Autoload آن در نظر گرفته شده است.

برای ایجاد این فایل، ابتدا وارد پوشه پروژه شوید و دستور زیر را اجرا کنید:

```bash
composer init
```

Composer چند سؤال درباره پروژه می‌پرسد و در نهایت فایل `composer.json` را ایجاد می‌کند.

همچنین می‌توانیم این فایل را به‌صورت دستی بسازیم.

مثال:

```json
{
    "name": "student/mvc-project",
    "description": "A simple PHP MVC project",
    "autoload": {
        "psr-4": {
            "App\\": "app/"
        }
    }
}
```

در این فایل، بخش `autoload` اهمیت زیادی دارد.

```json
"autoload": {
    "psr-4": {
        "App\\": "app/"
    }
}
```

این تنظیم به Composer می‌گوید کلاس‌هایی که Namespace آن‌ها با `App\` شروع می‌شود، باید در پوشه `app/` جست‌وجو شوند.

برای مثال، اگر کلاسی با Namespace زیر داشته باشیم:

```php
namespace App\Models;
```

Composer انتظار دارد فایل این کلاس در پوشه زیر قرار گرفته باشد:

```text
app/Models/
```

اما صرفاً نوشتن تنظیمات در `composer.json` کافی نیست. پس از تغییر تنظیمات Autoload باید دستور زیر را اجرا کنیم:

```bash
composer dump-autoload
```

این دستور فایل‌های موردنیاز برای Autoload را تولید یا به‌روزرسانی می‌کند.

---

<a id="sec-4"></a>

## ۴. مفهوم Autoload در PHP

فرض کنید دو کلاس در دو فایل جداگانه داریم.

فایل `User.php`:

```php
<?php

class User
{
    public function getName(): string
    {
        return "Ali";
    }
}
```

فایل `index.php`:

```php
<?php

require_once 'User.php';

$user = new User();

echo $user->getName();
```

در این مثال برای استفاده از کلاس `User`، فایل آن را با `require_once` بارگذاری کرده‌ایم.

این روش برای پروژه‌های کوچک مناسب است؛ اما اگر پروژه ده‌ها یا صدها کلاس داشته باشد، نوشتن تعداد زیادی `require_once` کار دشواری خواهد بود.

برای حل این مشکل از Autoload استفاده می‌کنیم.

Autoload به PHP امکان می‌دهد هنگام استفاده از کلاسی که هنوز تعریف نشده است، فایل مربوط به آن کلاس را به‌صورت خودکار بارگذاری کند.

Composer این کار را برای ما انجام می‌دهد.

پس از تنظیم Autoload و اجرای دستور زیر:

```bash
composer dump-autoload
```

فایلی به نام زیر ساخته می‌شود:

```text
vendor/autoload.php
```

برای فعال کردن Autoload در پروژه کافی است این فایل را یک بار بارگذاری کنیم:

```php
require_once __DIR__ . '/vendor/autoload.php';
```

از این پس، کلاس‌هایی که مطابق تنظیمات Autoload تعریف شده‌اند، بدون نیاز به `require` جداگانه قابل استفاده خواهند بود.

---

<a id="sec-5"></a>

## ۵. آشنایی با Namespace

قبل از کار با Autoload بهتر است با مفهوم Namespace آشنا شویم.

Namespace یا فضای نام، روشی برای دسته‌بندی کلاس‌ها و جلوگیری از تداخل نام آن‌هاست.

فرض کنید دو کلاس به نام `User` داریم:

- یکی برای مدیریت کاربران
- دیگری برای مدیریت کاربران پنل مدیریت

اگر هر دو کلاس بدون Namespace تعریف شوند، نمی‌توانیم آن‌ها را به‌صورت معمول با یک نام در یک پروژه استفاده کنیم.

برای حل این مشکل، از Namespace استفاده می‌کنیم.

مثال اول؛ فایل `app/Models/User.php`:

```php
<?php

namespace App\Models;

class User
{
    public function getName(): string
    {
        return "Ali";
    }
}
```

مثال دوم؛ فایل `app/Controllers/User.php`:

```php
<?php

namespace App\Controllers;

class User
{
    public function index(): string
    {
        return "User Controller";
    }
}
```

هر دو کلاس نام `User` دارند؛ اما Namespace آن‌ها متفاوت است.

نام کامل این کلاس‌ها عبارت است از:

```text
App\Models\User
App\Controllers\User
```

بنابراین می‌توانیم هر دو کلاس را در یک پروژه داشته باشیم.

### استفاده از کلاس دارای Namespace

برای استفاده از کلاس `User` در Namespace مدل‌ها، می‌توانیم بنویسیم:

```php
<?php

$user = new \App\Models\User();

echo $user->getName();
```

علامت `\` در ابتدای نام کامل کلاس نشان می‌دهد که نام از فضای نام اصلی PHP آغاز می‌شود.

روش دیگر استفاده از `use` است:

```php
<?php

use App\Models\User;

$user = new User();

echo $user->getName();
```

استفاده از `use` باعث می‌شود مجبور نباشیم هر بار نام کامل کلاس را بنویسیم.

> **نکته مهم:** دستور `use` به‌تنهایی فایل کلاس را بارگذاری نمی‌کند؛ فقط نام کلاس را در کد کوتاه‌تر و قابل‌استفاده می‌کند. برای بارگذاری خودکار فایل، به Autoload نیاز داریم.

---

<a id="sec-6"></a>

## ۶. آشنایی با PSR-4

PSR-4 یک استاندارد در PHP برای نگاشت Namespace کلاس‌ها به مسیر فایل‌های آن‌هاست.

Composer از این استاندارد پشتیبانی می‌کند و با استفاده از آن می‌تواند مسیر فایل هر کلاس را تشخیص دهد.

فرض کنید در فایل `composer.json` تنظیم زیر را داریم:

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "app/"
        }
    }
}
```

این تنظیم یعنی Namespace پایه `App\` به پوشه `app/` متصل شده است.

حالا اگر کلاسی با مشخصات زیر داشته باشیم:

```php
namespace App\Models;

class Product
{
}
```

Composer مسیر کلاس را به شکل زیر پیدا می‌کند:

```text
App\Models\Product
        ↓
app/Models/Product.php
```

یا برای کلاس زیر:

```php
namespace App\Controllers;

class ProductController
{
}
```

مسیر فایل به این صورت خواهد بود:

```text
App\Controllers\ProductController
        ↓
app/Controllers/ProductController.php
```

در این ساختار:

- `App\` بخش اصلی Namespace است.
- `Models` به پوشه `Models` اشاره می‌کند.
- `Product` نام کلاس و نام فایل آن است.

> **نکته:** در استاندارد PSR-4، نام پوشه‌ها و فایل‌ها باید با بخش‌های متناظر Namespace و نام کلاس مطابقت داشته باشند. در سیستم‌عامل‌های حساس به حروف بزرگ و کوچک، این موضوع اهمیت ویژه‌ای دارد.

---

<a id="sec-7"></a>

## ۷. ساختار پروژه MVC

حالا می‌خواهیم Composer و Autoload را در یک پروژه ساده MVC به کار ببریم.

در این مثال، فهرستی از محصولات را نمایش می‌دهیم.

ساختار پروژه به شکل زیر خواهد بود:

```text
mvc-project/
│
├── app/
│   ├── Models/
│   │   └── Product.php
│   │
│   ├── Controllers/
│   │   └── ProductController.php
│   │
│   └── Views/
│       └── products.php
│
├── vendor/
│
├── composer.json
├── index.php
└── .gitignore
```

نقش پوشه‌ها:

| مسیر               | کاربرد                                           |
| ------------------ | ------------------------------------------------ |
| `app/Models/`      | تعریف مدل‌ها و منطق مربوط به داده‌ها             |
| `app/Controllers/` | مدیریت درخواست‌ها و ارتباط مدل با View           |
| `app/Views/`       | نمایش خروجی به کاربر                             |
| `vendor/`          | فایل‌های مربوط به وابستگی‌ها و Autoload Composer |
| `composer.json`    | تنظیمات Composer و وابستگی‌های پروژه             |
| `index.php`        | نقطه شروع اجرای برنامه                           |

پوشه `vendor/` و فایل‌های Autoload معمولاً به‌صورت خودکار توسط Composer ایجاد می‌شوند.

---

<a id="sec-8"></a>

## ۸. تنظیم Autoload در پروژه MVC

ابتدا فایل `composer.json` را در ریشه پروژه ایجاد می‌کنیم:

```json
{
    "name": "student/mvc-project",
    "description": "Simple MVC project using Composer",
    "autoload": {
        "psr-4": {
            "App\\": "app/"
        }
    }
}
```

در اینجا Namespace پایه `App\` به پوشه `app/` متصل شده است.

حالا در ترمینال، از داخل پوشه اصلی پروژه دستور زیر را اجرا می‌کنیم:

```bash
composer dump-autoload
```

Composer پوشه `vendor/` و فایل `vendor/autoload.php` را ایجاد یا به‌روزرسانی می‌کند.

از این پس می‌توانیم کلاس‌های موجود در پوشه `app/` را با رعایت استاندارد PSR-4 به‌صورت خودکار بارگذاری کنیم.

---

<a id="sec-9"></a>

## ۹. پیاده‌سازی پروژه MVC

اکنون بخش‌های مختلف پروژه را ایجاد می‌کنیم.

### مرحله اول: ساخت Model

فایل `app/Models/Product.php`:

```php
<?php

namespace App\Models;

class Product
{
    public function getAll(): array
    {
        return [
            [
                'id' => 1,
                'name' => 'Laptop',
                'price' => 500
            ],
            [
                'id' => 2,
                'name' => 'Mobile',
                'price' => 300
            ],
            [
                'id' => 3,
                'name' => 'Keyboard',
                'price' => 50
            ]
        ];
    }
}
```

در این کلاس، متد `getAll()` یک آرایه از محصولات را برمی‌گرداند.

در یک پروژه واقعی، این اطلاعات می‌توانند از پایگاه داده دریافت شوند؛ اما برای ساده نگه داشتن مثال از یک آرایه استفاده کرده‌ایم.

به Namespace ابتدای فایل توجه کنید:

```php
namespace App\Models;
```

به همین دلیل، نام کامل این کلاس `App\Models\Product` خواهد بود.

### مرحله دوم: ساخت Controller

فایل `app/Controllers/ProductController.php`:

```php
<?php

namespace App\Controllers;

use App\Models\Product;

class ProductController
{
    public function index(): void
    {
        $productModel = new Product();

        $products = $productModel->getAll();

        require __DIR__ . '/../Views/products.php';
    }
}
```

در این کلاس، از Model استفاده می‌کنیم تا اطلاعات محصولات را دریافت کنیم.

این خط را بررسی کنید:

```php
use App\Models\Product;
```

با این دستور، نام کلاس `Product` را از Namespace مربوط به مدل‌ها وارد کرده‌ایم.

سپس می‌توانیم بدون نوشتن نام کامل کلاس، شیء آن را بسازیم:

```php
$productModel = new Product();
```

از آنجا که Autoload مربوط به Composer را فعال خواهیم کرد، نیازی نیست فایل `Product.php` را با `require_once` بارگذاری کنیم.

همچنین این خط:

```php
require __DIR__ . '/../Views/products.php';
```

فایل View را بارگذاری می‌کند. این کار در این مثال برای نمایش خروجی انجام می‌شود؛ Autoload مربوط به Composer جایگزین بارگذاری فایل‌های View نیست.

### مرحله سوم: ساخت View

فایل `app/Views/products.php`:

```php
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>فهرست محصولات</title>
</head>
<body>

<h1>فهرست محصولات</h1>

<table border="1" cellpadding="10">
    <thead>
        <tr>
            <th>شناسه</th>
            <th>نام محصول</th>
            <th>قیمت</th>
        </tr>
    </thead>

    <tbody>
        <?php foreach ($products as $product): ?>
            <tr>
                <td>
                    <?= htmlspecialchars((string) $product['id']) ?>
                </td>

                <td>
                    <?= htmlspecialchars($product['name']) ?>
                </td>

                <td>
                    <?= htmlspecialchars((string) $product['price']) ?>
                </td>
            </tr>
        <?php endforeach; ?>
    </tbody>
</table>

</body>
</html>
```

در این فایل، آرایه `$products` که توسط Controller ایجاد شده است، دریافت و نمایش داده می‌شود.

از `htmlspecialchars()` برای جلوگیری از تفسیر ناخواسته کاراکترهای HTML استفاده کرده‌ایم.

### مرحله چهارم: ساخت فایل اصلی پروژه

فایل `index.php` در ریشه پروژه:

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

use App\Controllers\ProductController;

$controller = new ProductController();

$controller->index();
```

این فایل نقطه شروع برنامه است.

ابتدا Autoload مربوط به Composer را فعال می‌کنیم:

```php
require_once __DIR__ . '/vendor/autoload.php';
```

سپس Controller را استفاده می‌کنیم:

```php
use App\Controllers\ProductController;
```

و در نهایت، متد `index()` را اجرا می‌کنیم:

```php
$controller = new ProductController();

$controller->index();
```

با این کار، Controller مدل را فراخوانی می‌کند، اطلاعات محصولات را می‌گیرد و View را نمایش می‌دهد.

> **توجه:** در PHP، دستورهای `namespace` و `use` باید در جایگاه مجاز خود، پیش از کد اجرایی عادی فایل قرار بگیرند. بنابراین اگر بخواهید کد `index.php` را دقیقاً مطابق قواعد PHP تنظیم کنید، بهتر است آن را به این شکل بنویسید:

```php
<?php

use App\Controllers\ProductController;

require_once __DIR__ . '/vendor/autoload.php';

$controller = new ProductController();

$controller->index();
```

در این مثال، `use` پیش از دستور اجرایی قرار گرفته است و پس از آن می‌توانیم Autoload را فعال و برنامه را اجرا کنیم.

---

<a id="sec-10"></a>

## ۱۰. اجرای پروژه

پس از ایجاد فایل‌ها و پوشه‌های بالا، ساختار پروژه باید مشابه این باشد:

```text
mvc-project/
├── app/
│   ├── Models/
│   │   └── Product.php
│   ├── Controllers/
│   │   └── ProductController.php
│   └── Views/
│       └── products.php
├── vendor/
│   └── autoload.php
├── composer.json
└── index.php
```

ابتدا از داخل پوشه اصلی پروژه، دستور زیر را اجرا کنید:

```bash
composer dump-autoload
```

سپس سرور داخلی PHP را اجرا کنید:

```bash
php -S localhost:8000
```

حالا در مرورگر آدرس زیر را باز کنید:

```text
http://localhost:8000
```

باید جدول محصولات را مشاهده کنید.

---

<a id="sec-11"></a>

## ۱۱. نکات مهم و خطاهای رایج

### خطای اول: فایل autoload.php پیدا نمی‌شود

اگر خطایی مشابه زیر مشاهده کردید:

```text
Failed opening required 'vendor/autoload.php'
```

بررسی کنید:

- آیا دستور Composer را در پوشه اصلی پروژه اجرا کرده‌اید؟
- آیا پوشه `vendor/` ایجاد شده است؟
- آیا مسیر `vendor/autoload.php` صحیح است؟

اگر فایل وجود ندارد، از پوشه اصلی پروژه اجرا کنید:

```bash
composer dump-autoload
```

### خطای دوم: کلاس پیدا نمی‌شود

اگر خطایی مشابه زیر دریافت کردید:

```text
Class "App\Models\Product" not found
```

موارد زیر را بررسی کنید:

1. آیا Namespace کلاس صحیح است؟
2. آیا مسیر فایل با Namespace مطابقت دارد؟
3. آیا نام فایل و کلاس صحیح نوشته شده است؟
4. آیا تنظیم PSR-4 در `composer.json` صحیح است؟
5. آیا پس از تغییر تنظیمات، دستور `composer dump-autoload` را اجرا کرده‌اید؟

برای مثال، اگر کلاس به این صورت تعریف شده باشد:

```php
namespace App\Models;

class Product
{
}
```

مسیر آن باید مطابق تنظیم PSR-4، به شکل زیر باشد:

```text
app/Models/Product.php
```

### خطای سوم: تصور اینکه `use` فایل را بارگذاری می‌کند

دستور زیر:

```php
use App\Models\Product;
```

به‌تنهایی فایل کلاس را بارگذاری نمی‌کند.

این دستور فقط به PHP می‌گوید که هنگام استفاده از نام `Product`، منظور کلاس `App\Models\Product` است.

برای بارگذاری خودکار کلاس، باید Autoload را فعال کرده باشیم:

```php
require_once __DIR__ . '/vendor/autoload.php';
```

---

<a id="sec-12"></a>

## ۱۲. آشنایی با composer install و composer update

دو دستور مهم Composer عبارت‌اند از:

```bash
composer install
```

و:

```bash
composer update
```

این دو دستور کاربرد یکسانی ندارند.

| دستور                    | کاربرد                                                       |
| ------------------------ | ------------------------------------------------------------ |
| `composer install`       | نصب وابستگی‌ها مطابق فایل `composer.lock`، در صورت وجود      |
| `composer update`        | بررسی نسخه‌های مجاز جدید و به‌روزرسانی وابستگی‌ها و فایل قفل |
| `composer dump-autoload` | تولید یا به‌روزرسانی فایل‌های Autoload                       |

در یک پروژه تیمی، معمولاً فایل‌های `composer.json` و `composer.lock` را در Git نگه می‌داریم.

پوشه `vendor/` معمولاً در Git قرار نمی‌گیرد؛ زیرا هر توسعه‌دهنده می‌تواند وابستگی‌ها را با Composer نصب کند.

برای این کار، فایل `.gitignore` را به شکل زیر تنظیم می‌کنیم:

```text
/vendor/
```

هنگام دریافت پروژه از Git، کافی است در پوشه اصلی پروژه اجرا کنیم:

```bash
composer install
```

Composer وابستگی‌های پروژه را نصب می‌کند و فایل‌های Autoload را نیز در اختیار پروژه قرار می‌دهد.

---

<a id="sec-13"></a>

## ۱۳. جمع‌بندی

در این جزوه با مفاهیم زیر آشنا شدیم:

- Composer ابزاری برای مدیریت وابستگی‌های PHP است.
- فایل `composer.json` تنظیمات پروژه و Autoload را مشخص می‌کند.
- Namespace برای دسته‌بندی کلاس‌ها و جلوگیری از تداخل نام‌ها استفاده می‌شود.
- استاندارد PSR-4 ارتباط بین Namespace و مسیر فایل‌ها را مشخص می‌کند.
- فایل `vendor/autoload.php` امکان بارگذاری خودکار کلاس‌ها را فراهم می‌کند.
- در ساختار MVC، Controller می‌تواند از Model استفاده کند، بدون اینکه لازم باشد فایل Model را به‌صورت دستی بارگذاری کنیم.
- فایل‌های View در این مثال با `require` بارگذاری می‌شوند؛ Composer به‌صورت پیش‌فرض مسئول بارگذاری آن‌ها نیست.

---

<a id="sec-14"></a>

## ۱۴. تمرین دانشجویی

یک پروژه ساده MVC برای مدیریت کتاب‌ها ایجاد کنید.

ساختار پروژه باید شامل این کلاس‌ها و فایل‌ها باشد:

1. کلاس `Book` در پوشه `app/Models/`
2. کلاس `BookController` در پوشه `app/Controllers/`
3. فایل `books.php` در پوشه `app/Views/`
4. فایل `index.php` در ریشه پروژه
5. فایل `composer.json` با تنظیم PSR-4 برای Namespace پایه `App\`

خواسته‌های تمرین:

- در کلاس `Book` متدی به نام `getAll()` تعریف کنید که چند کتاب را برگرداند.
- در کلاس `BookController`، مدل `Book` را فراخوانی کنید.
- اطلاعات کتاب‌ها را در View نمایش دهید.
- کلاس‌ها را با Composer Autoload کنید.
- در فایل‌های Controller و Model برای هر کلاس Namespace مناسب تعریف کنید.
- مطمئن شوید برای استفاده از کلاس‌ها نیازی به نوشتن `require` جداگانه برای فایل‌های Model و Controller ندارید.

هدف تمرین این است که دانشجو بتواند ارتباط بین Composer، Namespace، PSR-4 و ساختار MVC را در یک پروژه عملی درک کند.

---
[صفحه قبلی: mvc    ➡️](18-mvc.md)
|
[فهرست مطالب](README.md)
|
<!-- [صفحه بعدی: composer ➡️](19-composer.md) -->