# Getter, Setter
<p>
  بطور خلاصه، setter برای تغییر مقدار property و getter برای دریافت مقدار property یک کلاس استفاده میشود.
</p>

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

$fruit = new Fruit;
$fruit->setDetils('Banana', 'Yellow');
echo $fruit->getDetails();
```

---
[صفحه قبلی: تعریف متد با پارامتر ➡️](02-method-parameter.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: متد سازنده ➡️](04-constructor.md)
