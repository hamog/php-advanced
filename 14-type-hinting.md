# استفاده از Type Hinting در کلاس ها
<p>در PHP، از Type Hinting برای مشخص کردن نوع داده‌ای که یک متغیر، پارامتر، ویژگی یا مقدار بازگشتی باید داشته باشد استفاده می‌کنیم.</p>

<p>فرض کن متدی داریم که سن یک کاربر را دریافت می‌کند. انتظار داریم سن به‌صورت عدد صحیح باشد، نه یک آرایه یا یک شیء.</p>
<p>بدون Type Hinting:</p>

```php
class User
{
    public function setAge($age)
    {
        echo $age;
    }
}

$user = new User();

$user->setAge(25);
$user->setAge("Ali");
$user->setAge([10, 20]);
```
<p>با استفاده از با Type Hinting</p>

```php
class User
{
    public function setAge(int $age)
    {
        echo $age;
    }
}

$user = new User();

$user->setAge(25);

$user->setAge([10, 20]); //Type Error 
```
<p>نکته: در حالت پیش‌فرض و بدون declare(strict_types=1)، PHP ممکن است بعضی انواع سازگار، مانند رشته عددی "25"، را به عدد تبدیل کند. بنابراین Type Hinting همیشه به معنای رد کردن هر نوع متفاوتی نیست.</p>

### تعیین نوع مقدار بازگشتی متد (Return Type)

```php
class Calculator
{
    public function sum(int $a, int $b): int
    {
        return $a + $b;
    }
}

$calculator = new Calculator();

echo $calculator->sum(10, 20);
```

### Type Hinting برای ویژگی‌های کلاس (Properties)

```php
class User
{
    public string $name;
    public int $age;
}

$user = new User();

$user->name = "Ali";
$user->age = 25;

echo $user->name;
echo $user->age;
```


---
[صفحه قبلی: namespace   ➡️](13-namespace.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: تزریق وابستگی ➡️](15-dependency-injection.md)