# سطح دسترسی یا Access Modifiers
<p>ویژگی‌ها و متدها می‌توانند دارای اصلاح‌کننده‌های دسترسی (یا کلمات کلیدی تعیین‌کننده سطح دسترسی) باشند که تعیین می‌کنند از کجا می‌توان به آن‌ها دسترسی داشت.</p>
<p>در PHP سه نوع تعیین‌کننده سطح دسترسی (Access Modifier) ​​وجود دارد:</p>
<p>public - امکان دسترسی به ویژگی یا متد از هر جایی وجود دارد (این حالت پیش‌فرض است).</p>
<p>protected - دسترسی به ویژگی یا متد تنها در داخل همان کلاس و کلاس‌های مشتق‌شده از آن امکان‌پذیر است.</p>
<p>private - دسترسی به ویژگی یا متد تنها در داخل همان کلاسی که در آن تعریف شده‌اند، امکان‌پذیر است.</p>
<p>نکته: اگر هیچ اصلاح‌گر دسترسی‌ای مشخص نشود، مقدار آن public تعیین خواهد شد.</p>

### public
<p>اصلاح‌گر دسترسی public امکان دسترسی به ویژگی‌ها یا متدهای کلاس را از هر جایی فراهم می‌کند.</p>

```php
class Fruit {
  public $name;

  public function getDetails() {
    echo "Name: " . $this->name . ".";
  }
}

$apple = new Fruit();
$apple->name = "Apple"; // Can be accessed directly
$apple->getDetails();
```

### private
<p>اصلاح‌کننده دسترسی private (خصوصی) امکان دسترسی به ویژگی‌ها یا متدهای کلاس را *تنها* در همان کلاسی که در آن تعریف شده‌اند، فراهم می‌کند.</p>

```php
class Fruit {
  private $name;

  public function getDetails() {
    echo "Name: " . $this->name . ".";
  }
}

$apple = new Fruit();
$apple->name = "Apple"; // Error: Cannot access private property
$apple->getDetails();
```

### protected
<p>اصلاح‌گر دسترسی protected امکان دسترسی به ویژگی‌ها یا متدهای کلاس را هم در داخل همان کلاس و هم توسط کلاس‌های مشتق‌شده از آن (کلاس‌های فرزند) فراهم می‌کند.</p>

```php
class Fruit {
  protected $name;

  public function getDetails() {
    echo "Name: " . $this->name . ".";
  }
}

$apple = new Fruit();
$apple->name = "Apple"; // Error: Cannot access protected property
$apple->getDetails();
```
<p>اما در کلاس فرزند قابل دسترسی است:</p>

```php
class Fruit {
  protected $name;

  public function setType($name) {
    $this->name = $name;
  }
}

class Apple extends Fruit {
  public function getType() {
    echo "Name: " . $this->name . ".";
  }
}

$apple = new Apple();
$apple->setType("Apple");
//echo $apple->name; // Error: Cannot access protected property
echo $apple->getType(); // Output: Name: Apple.
```

---
[صفحه قبلی: تعریف متد تخریب کننده ➡️](05-destructor.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: وراثت ➡️](07-inheritance.md)