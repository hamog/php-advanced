# آشنایی با تزریق وابستگی (Dependency Injection)
<p>Dependency یعنی یک کلاس برای انجام وظیفه خودش به کلاس یا سرویس دیگری نیاز داشته باشد.</p>

```php
class SMS {
    public function send() {
        //
    }
}

class Customer {
    public function login() {
        // ...........

        //Send Authentication Code
        $sms = new SMS;
        $sms->send();

        //..................
    }
}
```
<p>روش بالا برای پروژه های کوچک قابل قبول است ولی در پروژه های بزرگ مشکلی به نام Tight Coupling ایجاد می شود.</p>
<p>Tight Coupling یا وابستگی شدید زمانی اتفاق می‌افتد که یک کلاس مستقیماً به پیاده‌سازی مشخص یک کلاس دیگر وابسته باشد.</p>
<p>اگر بخواهیم کلاس SMS را به Email تغییر دهیم مستقیما باید کلاس Customer را تغییر دهیم.</p>
<hr>

### Dependency Injection چیست؟
<p>Dependency Injection یا DI روشی برای مدیریت وابستگی بین کلاس‌ها است که در آن وابستگی مورد نیاز یک کلاس، به جای اینکه داخل آن ساخته شود، از بیرون به آن داده می‌شود.</p>

```php
class Customer
{
    private $sms;
    
    public function __construct($sms)
    {
        $this->sms = $sms;
    }

    public function login()
    {
        //login user
        $this->sms->send();
        //
    }
}
```



---
[صفحه قبلی: تعیین کننده نوع   ➡️](14-type-hinting.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: traits ➡️](16-traits.md)