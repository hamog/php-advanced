# Namespaces


## Namespace چیست؟
<p>کلمه‌ی namespace در PHP به معنی فضای نام است. از آن استفاده می‌کنیم تا کلاس‌ها، توابع و ثابت‌ها را در گروه‌های جداگانه قرار دهیم و از تداخل نام‌های آن‌ها جلوگیری کنیم.</p>
<p>در PHP هم ممکن است دو کلاس با نام یکسان داشته باشیم. namespace کمک می‌کند این کلاس‌ها را از هم تشخیص دهیم.</p>

### نحوه تعریف namespace
<p>فضاهای نام (Namespaces) باید در ابتدای یک فایل PHP تعریف شوند؛ بدین صورت که از کلمه کلیدی `namespace` استفاده شده و پس از آن، نامی برای فضای نام می‌آید.</p>

```php
namespace App; 
```
<p>نکته: تعریف فضای نام (namespace) باید نخستین چیزی باشد که در فایل PHP می‌آید!</p>

### یک مثال از نحوه استفاده کلاس با namespace
```php
namespace App; 

class User {

}

//Using Namespaces
$user = new App\User;

```
<p>اگر دو کلاس همنام همزمان در یک برنامه استفاده شوند PHP نمی تواند تشخیص دهد و خطا می دهد.</p>

```php

namespace App;

class User
{
    public function getName()
    {
        return "App User";
    }
}
```

```php

namespace Admin;

class User
{
    public function getName()
    {
        return "Admin User";
    }
}

```

```php
require 'App/User.php';
require 'Admin/User.php';

$appUser = new App\User();
$adminUser = new Admin\User();

echo $appUser->getName();
echo "<br>";
echo $adminUser->getName();
```

### استفاده از دستور use
<p>اگر نام کلاس طولانی باشد، مجبوریم هر بار نام کامل آن را بنویسیم. PHP برای ساده‌تر شدن این کار دستور use را در اختیارمان قرار می‌دهد.</p>

```php
use App\Models\User;

//$appUser = new App\Models\User();
$appUser = new User();
```

### استفاده از alias در namespace 
<p>اختصاص دادن یک نام مستعار (Alias) به یک فضای نام (Namespace) یا کلاس می‌تواند نوشتن کد را آسان‌تر کند.</p>

```php
use App\User as AppUser;
use Admin\User as AdminUser;

// $appUser = new User();
// $adminUser = new User();

$appUser = new AppUser();
$adminUser = new AdminUser();
```

---
[صفحه قبلی: static   ➡️](12-static.md)
|
[فهرست مطالب](README.md)
|
[ُType Hinting: ➡️](14-type-hinting.md)