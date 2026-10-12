# کلاس های انتزاعی (Abstract)
<p>کلاس انتزاعی (Abstract Class) کلاسی است که حداقل یک متد انتزاعی (Abstract Method) دارد. متد انتزاعی متدی است که در کلاس انتزاعی تعریف (اعلام) شده، اما پیاده‌سازی نشده است؛ پیاده‌سازی آن باید در کلاس(های) فرزند انجام شود.</p>
<p>هدف از کلاس انتزاعی (Abstract Class)، ملزم کردن تمام کلاس‌های مشتق‌شده (کلاس‌های فرزند) به پیاده‌سازی متد(های) انتزاعیِ تعریف‌شده در کلاس والد است.</p>
<p>یک کلاس یا متد انتزاعی با کلمه کلیدی abstract تعریف می‌شود.</p>

```php
// Abstract base class
abstract class Car {
  public $name;

  // Non-abstract method
  public function __construct($name) {
    $this->name = $name;
  }

  // Abstract method - forces child classes to implement it
  abstract public function intro();
}

// Child class that extends the abstract class
class Audi extends Car {
  public function intro() {
    return "German quality! I'm an $this->name!";
  }
}

// Child class that extends the abstract class
class Citroen extends Car {
  public function intro() {
    return "French extravagance! I'm a $this->name!";
  }
}

// Create objects of the child classes
$audi = new audi("Audi");
echo $audi->intro();
echo "<br>";

$citroen = new citroen("Citroen");
echo $citroen->intro();
```

### نکاتی در مورد کلاس های انتزاعی
<p>هنگام ارث‌بری از یک کلاس انتزاعی (abstract class)، متد کلاس فرزند باید با همان نام و با اصلاح‌گر دسترسی (access modifier) ​​یکسان یا با محدودیت کمتر تعریف شود. بنابراین، اگر متد انتزاعی به صورت `protected` تعریف شده باشد، متد کلاس فرزند باید به صورت `protected` یا `public` تعریف شود، اما نباید `private` باشد. همچنین، نوع و تعداد آرگومان‌های الزامی باید یکسان باشد؛ با این حال، کلاس‌های فرزند می‌توانند آرگومان‌های اختیاری نیز داشته باشند.</p>
<p>بنابراین، هنگامی که یک کلاس فرزند از یک کلاس انتزاعی (Abstract Class) به ارث برده می‌شود، قوانین زیر برقرار است:</p>
<ul>
  <li>متد کلاس فرزند باید با همان نام تعریف شود و متد انتزاعی (abstract) کلاس والد را مجدداً تعریف (re-declare) کند.</li>
  <li>متد کلاس فرزند باید با همان سطح دسترسی یا سطحی با محدودیت کمتر (مجوز دسترسی بازتر) تعریف شود.</li>
  <li>تعداد آرگومان‌های الزامی باید یکسان باشد؛ با این حال، کلاس فرزند می‌تواند علاوه بر آن‌ها، آرگومان‌های اختیاری نیز داشته باشد.</li>
</ul>

### متد انتزاعی دارای آرگومان

```php
abstract class ParentClass {
  // Abstract method with an argument
  abstract protected function prefixName($name);
}

class ChildClass extends ParentClass {
  public function prefixName($name) {
    if ($name == "John Doe") {
      $prefix = "Mr.";
    } elseif ($name == "Jane Doe") {
      $prefix = "Mrs.";
    } else {
      $prefix = "";
    }
    return "$prefix $name";
  }
}

$class = new ChildClass;
echo $class->prefixName("John Doe");
echo "<br>";
echo $class->prefixName("Jane Doe");
echo "<br>";
echo $class->prefixName("Baby Doe");


//with optional arguments
abstract class ParentClass {
  // Abstract method with an argument
  abstract protected function prefixName($name);
}

class ChildClass extends ParentClass {
  // The child class may define optional arguments that are not in the parent's abstract method
  public function prefixName($name, $separator = ".", $greet = "Dear") {
    if ($name == "John Doe") {
      $prefix = "Mr";
    } elseif ($name == "Jane Doe") {
      $prefix = "Mrs";
    } else {
      $prefix = "";
    }
    return "$greet $prefix$separator $name";
  }
}

$class = new ChildClass;
echo $class->prefixName("John Doe");
echo "<br>";
echo $class->prefixName("Jane Doe");
```

### 

---
[صفحه قبلی: ثابت ها   ➡️](08-constants.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: رابط ها ➡️](10-interface.md)