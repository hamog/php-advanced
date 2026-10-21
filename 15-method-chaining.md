# استفاده از متد های زنجیره ای (method chaining) در کلاس ها
<p>«زنجیره‌سازی متدها» یکی از الگوهای محبوب در برنامه‌نویسی شیءگرا (OOP) است که به شما اجازه می‌دهد چندین متد را پشت سر هم و روی یک شیء واحد، تنها با استفاده از یک عملگرشی (->) فراخوانی کنید.</p>
<p>به جای اینکه هر متد را در یک خط جداگانه بنویسید و نام شیء را هر بار تکرار کنید، همه آن‌ها را در یک خط به هم متصل می‌کنید.</p>

### چرا از method chaining استفاده می‌کنیم؟
<ul>
    <li>۱. خوانایی بالا: کد شبیه به زبان طبیعی و جمله‌وار خوانده می‌شود.</li>
    <li>۲. کاهش کدهای تکراری (Clean Code): نیازی به تکرار نام شیء در هر خط نیست.</li>
    <li>۳. سرعت بالا در توسعه: کدهای مرتب‌تر و کوتاه‌تری خواهید داشت (کتابخانه‌های بزرگی مثل Laravel یا Query Builderها بسیار از این الگو استفاده می‌کنند).</li>
</ul>

### قانون اصلی برای پیاده‌سازی method chaining:
<p>برای اینکه یک متد قابل chain شدن باشد، باید در انتهای اجرای خود، خودِ شیء ($this) را برگرداند (return $this;).</p>

```php
class Calculator {
    private float $result = 0;

    public function add(float $number): self {
        $this->result += $number;
        return $this; 
    }

    public function subtract(float $number): self {
        $this->result -= $number;
        return $this;
    }

    public function multiply(float $number): self {
        $this->result *= $number;
        return $this;
    }

    public function getResult(): float {
        return $this->result;
    }
}

// using method chaining
$calc = new Calculator();

$finalResult = $calc->add(10)     // output: 10
                    ->subtract(2)  // output: 8
                    ->multiply(4)  // output: 32
                    ->getResult(); // get final output: 32

echo $finalResult;
```
#### توضیحات کد بالا:
<ul>
    <li>کلمه کلیدی self: در PHP (نسخه ۸ به بعد)، نوع خروجی متدها را می‌توان self گذاشت که یعنی این متد نمونه‌ای از همین کلاس را برمی‌گرداند.</li>
    <li>return $this;: این قلب تپنده‌ی متد چینینگ است. $this به شیء جاری اشاره می‌کند. وقتی یک متد اجرا می‌شود و return $this; می‌دهد، یعنی «من کارم تمام شد، حالا با همین شیء برو سراغ متد بعدی»</li>
    <li>ترتیب اجرا: متدها دقیقاً از چپ به راست (یا به ترتیب نوشته شدن از بالا به پایین در خطوط مختلف) اجرا می‌شوند.</li>
</ul>

---
[صفحه قبلی: تعیین گر نوع   ➡️](14-type-hinting.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: تزریق وابستگی ➡️](16-dependency-injection.md)