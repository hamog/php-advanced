# تعریف ثابت ها (Constants)

<p>ثابت‌های کلاس زمانی مفید هستند که بخواهید داده‌های ثابتی را درون یک کلاس تعریف کنید.</p>
<p>ثابت کلاس با استفاده از کلمه کلیدی `const` درون کلاس تعریف می‌شود.</p>
<p>سطح دسترسی پیش‌فرض برای ثابت‌های کلاس، `public` (عمومی) است.</p>
<p>ثابت‌های کلاس به کوچک یا بزرگ بودن حروف (Case-sensitivity) حساس هستند؛ با این حال، توصیه می‌شود نام آن‌ها با حروف تمام‌بزرگ نوشته شود.</p>

### دسترسی به ثابت کلاس از بیرون کلاس
```php
class Goodbye {
  const MESSAGE = "Thank you for visiting shetabamooz.com!";
}

echo Goodbye::MESSAGE; // Access constant
```

### دسترسی به ثابت کلاس از داخل کلاس

```php
class Goodbye {
  const MESSAGE = "Thank you for visiting shetabamooz.com!";

  public function bye() {
    echo self::MESSAGE; // Access constant
  }
}

$goodbye = new Goodbye();
$goodbye->bye();
```

---
[صفحه قبلی: وراثت   ➡️](07-inheritance.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: انتزاع ➡️](09-abastract.md)