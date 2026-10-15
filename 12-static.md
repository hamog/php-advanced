# تعریف متد ها و پراپرتی های static
<p>از کلمه کلیدی static برای ایجاد متدها و پراپرتی های استاتیک استفاده می‌شود.</p>
<p>به متدهای استاتیک می‌توان مستقیماً دسترسی پیدا کرد — بدون اینکه نیاز باشد ابتدا نمونه‌ای از کلاس ایجاد شود.</p>

```php
class Calculator {
  // static method
  public static function sum($x, $y) {
    return $x * $y;
  }
}

// Call static method
$result = Calculator::sum(3, 7);
echo $result;
```

<p>یک کلاس می‌تواند هم شامل متدهای استاتیک (static) و هم متدهای غیر استاتیک باشد. از طریق یک متد در همان کلاس، می‌توان با استفاده از کلمه کلیدی `self` و علامت دو نقطه (::) به یک متد استاتیک دسترسی پیدا کرد.</p>

```php
class Greeting {
  // static method
  public static function welcome() {
    echo "Hello World!";
  }

  // non-static method
  public function syHello() {
    self::welcome();
  }
}
```

<p>برای فراخوانی یک متد استاتیک از درون یک کلاس فرزند، از کلمه کلیدی `parent` در داخل آن کلاس استفاده کنید. در این حالت، متد استاتیک می‌تواند `public` یا `protected` باشد.</p>

```php
class Domain {
  protected static function getWebsiteName() {
    return "shetabamooz.com";
  }
}

class Website extends Domain {
  public $websiteName;
  public function __construct() {
    $this->websiteName = parent::getWebsiteName();
  }
}

$website = new Website;
echo $website->websiteName;
```

## پراپرتی های static
<p>به ویژگی‌های استاتیک می‌توان به‌طور مستقیم — و بدون نیاز به ایجاد نمونه‌ای از کلاس — دسترسی پیدا کرد.</p>

```php
class Circle {
  // static property
  public static $pi = 3.14159;
}

// Get static property directly
echo Circle::$pi;
```

<p>یک کلاس می‌تواند هم پراپرتی های ایستا (static) و هم پراپرتی های غیرایستا داشته باشد. در یک کلاس، دسترسی به پراپرتی های ایستا از طریق متدهای همان کلاس، با استفاده از کلمه کلیدی `self` و علامت دو نقطه (::) امکان‌پذیر است.</p>

```php
class Circle {
  // static property
  public static $pi = 3.14159;

  public function staticValue() {
    return self::$pi;
  }
}

// Get static property directly
$circle = new Circle;
echo $circle->staticValue();

```

<p>برای فراخوانی یک ویژگی استاتیک از یک کلاس فرزند، از کلمه کلیدی `parent` در داخل کلاس فرزند استفاده کنید.</p>

```php
class Shape {
  public static $pi=3.14159;
}

class Circle extends Shape {
  public function getPi() {
    return parent::$pi;
  }
}

// Get value of static property directly via child class
echo Circle::$pi;

// or get value of static property via getPi() method
$circle = new Circle;
echo $circle->getPi();
```

---
[صفحه قبلی: traits   ➡️](11-traits.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: namespace ➡️](12-namespace.md)