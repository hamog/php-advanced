# Traits
<p>از «Trait»ها برای تعریف متدهایی استفاده می‌شود که می‌توانند در چندین کلاس به کار روند. Traitها می‌توانند شامل متدها و متدهای انتزاعی (abstract) باشند که در کلاس‌های متعدد قابل استفاده‌اند و این متدها می‌توانند هر نوع سطح دسترسی (public، private یا protected) را داشته باشند.</p>
<p>«تریت»ها (Traits) به شما امکان می‌دهند چندین متد را آزادانه در کلاس‌های مختلف مجدداً استفاده کنید و مکانیزمی برای استفاده‌ی مجدد از کد محسوب می‌شوند.</p>
<p>یک «تریت» (trait) با استفاده از کلمه کلیدی `trait` و به دنبال آن، نامی برای آن تریت تعریف می‌شود.</p>
<p>برای استفاده از یک trait در یک کلاس، از کلمه کلیدی `use` و به دنبال آن نام trait استفاده کنید.</p>

```php
// Define a trait
trait message1 {
  public function msg1() {
    echo "PHP OOP is fun! ";
  }
}

// Use the trait in a class
class Welcome {
  use message1;
}

$obj = new Welcome();
$obj->msg1();
```

<p>نکته: اگر کلاس‌های دیگر نیاز به استفاده از تابع `msg1()` دارند، کافی است از trait مربوط به `message1` در آن کلاس‌ها استفاده کنید. این کار باعث کاهش تکرار کد می‌شود، زیرا دیگر نیازی به تعریف مجدد و مکرر همان متد نیست.</p>

```php
trait message1 {
  public function msg1() {
    echo "PHP OOP is fun! ";
  }
  public function msg2() {
    echo "Traits reduce code duplication! ";
  }
  public function msg3() {
    echo "Hello World!";
  }
}

class Welcome {
  use message1;
}

class Welcome2 {
  use message1;
}

$obj = new Welcome();
$obj->msg1();
echo "<br>";

$obj2 = new Welcome2();
$obj2->msg1();
$obj2->msg2();
$obj2->msg3();
```

### استفاده از چند trait در یک کلاس

```php
trait message1 {
  public function msg1() {
    echo "PHP OOP is fun! ";
  }
}

trait message2 {
  public function msg2() {
    echo "Traits reduce code duplication!";
  }
}

class Welcome {
  use message1;
}

class Welcome2 {
  use message1, message2;
}

$obj = new Welcome();
$obj->msg1();
echo "<br>";

$obj2 = new Welcome2();
$obj2->msg1();
$obj2->msg2();
```

---
[صفحه قبلی: رابط ها   ➡️](10-interface.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: static ➡️](12-static.md)