### متد سازنده (constructor)
<p>
تابع `__construct()` در PHP یک متد ویژه درون یک کلاس است که هر بار با ایجاد یک شیء جدید از آن کلاس (با استفاده از کلمه کلیدی `new`)، به‌طور خودکار فراخوانی می‌شود.</p>
<p>تابع `__construct()` آرگومان‌هایی را می‌پذیرد که هنگام ایجاد شیء به آن ارسال می‌شوند (برای مثال: `new Fruit("Apple", "Red"); = $apple`). این ویژگی امکان مقداردهی اولیه پویا را فراهم می‌کند (و باعث کاهش حجم کد می‌شود).</p>
<p>توجه داشته باشید که تابع `__construct()` با دو علامت زیرخط (__) شروع می‌شود!</p>

```php
class Fruit {
  public $name;
  public $color;

  function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  function getDetails() {
    echo "Name: " . $this->name . ". Color: " . $this->color .".<br>";
  }
}

$apple = new Fruit('Apple', 'Red');
$apple->getDetails();

$banana = new Fruit('Banana', 'Yellow');
$banana->getDetails();
```

---
[صفحه قبلی: تعریف getter, setter ➡️](03-getter-setter.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: متد تخریب کننده ➡️](05-destructor.md)
