# وراثت
<p>وراثت در برنامه‌نویسی شیءگرا (OOP) در PHP به یک کلاس فرزند اجازه می‌دهد تا تمام پراپرتی ها و متدهای عمومی (public) و محافظت‌شده (protected) را از یک کلاس والد به ارث ببرد. علاوه بر این، کلاس فرزند می‌تواند ویژگی‌ها و متدهای مختص به خود را نیز داشته باشد.</p>
<p>نکته: متدهای خصوصی (private) یک کلاس والد برای کلاس فرزند قابل دسترسی نیستند.</p>
<p>یک کلاس مشتق‌شده با استفاده از کلمه کلیدی extends تعریف می‌شود.</p>

```php
// Parent class
class Fruit {
  public $name;
  public $color;

  public function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  public function intro() {
    echo "The fruit is $this->name and the color is $this->color.<br>";
  }
}

// Strawberry is inherited from Fruit
class Strawberry extends Fruit {
  public function message() {
    echo "Am I a fruit or a berry? ";
  }
}

$strawberry = new Strawberry("Strawberry", "red");
$strawberry->intro();
$strawberry->message();
```

### وراثت و سطح دسترسی Protected
<p>در فصل قبل آموختیم که ویژگی‌ها یا متدهای محافظت‌شده (protected) از درون همان کلاس و همچنین توسط کلاس‌های مشتق‌شده از آن، قابل دسترسی هستند. این به چه معناست؟</p>

```php
class Fruit {
  public $name;
  public $color;

  public function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  protected function intro() {
    echo "The fruit is $this->name and the color is $this->color.";
  }
}

class Strawberry extends Fruit {
  public function message() {
    echo "Am I a fruit or a berry? ";
  }
}

// Try to call all three methods from outside class
$strawberry = new Strawberry("Strawberry", "red");  // OK. __construct() is public
$strawberry->message(); // OK. message() is public
$strawberry->intro(); // ERROR. intro() is protected
```

### بازنویسی (override) متدهای ارث‌بری شده
<p>متدهای به ارث رسیده را می‌توان با تعریف مجدد آن‌ها (با استفاده از همان نام) در کلاس فرزند، بازنویسی (Override) کرد.</p>

```php
class Fruit {
  public $name;
  public $color;

  public function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  public function intro() {
    echo "The fruit is $this->name and the color is $this->color.";
  }
}

class Strawberry extends Fruit {
  public $weight;

  public function __construct($name, $color, $weight) {
    $this->name = $name;
    $this->color = $color;
    $this->weight = $weight;
  }

  public function intro() {
    echo "A $this->name is $this->color, and the weight is $this->weight gram.";
  }
}

$strawberry = new Strawberry("Strawberry", "red", 50);
$strawberry->intro();
```

### کلمه کلیدی final
<p>کلمه کلیدی final می‌تواند برای جلوگیری از ارث‌بری کلاس یا جلوگیری از override شدن متد استفاده شود.</p>

```php
//final class
final class Fruit {
  // some code
}

// will result in error
class Strawberry extends Fruit {
  // some code
}

//final method
class Fruit {
  final public function intro() {
    // some code
  }
}

class Strawberry extends Fruit {
  // will result in error
  public function intro() {
    // some code
  }
}
```

---
[صفحه قبلی: تعریف سطح دسترسی ➡️](06-access-modifiers.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: وراثت ➡️](07-inheritance.md)