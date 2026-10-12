# کلاس های رابط (Interface)
<p>یک «اینترفیس» (Interface) به شما امکان می‌دهد مشخص کنید که یک کلاس ملزم به پیاده‌سازی کدام متدهای عمومی است، بدون آنکه نحوه پیاده‌سازی آن‌ها را تعیین کنید.</p>
<p>اینترفیس‌ها با کلمه کلیدی `interface` تعریف می‌شوند و متدهای تعریف‌شده در یک اینترفیس باید `public` باشند:</p>
<p>برای پیاده‌سازی یک اینترفیس (Interface)، یک کلاس باید از کلمه کلیدی `implements` استفاده کند.</p>
<p>کلاسی که یک اینترفیس را پیاده‌سازی می‌کند، باید تمام متدهای آن اینترفیس را پیاده‌سازی نماید.</p>

```php
// Define the interface
interface Animal {
  public function makeSound();
}

// Implement the interface in a class
class Cat implements Animal {
  public function makeSound() {
    echo "Meow";
  }
}

// Implement the interface in another class
class Dog implements Animal {
  public function makeSound() {
    echo "Woff";
  }
}

$cat = new Cat();
$cat->makeSound();

$dog = new Dog();
$dog->makeSound();
```

### تفاوت اینترفیس ها با abstract
<p>اینترفیس‌ها (Interface) به کلاس‌های انتزاعی (Abstract Class) شباهت دارند. تفاوت میان اینترفیس‌ها و کلاس‌های انتزاعی عبارت است از:</p> 
<ul>
  <li>رابط‌ها نمی‌توانند پراپرتی داشته باشند، در حالی که کلاس‌های انتزاعی می‌توانند</li>
  <li>تمام متدهای رابط (Interface) باید عمومی (public) باشند، در حالی که متدهای انتزاعی (abstract) می‌توانند عمومی (public) یا محافظت‌شده (protected) باشند.</li>
  <li>تمام متدها در یک اینترفیس (Interface) انتزاعی هستند؛ بنابراین نمی‌توان آن‌ها را در کد پیاده‌سازی کرد و استفاده از کلمه کلیدی abstract نیز ضروری نیست.</li>
  <li>کلاس‌ها می‌توانند هم‌زمان با ارث‌بری از یک کلاس دیگر، یک اینترفیس (Interface) را نیز پیاده‌سازی کنند.</li>
</ul>
<p>رابط‌ها (Interfaces) استفاده از کلاس‌های گوناگون را به شیوه‌ای یکسان، آسان می‌سازند. هنگامی که یک یا چند کلاس از رابط یکسانی استفاده می‌کنند، این پدیده «چندریختی» (Polymorphism) نامیده می‌شود.</p>

---
[صفحه قبلی: انتزاع   ➡️](08-constants.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: وراثت ➡️](07-inheritance.md)