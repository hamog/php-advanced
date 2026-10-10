# Getter, Setter
```php
class Fruit {
  // Properties (Variables)
  public $name;
  public $color;

  // Setter
  function setDetils($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }

  // Getter
  function getDetails() {
    return $this->name . ' , ' . $this->color;
  }
}

```

---
[صفحه قبلی: تعریف کلاس, پراپرتی و متد ➡️](01-class.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: ارث‌بری ➡️](00-introduction.md)
