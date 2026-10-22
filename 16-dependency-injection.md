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
        //Send Authentication Code
        $this->sms->send();
        //
    }
}

$sms = new Sms;
$customer = new Customer($sms)
$customer->login();
```

### یک مثال بهتر با استفاده از interface

```php
interface Sender {
    public function send();
}

class Sms implements Sender {
    public function send() {
        echo 'Send Sms';
    }
}

class Email implements Sender {
    public function send() {
        echo 'Send Email';
    }
}

class Customer {
    private Sender $sender;

    public function __construct(Sender $sender) {
        $this->sender = $sender;
    }

    public function login() {
        //********** */

        $this->sender->send();

        //************ */
    }
}

$sms = new Sms;
$customer = new Customer($sms); 
$customer->login(); // output: Send Sms


echo '<hr />';

$email = new Email;
$customer = new Customer($email); 
$customer->login(); // output: Send Email
```


---
[صفحه قبلی: زنجیره متدها   ➡️](15-method-chaining.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: autoloading ➡️](17-autoload.md)