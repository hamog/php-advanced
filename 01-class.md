# تعریف کلاس, شی, پراپرتی , متد
<p>کلاس قالبی برای اشیاء است و ساختار (properties) و رفتار (methods) یک شیء را تعیین می‌کند.</p>

```php
class Fruit {
  // Properties (Variables)
  public $name = 'Apple';
  public $color = 'Red';

  // Methods (functions)
  function showDescription() {
    echo 'The red apple is a good and beneficial fruit.';
  }
}
```

<p>یک شیء، نمونه‌ای منفرد از یک کلاس است.</p>

```php
$fruit = new Fruit();
echo $fruit->name;
$fruit->showDescription();
```

نکته: هرتعداد شی می توانیم از یک کلاس ایجاد کنیم .

```php
$fruit = new Fruit();
$fruit2 = new Fruit();
$fruit3 = new Fruit();
    ...
```

---
[صفحه قبلی: مقدمه ➡️](00-introduction.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی:  تعریف متد با پارامتر ➡️](02-method-parameter.md)
