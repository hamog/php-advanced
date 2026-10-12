# تعریف متد با پارامتر
```php
class Fruit {
  // Properties (Variables)
  public $name = 'Apple';
  public $color = 'Red';

  // Methods (functions)
  function showDescription($name = 'Apple', $color) {
    echo "The {$color} {$name} is a good and beneficial fruit.";
  }
}

class Calculator {
  function sum($x, $y) {
    return $x + $y;
  }
}
```

<p>یک شیء، نمونه‌ای منفرد از یک کلاس است.</p>

```php
$fruit = new Fruit;
$fruit->showDescription('Banana', 'Yellow');

$calc = new Calculator;
echo $calc->sum(5, 8);
```

# استفاده از کلمه کلیدی $this
<p>با استفاده از کلمه کلیدی $this به محتویات داخل یک کلاس دسترسی داریم.</p>

```php
class Fruit {
  // Properties (Variables)
  public $name = 'Apple';
  public $color = 'Red';

  // Methods (functions)
  function showDescription() {
    echo "The {$this->color} {$this->name} is a good and beneficial fruit.";
  }

  function print() {
    return $this->showDescription();
  }
}
```

---
[صفحه قبلی: تعریف کلاس, پراپرتی و متد ➡️](01-class.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: Getter,Setter  ➡️](03-getter-setter.md)
