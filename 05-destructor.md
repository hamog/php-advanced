## متد destructor یا تخریب گر
<p>تابع `__destruct()` در PHP یک متد ویژه درون یک کلاس است که هنگام از بین رفتن شیء یا پایان اجرای اسکریپت، به‌طور خودکار فراخوانی می‌شود.</p>
<p>تابع `__destruct()` در PHP، نقطه مقابل تابع `__construct()` در PHP است.</p>
<p>اگر تابع `__destruct()` را ایجاد کنید، PHP به‌طور خودکار این تابع را در پایان اسکریپت فراخوانی خواهد کرد.</p>
<p>Notice that the __destruct() function starts with two underscores (__)!</p>

```php
class Fruit {
  public $name;
  public $color;

  function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  function __destruct() {
    echo "Name: " . $this->name . ". Color: " . $this->color .".<br>";
  }
}

$apple = new Fruit('Apple', 'Red');
$banana = new Fruit('Banana', 'Yellow');
```
