# آشنایی با autoloading
<p>بدون Autoload، باید فایل کلاس‌ها را معمولاً با require_once یا require بارگذاری کنیم.</p>

```php
// User.php
class User
{
    public function getName(): string
    {
        return "Ali";
    }
}

// Product.php
class Product
{
    public function getName(): string
    {
        return "Laptop";
    }
}

// index.php
require_once 'User.php';
require_once 'Product.php';

$user = new User();
$product = new Product();

echo $user->getName();
echo "<br>";
echo $product->getName();

```
<p>این کد درست کار می‌کند؛ اما تصور کنید پروژه به‌جای دو کلاس، ۱۰۰ یا ۵۰۰ کلاس داشته باشد. در این صورت باید تعداد زیادی فایل را به‌صورت دستی بارگذاری کنیم و مدیریت وابستگی‌ها دشوارتر می‌شود.</p>


## استفاده از autoload
<p>استفاده از Autoload که قابلیتی در PHP است که به ما اجازه می‌دهد هنگام نیاز به یک کلاس، فایل مربوط به آن به‌صورت خودکار بارگذاری شود؛ در نتیجه دیگر لازم نیست برای هر کلاس یک require_once بنویسیم.</p>

### روش اول: استفاده از spl_autoload_register
<p>فکر کنید ساختار پروژه به این صورت است:</p>
<p>نکته: نام فایل هر کلاس با نام خود کلاس یکسان است؛ مثلاً کلاس User در فایل User.php قرار دارد.</p>

```bash
project/
├── classes/
│   ├── User.php
│   └── Product.php
└── index.php
```

<p>حالا در فایل index.php می‌نویسیم:</p>

```php
spl_autoload_register(function ($className) {
    require_once __DIR__ . '/classes/' . $className . '.php';
});

$user = new User();
$product = new Product();

echo $user->getName();
echo "<br>";
echo $product->getName();
```
<ul>
    <li>spl_autoload_register() تابع بارگذاری خودکار ما را ثبت می‌کند.</li>
    <li>$className نام کلاسی است که PHP به دنبال آن می‌گردد.</li>
    <li>__DIR__ مسیر پوشه‌ای است که فایل فعلی، یعنی index.php، در آن قرار دارد.</li>
    <li>- require_once فایل کلاس موردنظر را بارگذاری می‌کند.</li>
    <li></li>
</ul>

#### اگر کلاس‌ها در پوشه‌های مختلف باشند چه می‌شود؟

```bash
project/
├── app/
│   ├── Models/
│   │   └── User.php
│   └── Services/
│       └── UserService.php
└── index.php
```

<p>راه حل استفاده از namespace است.</p>

```php
// User.php
namespace App\Models;

class User
{
    public function getName(): string
    {
        return "Ali";
    }
}

// Product.php
namespace App\Services;

use App\Models\User;

class UserService
{
    public function getUserName(User $user): string
    {
        return $user->getName();
    }
}

// index.php
spl_autoload_register(function ($className) {

    $prefix = 'App\\';
    $baseDir = __DIR__ . '/app/';

    // بررسی می‌کنیم نام کلاس با App\ شروع شود
    if (strncmp($prefix, $className, strlen($prefix)) !== 0) {
        return;
    }

    // حذف پیشوند App\
    $relativeClass = substr($className, strlen($prefix));

    // تبدیل جداکننده‌های namespace به جداکننده مسیر
    $file = $baseDir
        . str_replace('\\', DIRECTORY_SEPARATOR, $relativeClass)
        . '.php';

    // فقط در صورت وجود فایل، آن را بارگذاری می‌کنیم
    if (file_exists($file)) {
        require_once $file;
    }
});

use App\Models\User;
use App\Services\UserService;

$user = new User();

$service = new UserService();

echo $service->getUserName($user);
```

### روش دوم: روش استاندارد پروژه‌های PHP: استفاده از Composer
<p>در پروژه‌های واقعی PHP، یکی از رایج‌ترین روش‌ها برای Autoload کردن کلاس‌ها، استفاده از Composer است.</p>
<p>Composer علاوه بر مدیریت کتابخانه‌های پروژه، می‌تواند فایل کلاس‌های خودمان را نیز به‌صورت خودکار بارگذاری کند.</p>


---
[صفحه قبلی: تزریق وابستگی ها   ➡️](15-dependency-injection.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: mvc ➡️](18-mvc.md)
