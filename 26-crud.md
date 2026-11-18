# آموزش پیاده‌سازی CRUD دسته‌بندی در میکروفریم‌ورک MVC با PHP

## فهرست مطالب

1. [مقدمه](#مقدمه)
2. [آشنایی با مفهوم Model](#آشنایی-با-مفهوم-model)
3. [ایجاد کلاس انتزاعی Model](#ایجاد-کلاس-انتزاعی-model)
4. [ایجاد مدل Category](#ایجاد-مدل-category)
5. [ایجاد کنترلر CategoryController](#ایجاد-کنترلر-categorycontroller)
6. [پیاده‌سازی عملیات نمایش دسته‌بندی‌ها](#۱-نمایش-لیست-دسته‌بندیها)
7. [پیاده‌سازی فرم ثبت دسته‌بندی](#۲-نمایش-فرم-ثبت-دسته‌بندی)
8. [ذخیره دسته‌بندی جدید](#۳-ذخیره-دسته‌بندی-جدید)
9. [پیاده‌سازی فرم ویرایش دسته‌بندی](#۴-نمایش-فرم-ویرایش-دسته‌بندی)
10. [به‌روزرسانی دسته‌بندی](#۵-بهروزرسانی-دسته‌بندی)
11. [حذف دسته‌بندی](#۶-حذف-دسته‌بندی)
12. [تعریف مسیرهای CRUD](#تعریف-مسیرهای-crud)
13. [افزودن متد Redirect به Controller](#افزودن-متد-redirect-به-controller)
14. [تنظیم منطقه زمانی برنامه](#تنظیم-منطقه-زمانی-برنامه)
15. [بررسی جریان اجرای برنامه](#بررسی-جریان-اجرای-برنامه)
16. [نکات مهم امنیتی و بهبود کد](#نکات-مهم-امنیتی-و-بهبود-کد)

---

## مقدمه

تا این مرحله از آموزش، ما یک میکروفریم‌ورک ساده بر اساس معماری MVC با زبان PHP ایجاد کرده‌ایم.

در این ساختار، قسمت‌های مختلف برنامه وظایف مشخصی دارند:

- **Model:** مسئول ارتباط با دیتابیس و اجرای عملیات مربوط به داده‌ها است.
- **View:** مسئول نمایش اطلاعات به کاربر و دریافت اطلاعات از طریق فرم‌ها است.
- **Controller:** مسئول مدیریت درخواست‌های کاربر و ارتباط بین Model و View است.
- **Router:** درخواست‌های ورودی را بررسی کرده و آن‌ها را به متد مناسب در کنترلر هدایت می‌کند.

در این جلسه قصد داریم یک بخش مدیریتی برای دسته‌بندی‌ها ایجاد کنیم.

فرض کنید در یک وب‌سایت، قصد داریم دسته‌بندی‌هایی مانند موارد زیر داشته باشیم:

- اخبار فناوری
- آموزش برنامه‌نویسی
- مقالات آموزشی
- اخبار عمومی

برای مدیریت این اطلاعات به قابلیت‌های زیر نیاز داریم:

| عملیات | توضیحات |
|---|---|
| Create | ثبت دسته‌بندی جدید |
| Read | مشاهده لیست دسته‌بندی‌ها |
| Update | ویرایش اطلاعات دسته‌بندی |
| Delete | حذف دسته‌بندی |

به مجموعه این چهار عملیات CRUD گفته می‌شود.

CRUD مخفف چهار کلمه زیر است:

- Create
- Read
- Update
- Delete

در ادامه، هرکدام از این عملیات را با استفاده از ساختار MVC پیاده‌سازی می‌کنیم.

## آشنایی با مفهوم Model

در معماری MVC، مدل (Model) بخشی از برنامه است که مسئول مدیریت داده‌ها و ارتباط با دیتابیس است.

برای مثال، فرض کنید در برنامه خود سه موجودیت مختلف داریم:

- Category
- Post
- User

هرکدام از این موجودیت‌ها اطلاعات مربوط به خود را در دیتابیس مدیریت می‌کنند.

بهتر است برای هر موجودیت یک کلاس مدل مجزا ایجاد کنیم.

ساختار پیشنهادی پروژه به شکل زیر خواهد بود:

```text
app/
├── Controllers/
│   └── Admin/
│       └── CategoryController.php
│
├── Models/
│   └── Category.php
│
└── Views/
    └── admin/
        └── category/
            ├── index.php
            ├── create.php
            └── edit.php

core/
└── Libraries/
    ├── Controller.php
    ├── Database.php
    ├── Model.php
    ├── Router.php
    └── View.php
```

در این ساختار، کلاس Category مسئول مدیریت اطلاعات جدول categories است.

اما ممکن است در آینده مدل‌های دیگری مانند Post و User نیز به برنامه اضافه شوند.

از آنجایی که تمام این مدل‌ها برای ارتباط با دیتابیس به قابلیت‌های مشابهی نیاز دارند، بهتر است یک کلاس پایه ایجاد کنیم تا مدل‌های دیگر از آن ارث‌بری کنند.

برای این کار از مفهوم کلاس انتزاعی (Abstract Class) در PHP استفاده می‌کنیم.

## ایجاد کلاس انتزاعی Model

ابتدا در مسیر زیر یک فایل جدید با نام Model.php ایجاد می‌کنیم:

```text
core/Libraries/Model.php
```

محتوای فایل:

```php
<?php

namespace Core\Libraries;

abstract class Model
{
    protected string $table;

    protected Database $db;

    public function __construct()
    {
        $database = new Database();
        $this->db = $database;
    }

    abstract public function query(string $sql);
}
```

### بررسی کد

**۱. تعریف کلاس انتزاعی**

```php
abstract class Model
```

با استفاده از کلمه کلیدی `abstract` یک کلاس انتزاعی ایجاد کرده‌ایم.

کلاس انتزاعی کلاسی است که نمی‌توانیم مستقیماً از آن نمونه‌سازی (Instantiate) کنیم.

برای مثال، دستور زیر مجاز نیست:

```php
$model = new Model();
```

هدف ما این است که مدل‌های دیگر از این کلاس ارث‌بری کنند و امکانات مشترک خود را از آن دریافت کنند.

**۲. تعریف ویژگی table**

```php
protected string $table;
```

این ویژگی نام جدول دیتابیس مربوط به هر مدل را نگهداری می‌کند.

برای مثال:

- در مدل Category مقدار آن `categories` خواهد بود.
- در مدل Post مقدار آن `posts` خواهد بود.
- در مدل User مقدار آن `users` خواهد بود.

از آنجایی که این ویژگی با سطح دسترسی `protected` تعریف شده است، کلاس‌های فرزند نیز می‌توانند به آن دسترسی داشته باشند.

**۳. تعریف ویژگی db**

```php
protected Database $db;
```

این ویژگی یک شیء از کلاس Database را نگهداری می‌کند.

با استفاده از این ویژگی، تمام مدل‌های فرزند می‌توانند از امکانات کلاس Database برای ارتباط با دیتابیس استفاده کنند.

**۴. سازنده کلاس**

```php
public function __construct()
{
    $database = new Database();
    $this->db = $database;
}
```

در سازنده کلاس Model یک نمونه از کلاس Database ایجاد می‌کنیم و آن را در ویژگی `$db` قرار می‌دهیم.

بنابراین هر زمان از یک مدل که از Model ارث‌بری کرده است نمونه‌سازی کنیم، ابتدا سازنده کلاس والد اجرا شده و اتصال دیتابیس در اختیار مدل قرار می‌گیرد.

**۵. تعریف متد انتزاعی query**

```php
abstract public function query(string $sql);
```

با استفاده از کلمه کلیدی `abstract` یک متد انتزاعی تعریف کرده‌ایم.

متد انتزاعی بدنه ندارد و کلاس فرزند موظف است آن را پیاده‌سازی کند.

در این مثال، قصد داریم تمام مدل‌های برنامه امکان اجرای کوئری SQL را داشته باشند.

به همین دلیل پیاده‌سازی این متد را به کلاس‌های فرزند واگذار کرده‌ایم.

> نکته: تفاوت کلاس انتزاعی با کلاس معمولی این است که کلاس انتزاعی می‌تواند شامل متدهای پیاده‌سازی‌شده و متدهای انتزاعی باشد. همچنین نمی‌توان مستقیماً از آن نمونه‌سازی کرد.

## ایجاد مدل Category

حالا که کلاس پایه Model را ایجاد کردیم، می‌توانیم مدل مربوط به دسته‌بندی‌ها را بسازیم.

در مسیر زیر فایل Category.php را ایجاد می‌کنیم:

```text
app/Models/Category.php
```

محتوای فایل:

```php
<?php

namespace App\Models;

use Core\Libraries\Model;

class Category extends Model
{
    protected string $table = 'categories';

    public function getAll(): array
    {
        return $this->db->select(
            "SELECT * FROM `{$this->table}` ORDER BY `created_at` DESC",
            []
        );
    }

    public function query(string $sql)
    {
        return $this->db->query($sql);
    }

    public function insert(array $data): bool
    {
        return $this->db->insert($this->table, $data);
    }

    public function update(array $data, $id): bool
    {
        return $this->db->update($this->table, $data, "id = $id");
    }

    public function delete($id): bool
    {
        return $this->db->delete($this->table, "id = $id");
    }
}
```

### بررسی کد مدل Category

**۱. ارث‌بری از Model**

```php
class Category extends Model
```

با استفاده از `extends` مشخص می‌کنیم که کلاس Category از کلاس Model ارث‌بری می‌کند.

بنابراین ویژگی‌ها و متدهای قابل دسترسی کلاس Model در اختیار Category قرار می‌گیرند.

برای مثال، دیگر نیازی نیست در کلاس Category مجدداً یک نمونه از Database ایجاد کنیم.

**۲. مشخص کردن نام جدول**

```php
protected string $table = 'categories';
```

نام جدول مربوط به این مدل را مشخص می‌کنیم.

از این ویژگی در متدهای مختلف برای اجرای عملیات روی جدول دسته‌بندی‌ها استفاده خواهیم کرد.

**۳. دریافت تمام دسته‌بندی‌ها**

```php
public function getAll(): array
{
    return $this->db->select(
        "SELECT * FROM `{$this->table}` ORDER BY `created_at` DESC",
        []
    );
}
```

این متد تمام رکوردهای جدول categories را دریافت می‌کند.

در کوئری از دستور زیر استفاده کرده‌ایم:

```sql
SELECT * FROM categories
ORDER BY created_at DESC
```

دستور `ORDER BY` برای مرتب‌سازی نتایج استفاده می‌شود.

عبارت `DESC` نیز مشخص می‌کند که اطلاعات به صورت نزولی مرتب شوند؛ یعنی دسته‌بندی‌هایی که جدیدتر ثبت شده‌اند در ابتدای لیست قرار بگیرند.

خروجی این متد یک آرایه از دسته‌بندی‌ها است.

**۴. پیاده‌سازی متد query**

```php
public function query(string $sql)
{
    return $this->db->query($sql);
}
```

این متد برای اجرای مستقیم دستورات SQL استفاده می‌شود.

در واقع، متد انتزاعی query که در کلاس Model تعریف کرده بودیم، در این قسمت پیاده‌سازی شده است.

**۵. ثبت اطلاعات جدید**

```php
public function insert(array $data): bool
{
    return $this->db->insert($this->table, $data);
}
```

این متد یک آرایه از اطلاعات دریافت می‌کند و آن را در جدول مربوط به مدل ذخیره می‌کند.

برای مثال، آرایه زیر می‌تواند برای ثبت یک دسته‌بندی استفاده شود:

```php
$data = [
    'name' => 'آموزش PHP',
    'status' => 1,
    'created_at' => date('Y-m-d H:i:s')
];
```

با فراخوانی متد insert، اطلاعات در جدول categories ذخیره می‌شوند.

**۶. به‌روزرسانی اطلاعات**

```php
public function update(array $data, $id): bool
{
    return $this->db->update($this->table, $data, "id = $id");
}
```

این متد دو ورودی دریافت می‌کند:

- `$data`: اطلاعات جدیدی که باید ذخیره شوند.
- `$id`: شناسه رکوردی که قصد ویرایش آن را داریم.

برای مثال، اگر بخواهیم نام دسته‌بندی با شناسه 5 را تغییر دهیم، می‌توانیم از این متد استفاده کنیم.

**۷. حذف اطلاعات**

```php
public function delete($id): bool
{
    return $this->db->delete($this->table, "id = $id");
}
```

این متد شناسه دسته‌بندی را دریافت کرده و رکورد مربوط به آن را از دیتابیس حذف می‌کند.

به این ترتیب، مدل Category امکانات مورد نیاز برای مدیریت اطلاعات دسته‌بندی‌ها را در اختیار کنترلر قرار می‌دهد.

> نکته: متدهای `insert`، `update` و `delete` در این مدل به متدهای هم‌نام در کلاس Database وابسته هستند. بنابراین کلاس Database باید این متدها را پیاده‌سازی کرده باشد.

## ایجاد کنترلر CategoryController

حالا نوبت به ایجاد کنترلر مربوط به مدیریت دسته‌بندی‌ها می‌رسد.

کنترلر وظیفه دارد درخواست‌های کاربر را دریافت کند، عملیات مورد نیاز را از طریق مدل انجام دهد و در نهایت View مناسب را نمایش دهد.

در مسیر زیر فایل CategoryController.php را ایجاد می‌کنیم:

```text
app/Controllers/Admin/CategoryController.php
```

محتوای فایل:

```php
<?php

namespace App\Controllers\Admin;

use App\Models\Category;
use Core\Libraries\Controller;

class CategoryController extends Controller
{
    private Category $category;

    public function __construct()
    {
        $this->category = new Category();
    }

    public function index(): void
    {
        $categories = $this->category->getAll();

        $this->view('admin.category.index', [
            'categories' => $categories
        ]);
    }

    public function create(): void
    {
        $this->view('admin.category.create');
    }

    public function store(array $inputs): void
    {
        // validation
        $name = trim($inputs['name'] ?? '');

        if ($name === '') {
            $this->redirect('/categories/create', [
                'errors' => [
                    'name' => 'نام دسته‌بندی را وارد کنید!'
                ]
            ]);
        }

        $inputs['name'] = $name;
        $inputs['created_at'] = date('Y-m-d H:i:s');
        $inputs['status'] = isset($inputs['status']) ? 1 : 0;

        $this->category->insert($inputs);

        $this->redirect('/categories', [
            'success' => 'دسته‌بندی با موفقیت ثبت شد.'
        ]);
    }

    public function edit(int $id): void
    {
        $query = $this->category->query(
            'SELECT * FROM categories WHERE id = ' . $id
        );

        $category = $query->fetch();

        if (!$category) {
            $this->redirect('/categories', [
                'error' => 'دسته بندی انتخاب شده پیدا نشد!'
            ]);

            return;
        }

        $this->view('admin.category.edit', [
            'category' => $category
        ]);
    }

    public function update(array $inputs, int $id): void
    {
        $inputs['status'] = isset($inputs['status']) ? 1 : 0;

        $this->category->update($inputs, $id);

        $this->redirect('/categories', [
            'success' => 'دسته بندی با موفقیت به روزرسانی شد.'
        ]);
    }

    public function delete(int $id): void
    {
        $this->category->delete($id);

        $this->redirect('/categories', [
            'success' => 'دسته بندی با موفقیت حذف شد.'
        ]);
    }
}
```

### بررسی ساختار کنترلر

**۱. استفاده از کلاس‌های مورد نیاز**

```php
use App\Models\Category;
use Core\Libraries\Controller;
```

در این قسمت کلاس Category و کلاس پایه Controller را وارد می‌کنیم.

از آنجایی که CategoryController قرار است از امکانات مدل Category استفاده کند، باید کلاس مربوطه را در اختیار داشته باشد.

همچنین با ارث‌بری از Controller می‌توانیم از متدهایی مانند `view` و `redirect` استفاده کنیم.

**۲. تعریف ویژگی category**

```php
private Category $category;
```

یک ویژگی خصوصی از نوع Category تعریف کرده‌ایم.

هدف این است که نمونه مدل Category را در این ویژگی نگهداری کنیم تا تمام متدهای کنترلر بتوانند از آن استفاده کنند.

**۳. سازنده کنترلر**

```php
public function __construct()
{
    $this->category = new Category();
}
```

در سازنده کنترلر یک نمونه از مدل Category ایجاد می‌کنیم.

از این به بعد در تمام متدهای کنترلر می‌توانیم با استفاده از `$this->category` به امکانات مدل دسترسی داشته باشیم.

---

## ۱. نمایش لیست دسته‌بندی‌ها

اولین عملیات CRUD، نمایش اطلاعات موجود در دیتابیس است.

برای این کار متد index را در کنترلر پیاده‌سازی کرده‌ایم:

```php
public function index(): void
{
    $categories = $this->category->getAll();

    $this->view('admin.category.index', [
        'categories' => $categories
    ]);
}
```

در این متد ابتدا با استفاده از مدل Category، تمام دسته‌بندی‌ها را دریافت می‌کنیم:

```php
$categories = $this->category->getAll();
```

سپس اطلاعات دریافت‌شده را به View ارسال می‌کنیم:

```php
$this->view('admin.category.index', [
    'categories' => $categories
]);
```

در اینجا از متد view که قبلاً در کلاس Controller پیاده‌سازی کرده‌ایم استفاده می‌کنیم.

مسیر View را به صورت زیر مشخص کرده‌ایم:

```text
admin.category.index
```

کلاس View این مسیر را به مسیر فیزیکی زیر تبدیل می‌کند:

```text
app/Views/admin/category/index.php
```

### ایجاد View لیست دسته‌بندی‌ها

در مسیر زیر فایل index.php را ایجاد می‌کنیم:

```text
app/Views/admin/category/index.php
```

محتوای فایل:

```php
<div class="card mt-5">
    <div class="card-header">
        <h1>لیست دسته بندی ها</h1>

        <div class="float-left">
            <a class="btn btn-primary"
               href="<?= BASE_URL ?>/categories/create"
               role="button">
                ثبت دسته بندی
            </a>
        </div>
    </div>

    <div class="card-body">
        <table class="table table-bordered">
            <thead>
            <tr>
                <th>شناسه</th>
                <th>نام دسته بندی</th>
                <th>وضعیت</th>
                <th>تاریخ ثبت</th>
                <th>عملیات</th>
            </tr>
            </thead>

            <tbody>
            <?php foreach ($categories as $category): ?>
                <tr>
                    <td scope="row">
                        <?= $category['id'] ?>
                    </td>

                    <td>
                        <?= $category['name'] ?>
                    </td>

                    <td>
                        <span class="badge badge-<?= $category['status'] ? 'success' : 'danger' ?>">
                            <?= $category['status'] ? 'فعال' : 'غیرفعال' ?>
                        </span>
                    </td>

                    <td>
                        <?= $category['created_at'] ?? '-' ?>
                    </td>

                    <td>
                        <a class="btn btn-warning"
                           href="<?= BASE_URL ?>/categories/edit/<?= $category['id'] ?>"
                           role="button">
                            ویرایش
                        </a>

                        <a class="btn btn-danger"
                           href="<?= BASE_URL ?>/categories/delete/<?= $category['id'] ?>"
                           role="button">
                            حذف
                        </a>
                    </td>
                </tr>
            <?php endforeach; ?>
            </tbody>
        </table>
    </div>
</div>
```

### بررسی کد View

در این فایل از HTML و PHP برای نمایش اطلاعات استفاده کرده‌ایم.

**۱. نمایش اطلاعات با foreach**

```php
<?php foreach ($categories as $category): ?>
```

از حلقه foreach استفاده کرده‌ایم تا تمام دسته‌بندی‌های دریافت‌شده از دیتابیس را پیمایش کنیم.

در هر بار اجرای حلقه، اطلاعات یک دسته‌بندی در متغیر `$category` قرار می‌گیرد.

**۲. نمایش وضعیت دسته‌بندی**

```php
<?= $category['status'] ? 'فعال' : 'غیرفعال' ?>
```

در این قسمت از عملگر سه‌تایی (Ternary Operator) استفاده کرده‌ایم.

اگر مقدار status برابر با مقدار درست باشد، عبارت فعال و در غیر این صورت عبارت غیرفعال نمایش داده می‌شود.

همچنین کلاس CSS مربوط به Badge نیز بر اساس وضعیت دسته‌بندی تغییر می‌کند.

**۳. لینک ویرایش**

```php
href="<?= BASE_URL ?>/categories/edit/<?= $category['id'] ?>"
```

با کلیک روی لینک ویرایش، کاربر به صفحه ویرایش همان دسته‌بندی منتقل می‌شود.

شناسه دسته‌بندی نیز در انتهای URL قرار می‌گیرد.

برای مثال:

```text
/categories/edit/5
```

در این مثال قصد داریم دسته‌بندی با شناسه 5 را ویرایش کنیم.

**۴. لینک حذف**

```php
href="<?= BASE_URL ?>/categories/delete/<?= $category['id'] ?>"
```

این لینک نیز شناسه دسته‌بندی را به مسیر حذف ارسال می‌کند.

در ادامه متد مربوط به حذف را پیاده‌سازی خواهیم کرد.

---

## ۲. نمایش فرم ثبت دسته‌بندی

برای ثبت یک دسته‌بندی جدید، ابتدا باید فرمی در اختیار کاربر قرار دهیم.

در کنترلر متد create را تعریف کرده‌ایم:

```php
public function create(): void
{
    $this->view('admin.category.create');
}
```

این متد فقط وظیفه نمایش فرم ثبت دسته‌بندی را بر عهده دارد.

فایل View مربوط به آن را در مسیر زیر ایجاد می‌کنیم:

```text
app/Views/admin/category/create.php
```

محتوای فایل:

```php
<div class="card mt-5">
    <div class="card-header">
        <h1>ثبت دسته بندی</h1>
    </div>

    <div class="card-body">
        <form action="<?= BASE_URL ?>/categories/store" method="post">

            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label class="control-leabel" for="name">
                            نام
                        </label>

                        <input type="text"
                               name="name"
                               id="name"
                               class="form-control">

                        <?php if (isset($_SESSION['errors']['name'])): ?>
                            <div class="form-text text-danger">
                                <?= $_SESSION['errors']['name']; ?>
                            </div>
                        <?php endif; ?>
                    </div>
                </div>

                <div class="col">
                    <div class="form-group">
                        <label class="control-leabel" for="status">
                            وضعیت فعال بودن
                        </label>

                        <input type="checkbox"
                               class="form-control"
                               name="status"
                               id="status"
                               value="1"
                               checked>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <button type="submit" class="btn btn-primary">
                        ثبت و ذخیره
                    </button>

                    <?php
                    if (isset($_SESSION['errors'])) {
                        unset($_SESSION['errors']);
                    }
                    ?>
                </div>
            </div>

        </form>
    </div>
</div>
```

### بررسی فرم ثبت

در این فرم دو فیلد داریم:

1. نام دسته‌بندی
2. وضعیت فعال یا غیرفعال بودن دسته‌بندی

فرم با متد POST اطلاعات را به آدرس زیر ارسال می‌کند:

```text
/categories/store
```

چرا از POST استفاده می‌کنیم؟

چون قرار است اطلاعات جدیدی در دیتابیس ثبت کنیم.

به طور معمول برای ارسال اطلاعاتی که باعث تغییر وضعیت دیتابیس می‌شوند از متد POST استفاده می‌کنیم.

همچنین در قسمت نام دسته‌بندی، بررسی کرده‌ایم که آیا خطایی در Session وجود دارد یا خیر:

```php
<?php if (isset($_SESSION['errors']['name'])): ?>
```

اگر کاربر نام دسته‌بندی را وارد نکرده باشد، پیام خطا در این قسمت نمایش داده می‌شود.

---

## ۳. ذخیره دسته‌بندی جدید

حالا باید اطلاعات ارسال‌شده از فرم را دریافت کرده و در دیتابیس ذخیره کنیم.

برای این کار از متد store در CategoryController استفاده می‌کنیم.

```php
public function store(array $inputs): void
{
    // validation
    $name = trim($inputs['name'] ?? '');

    if ($name === '') {
        $this->redirect('/categories/create', [
            'errors' => [
                'name' => 'نام دسته‌بندی را وارد کنید!'
            ]
        ]);
    }

    $inputs['name'] = $name;
    $inputs['created_at'] = date('Y-m-d H:i:s');
    $inputs['status'] = isset($inputs['status']) ? 1 : 0;

    $this->category->insert($inputs);

    $this->redirect('/categories', [
        'success' => 'دسته‌بندی با موفقیت ثبت شد.'
    ]);
}
```

### بررسی کد

**۱. دریافت اطلاعات فرم**

```php
public function store(array $inputs): void
```

اطلاعات ارسالی از فرم در قالب یک آرایه به متد store ارسال می‌شوند.

برای مثال، اگر کاربر اطلاعات زیر را وارد کرده باشد:

```text
name = آموزش PHP
status = 1
```

اطلاعات دریافتی می‌توانند به شکل زیر باشند:

```php
$inputs = [
    'name' => 'آموزش PHP',
    'status' => '1'
];
```

**۲. پاک‌سازی مقدار نام**

```php
$name = trim($inputs['name'] ?? '');
```

تابع trim فاصله‌های اضافی ابتدا و انتهای رشته را حذف می‌کند.

همچنین با استفاده از عملگر `??` مشخص کرده‌ایم که اگر مقدار name در آرایه وجود نداشت، مقدار خالی در نظر گرفته شود.

**۳. اعتبارسنجی اطلاعات**

```php
if ($name === '') {
    $this->redirect('/categories/create', [
        'errors' => [
            'name' => 'نام دسته‌بندی را وارد کنید!'
        ]
    ]);
}
```

در این قسمت بررسی می‌کنیم که آیا کاربر نام دسته‌بندی را وارد کرده است یا خیر.

اگر نام خالی باشد، کاربر به صفحه ثبت دسته‌بندی بازگردانده می‌شود و پیام خطا در Session قرار می‌گیرد.

**۴. تنظیم تاریخ ثبت**

```php
$inputs['created_at'] = date('Y-m-d H:i:s');
```

با استفاده از تابع date، تاریخ و ساعت فعلی را دریافت می‌کنیم.

فرمت مورد استفاده به شکل زیر است:

```text
2026-11-16 14:30:00
```

در اینجا تاریخ را در همان قالب استاندارد دیتابیس MySQL ذخیره می‌کنیم.

**۵. تنظیم وضعیت دسته‌بندی**

```php
$inputs['status'] = isset($inputs['status']) ? 1 : 0;
```

فیلد status یک Checkbox است.

نکته مهم این است که اگر Checkbox انتخاب نشده باشد، مرورگر اصلاً آن فیلد را در درخواست ارسال نمی‌کند.

بنابراین با استفاده از تابع isset بررسی می‌کنیم که آیا این فیلد ارسال شده است یا خیر.

اگر ارسال شده باشد مقدار آن را 1 و در غیر این صورت مقدار آن را 0 قرار می‌دهیم.

**۶. ذخیره اطلاعات در دیتابیس**

```php
$this->category->insert($inputs);
```

در این قسمت متد insert مدل Category را فراخوانی می‌کنیم.

این متد اطلاعات را در جدول categories ذخیره می‌کند.

**۷. انتقال کاربر به صفحه لیست**

```php
$this->redirect('/categories', [
    'success' => 'دسته‌بندی با موفقیت ثبت شد.'
]);
```

پس از ثبت موفق اطلاعات، کاربر به صفحه لیست دسته‌بندی‌ها منتقل می‌شود.

همچنین یک پیام موفقیت در Session قرار می‌دهیم تا در صفحه بعدی نمایش داده شود.

---

## ۴. نمایش فرم ویرایش دسته‌بندی

در مرحله بعد باید امکان ویرایش دسته‌بندی‌های ثبت‌شده را فراهم کنیم.

برای این کار ابتدا باید اطلاعات دسته‌بندی مورد نظر را از دیتابیس دریافت کرده و در فرم ویرایش نمایش دهیم.

متد edit در کنترلر:

```php
public function edit(int $id): void
{
    $query = $this->category->query(
        'SELECT * FROM categories WHERE id = ' . $id
    );

    $category = $query->fetch();

    if (!$category) {
        $this->redirect('/categories', [
            'error' => 'دسته بندی انتخاب شده پیدا نشد!'
        ]);

        return;
    }

    $this->view('admin.category.edit', [
        'category' => $category
    ]);
}
```

### بررسی کد

**۱. دریافت شناسه دسته‌بندی**

```php
public function edit(int $id): void
```

شناسه دسته‌بندی از طریق URL به متد edit ارسال می‌شود.

برای مثال:

```text
/categories/edit/5
```

در این درخواست مقدار `$id` برابر با 5 خواهد بود.

**۲. دریافت اطلاعات از دیتابیس**

```php
$query = $this->category->query(
    'SELECT * FROM categories WHERE id = ' . $id
);
```

با استفاده از متد query، اطلاعات دسته‌بندی مورد نظر را از دیتابیس دریافت می‌کنیم.

**۳. دریافت نتیجه کوئری**

```php
$category = $query->fetch();
```

متد fetch اولین رکورد حاصل از اجرای کوئری را برمی‌گرداند.

از آنجایی که در این کوئری با استفاده از شناسه، فقط یک دسته‌بندی را جستجو می‌کنیم، به یک رکورد نیاز داریم.

**۴. بررسی وجود دسته‌بندی**

```php
if (!$category) {
    $this->redirect('/categories', [
        'error' => 'دسته بندی انتخاب شده پیدا نشد!'
    ]);

    return;
}
```

ممکن است شناسه ارسال‌شده در دیتابیس وجود نداشته باشد.

در این شرایط کاربر را به صفحه لیست دسته‌بندی‌ها منتقل می‌کنیم.

**۵. ارسال اطلاعات به View**

```php
$this->view('admin.category.edit', [
    'category' => $category
]);
```

اطلاعات دسته‌بندی را به View مربوط به ویرایش ارسال می‌کنیم.

فایل View مورد نظر در مسیر زیر قرار دارد:

```text
app/Views/admin/category/edit.php
```

### ایجاد View ویرایش دسته‌بندی

محتوای فایل edit.php:

```php
<div class="card mt-5">
    <div class="card-header">
        <h1>ویرایش دسته بندی</h1>
    </div>

    <div class="card-body">
        <form action="<?= BASE_URL ?>/categories/update/<?= $category['id'] ?>"
              method="post">

            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label class="control-leabel" for="name">
                            نام
                        </label>

                        <input type="text"
                               name="name"
                               id="name"
                               value="<?= $category['name'] ?>"
                               class="form-control">

                        <?php if (isset($errors['name'])): ?>
                            <div class="form-text text-danger">
                                <?= $errors['name'] ?>
                            </div>
                        <?php endif; ?>
                    </div>
                </div>

                <div class="col">
                    <div class="form-group">
                        <label class="control-leabel" for="status">
                            وضعیت فعال بودن
                        </label>

                        <input type="checkbox"
                               class="form-control"
                               name="status"
                               id="status"
                               value="1"
                               <?= $category['status'] ? 'checked' : '' ?>>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <button type="submit" class="btn btn-warning">
                        به روزرسانی
                    </button>
                </div>
            </div>

        </form>
    </div>
</div>
```

### بررسی فرم ویرایش

تفاوت اصلی فرم ویرایش با فرم ثبت این است که اطلاعات قبلی دسته‌بندی را در فیلدهای فرم قرار می‌دهیم.

برای مثال:

```php
value="<?= $category['name'] ?>"
```

با این کار نام فعلی دسته‌بندی در فیلد نام نمایش داده می‌شود.

همچنین برای Checkbox وضعیت از دستور زیر استفاده کرده‌ایم:

```php
<?= $category['status'] ? 'checked' : '' ?>
```

اگر دسته‌بندی فعال باشد، Checkbox به صورت انتخاب‌شده نمایش داده می‌شود.

اگر غیرفعال باشد، Checkbox انتخاب نشده خواهد بود.

در نهایت فرم اطلاعات را به مسیر زیر ارسال می‌کند:

```text
/categories/update/{id}
```

---

## ۵. به‌روزرسانی دسته‌بندی

پس از نمایش فرم ویرایش، باید اطلاعات ارسال‌شده از فرم را دریافت کرده و در دیتابیس به‌روزرسانی کنیم.

برای این کار از متد update در کنترلر استفاده می‌کنیم:

```php
public function update(array $inputs, int $id): void
{
    $inputs['status'] = isset($inputs['status']) ? 1 : 0;

    $this->category->update($inputs, $id);

    $this->redirect('/categories', [
        'success' => 'دسته بندی با موفقیت به روزرسانی شد.'
    ]);
}
```

### بررسی کد

این متد دو ورودی دریافت می‌کند:

- `$inputs`: اطلاعات جدید ارسال‌شده از فرم
- `$id`: شناسه دسته‌بندی مورد نظر

ابتدا وضعیت دسته‌بندی را بررسی می‌کنیم:

```php
$inputs['status'] = isset($inputs['status']) ? 1 : 0;
```

سپس با استفاده از مدل Category اطلاعات را به‌روزرسانی می‌کنیم:

```php
$this->category->update($inputs, $id);
```

در این قسمت متد update مدل Category فراخوانی می‌شود.

این متد با استفاده از شناسه، رکورد مورد نظر را پیدا کرده و اطلاعات جدید را جایگزین اطلاعات قبلی می‌کند.

در نهایت کاربر را به صفحه لیست دسته‌بندی‌ها منتقل می‌کنیم:

```php
$this->redirect('/categories', [
    'success' => 'دسته بندی با موفقیت به روزرسانی شد.'
]);
```

---

## ۶. حذف دسته‌بندی

آخرین عملیات CRUD، حذف اطلاعات است.

برای این کار متد delete را در کنترلر تعریف کرده‌ایم:

```php
public function delete(int $id): void
{
    $this->category->delete($id);

    $this->redirect('/categories', [
        'success' => 'دسته بندی با موفقیت حذف شد.'
    ]);
}
```

در این متد شناسه دسته‌بندی از طریق URL دریافت می‌شود.

سپس متد delete مدل Category را فراخوانی می‌کنیم:

```php
$this->category->delete($id);
```

این متد رکورد مورد نظر را از جدول categories حذف می‌کند.

پس از حذف نیز کاربر به صفحه لیست دسته‌بندی‌ها منتقل می‌شود.

---

## تعریف مسیرهای CRUD

حالا که متدهای مربوط به عملیات CRUD را در کنترلر ایجاد کرده‌ایم، باید مسیرهای دسترسی به آن‌ها را در Router تعریف کنیم.

برای این کار فایل زیر را ویرایش می‌کنیم:

```text
routes/web.php
```

محتوای مربوط به مسیرهای دسته‌بندی:

```php
<?php

use App\Controllers\Admin\CategoryController;
use App\Controllers\HomeController;

return [
    'GET' => [
        '/' => [HomeController::class, 'index'],

        '/categories' => [CategoryController::class, 'index'],

        '/categories/create' => [CategoryController::class, 'create'],

        '/categories/edit/([0-9]+)' => [CategoryController::class, 'edit'],

        '/categories/delete/([0-9]+)' => [CategoryController::class, 'delete'],
    ],

    'POST' => [
        '/categories/store' => [CategoryController::class, 'store'],

        '/categories/update/([0-9]+)' => [CategoryController::class, 'update'],
    ]
];
```

### بررسی مسیرها

در این فایل مسیرها را بر اساس متد HTTP به دو گروه GET و POST تقسیم کرده‌ایم.

| آدرس | متد HTTP | متد کنترلر | کاربرد |
|---|---|---|---|
| `/categories` | GET | index | نمایش لیست |
| `/categories/create` | GET | create | نمایش فرم ثبت |
| `/categories/store` | POST | store | ذخیره دسته‌بندی |
| `/categories/edit/{id}` | GET | edit | نمایش فرم ویرایش |
| `/categories/update/{id}` | POST | update | به‌روزرسانی اطلاعات |
| `/categories/delete/{id}` | GET | delete | حذف دسته‌بندی |

### آشنایی با عبارت منظم در مسیرها

در بعضی از مسیرها از عبارت زیر استفاده کرده‌ایم:

```php
([0-9]+)
```

این عبارت منظم (Regular Expression) برای دریافت اعداد استفاده می‌شود.

برای مثال، مسیر زیر:

```php
'/categories/edit/([0-9]+)'
```

می‌تواند آدرس‌های زیر را پوشش دهد:

```text
/categories/edit/1
/categories/edit/15
/categories/edit/250
```

Router مقدار عددی موجود در URL را استخراج کرده و به عنوان آرگومان به متد کنترلر ارسال می‌کند.

برای مثال در درخواست زیر:

```text
/categories/edit/15
```

متد زیر اجرا خواهد شد:

```php
CategoryController::edit(15);
```

نکته مهم این است که Router باید آرگومان‌های استخراج‌شده از الگوی مسیر را به متد کنترلر ارسال کند.

همچنین در مسیرهای POST، ابتدا اطلاعات فرم و سپس شناسه مسیر به متد کنترلر ارسال می‌شوند.

به همین دلیل امضای متد update به صورت زیر است:

```php
public function update(array $inputs, int $id): void
```

---

## افزودن متد Redirect به Controller

در عملیات CRUD، پس از انجام بعضی از درخواست‌ها لازم است کاربر را به صفحه دیگری منتقل کنیم.

برای مثال:

- پس از ثبت دسته‌بندی، کاربر به لیست دسته‌بندی‌ها منتقل شود.
- پس از ویرایش، کاربر به لیست دسته‌بندی‌ها منتقل شود.
- در صورت بروز خطا، کاربر به فرم مربوطه بازگردانده شود.

برای جلوگیری از تکرار کد، یک متد عمومی برای تغییر مسیر در کلاس Controller ایجاد می‌کنیم.

فایل زیر را باز کنید:

```text
core/Libraries/Controller.php
```

متد زیر را به کلاس اضافه کنید:

```php
protected function redirect(string $url, array $message = []): void
{
    if ($message) {
        $key = key($message);
        $_SESSION[$key] = $message[$key];
    }

    header('Location:' . BASE_URL . $url);

    return;
}
```

### بررسی متد Redirect

**۱. دریافت آدرس مقصد**

```php
string $url
```

این پارامتر آدرسی را مشخص می‌کند که قصد داریم کاربر را به آن منتقل کنیم.

برای مثال:

```php
$this->redirect('/categories');
```

**۲. دریافت پیام**

```php
array $message = []
```

این پارامتر یک آرایه اختیاری است که می‌توانیم پیام‌های مورد نیاز را در آن قرار دهیم.

برای مثال:

```php
$this->redirect('/categories', [
    'success' => 'عملیات با موفقیت انجام شد.'
]);
```

**۳. ذخیره پیام در Session**

```php
if ($message) {
    $key = key($message);
    $_SESSION[$key] = $message[$key];
}
```

در این قسمت کلید و مقدار آرایه را دریافت کرده و در Session ذخیره می‌کنیم.

برای مثال، اگر آرایه زیر را ارسال کنیم:

```php
[
    'success' => 'عملیات با موفقیت انجام شد.'
]
```

مقدار آن در متغیر زیر ذخیره خواهد شد:

```php
$_SESSION['success']
```

**۴. انتقال کاربر**

```php
header('Location:' . BASE_URL . $url);
```

تابع header در PHP برای ارسال Headerهای HTTP استفاده می‌شود.

با استفاده از هدر Location می‌توانیم مرورگر کاربر را به آدرس دیگری هدایت کنیم.

در این مثال BASE_URL نیز به آدرس پایه پروژه اضافه می‌شود.

> نکته مهم: برای اینکه پیام‌های Session در صفحه بعدی در دسترس باشند، باید session_start قبلاً در برنامه اجرا شده باشد. همچنین بهتر است پس از ارسال هدر انتقال، از `exit;` استفاده کنیم تا اجرای کد متوقف شود.

---

## تنظیم منطقه زمانی برنامه

یکی دیگر از موارد مهم در پروژه‌ها، تنظیم منطقه زمانی (Timezone) است.

در PHP توابعی مانند date برای دریافت تاریخ و ساعت از تنظیمات منطقه زمانی برنامه استفاده می‌کنند.

از آنجایی که قصد داریم تاریخ‌های برنامه بر اساس ساعت ایران ثبت شوند، منطقه زمانی را روی تهران تنظیم می‌کنیم.

برای این کار فایل bootstrap.php را باز کرده و دستور زیر را در ابتدای راه‌اندازی برنامه قرار می‌دهیم:

```php
date_default_timezone_set('Asia/Tehran');
```

با اجرای این دستور، توابع تاریخ و زمان PHP از منطقه زمانی تهران استفاده خواهند کرد.

برای مثال:

```php
echo date('Y-m-d H:i:s');
```

تاریخ و ساعت را بر اساس ساعت تهران نمایش می‌دهد.

این موضوع در ثبت تاریخ ایجاد رکوردها بسیار مهم است.

در پروژه ما هنگام ثبت دسته‌بندی از دستور زیر استفاده کردیم:

```php
$inputs['created_at'] = date('Y-m-d H:i:s');
```

بنابراین زمان ثبت دسته‌بندی بر اساس منطقه زمانی تهران ذخیره می‌شود.

---

## بررسی جریان اجرای برنامه

حالا که تمام قسمت‌های مورد نیاز را پیاده‌سازی کرده‌ایم، بهتر است جریان اجرای برنامه را بررسی کنیم.

### سناریوی اول: مشاهده دسته‌بندی‌ها

کاربر آدرس زیر را باز می‌کند:

```text
/categories
```

مراحل اجرا:

1. درخواست توسط Router دریافت می‌شود.
2. Router متد index از CategoryController را اجرا می‌کند.
3. کنترلر با استفاده از مدل Category اطلاعات دسته‌بندی‌ها را دریافت می‌کند.
4. اطلاعات به View ارسال می‌شوند.
5. View اطلاعات را در قالب جدول HTML نمایش می‌دهد.

### سناریوی دوم: ثبت دسته‌بندی جدید

کاربر روی دکمه ثبت دسته‌بندی کلیک می‌کند.

مراحل اجرا:

1. درخواست GET به مسیر `/categories/create` ارسال می‌شود.
2. متد create در کنترلر اجرا می‌شود.
3. فرم ثبت دسته‌بندی نمایش داده می‌شود.
4. کاربر اطلاعات را وارد کرده و فرم را ارسال می‌کند.
5. درخواست POST به مسیر `/categories/store` ارسال می‌شود.
6. متد store اطلاعات را دریافت می‌کند.
7. اطلاعات اعتبارسنجی می‌شوند.
8. مدل Category عملیات ذخیره‌سازی را انجام می‌دهد.
9. کاربر به صفحه لیست دسته‌بندی‌ها منتقل می‌شود.
10. پیام موفقیت نمایش داده می‌شود.

### سناریوی سوم: ویرایش دسته‌بندی

1. کاربر روی دکمه ویرایش یک دسته‌بندی کلیک می‌کند.
2. درخواست به مسیر `/categories/edit/{id}` ارسال می‌شود.
3. متد edit شناسه دسته‌بندی را دریافت می‌کند.
4. اطلاعات دسته‌بندی از دیتابیس خوانده می‌شوند.
5. اطلاعات به فرم ویرایش ارسال می‌شوند.
6. کاربر تغییرات را اعمال می‌کند.
7. فرم با متد POST به مسیر `/categories/update/{id}` ارسال می‌شود.
8. متد update کنترلر اجرا می‌شود.
9. مدل Category اطلاعات را در دیتابیس به‌روزرسانی می‌کند.
10. کاربر به لیست دسته‌بندی‌ها منتقل می‌شود.

### سناریوی چهارم: حذف دسته‌بندی

1. کاربر روی دکمه حذف کلیک می‌کند.
2. شناسه دسته‌بندی به مسیر حذف ارسال می‌شود.
3. متد delete در کنترلر اجرا می‌شود.
4. مدل Category رکورد مورد نظر را حذف می‌کند.
5. کاربر به صفحه لیست دسته‌بندی‌ها منتقل می‌شود.

---

## نکات مهم امنیتی و بهبود کد

در این مرحله یک CRUD ساده و کاربردی ساخته‌ایم؛ اما در پروژه‌های واقعی باید چند نکته مهم را نیز رعایت کنیم.

### ۱. استفاده از Prepared Statement

در متد edit از کد زیر استفاده کرده‌ایم:

```php
$query = $this->category->query(
    'SELECT * FROM categories WHERE id = ' . $id
);
```

بهتر است برای اجرای کوئری‌هایی که اطلاعات ورودی دریافت می‌کنند از Prepared Statement و پارامترهای متصل‌شده استفاده کنیم.

برای مثال:

```php
$stmt = $this->db->prepare(
    'SELECT * FROM categories WHERE id = :id'
);

$stmt->execute([
    'id' => $id
]);

$category = $stmt->fetch();
```

این روش از اتصال مستقیم ورودی به رشته SQL جلوگیری می‌کند و امنیت کوئری را افزایش می‌دهد.

همین موضوع در متدهای update و delete مدل نیز اهمیت دارد. شناسه باید به شکل امن به کوئری متصل شود، نه اینکه مستقیماً داخل رشته SQL قرار بگیرد.

### ۲. اعتبارسنجی اطلاعات در عملیات ویرایش

در متد store نام دسته‌بندی را بررسی کرده‌ایم؛ اما در متد update نیز باید همین اعتبارسنجی انجام شود.

نباید اجازه دهیم کاربر هنگام ویرایش، نام دسته‌بندی را خالی ارسال کند.

### ۳. جلوگیری از حذف ناخواسته

در پیاده‌سازی فعلی، حذف با یک لینک GET انجام می‌شود.

در پروژه واقعی بهتر است عملیات حذف از طریق فرم POST و همراه با محافظت CSRF انجام شود.

همچنین بهتر است قبل از حذف، از کاربر تأیید بگیریم.

### ۴. جلوگیری از حملات XSS

در فایل‌های View اطلاعات دیتابیس را مستقیماً نمایش داده‌ایم.

برای مثال:

```php
<?= $category['name'] ?>
```

اگر اطلاعات ذخیره‌شده شامل کد HTML یا JavaScript مخرب باشند، ممکن است در مرورگر اجرا شوند.

بهتر است هنگام نمایش اطلاعات از تابع htmlspecialchars استفاده کنیم:

```php
<?= htmlspecialchars($category['name'], ENT_QUOTES, 'UTF-8') ?>
```

این کار باعث می‌شود کاراکترهای خاص به شکل امن نمایش داده شوند.

### ۵. توقف اجرای برنامه پس از Redirect

در متد redirect بهتر است بعد از ارسال هدر Location از دستور exit استفاده کنیم:

```php
header('Location: ' . BASE_URL . $url);
exit;
```

با این کار پس از هدایت کاربر، اجرای کدهای بعدی متوقف می‌شود.

---

## جمع‌بندی

در این جلسه توانستیم با استفاده از معماری MVC یک بخش کامل برای مدیریت دسته‌بندی‌ها ایجاد کنیم.

مهم‌ترین مواردی که در این جلسه یاد گرفتیم:

- ایجاد کلاس انتزاعی Model
- استفاده از مفهوم ارث‌بری در مدل‌ها
- ارتباط مدل‌ها با کلاس Database
- پیاده‌سازی عملیات CRUD در مدل Category
- ایجاد کنترلر CategoryController
- ارسال اطلاعات از Controller به View
- دریافت اطلاعات فرم‌ها
- استفاده از Session برای نگهداری پیام‌های خطا و موفقیت
- استفاده از متد Redirect برای انتقال کاربر
- تعریف مسیرهای GET و POST در Router
- تنظیم منطقه زمانی برنامه در PHP

اکنون ساختار برنامه به شکلی است که می‌توانیم موجودیت‌های دیگری مانند Post و User را نیز به آن اضافه کنیم.

برای این کار کافی است مدل، کنترلر، Viewها و مسیرهای مربوط به موجودیت جدید را ایجاد کنیم و از ساختار مشترکی که در این جلسه پیاده‌سازی کردیم استفاده کنیم.

**تمرین پیشنهادی:**

یک بخش مدیریت اخبار (Posts) به برنامه اضافه کنید.

امکانات مورد نیاز:

1. ثبت خبر با عنوان و متن خبر
2. نمایش لیست اخبار
3. ویرایش اطلاعات خبر
4. حذف خبر
5. تعیین وضعیت فعال یا غیرفعال بودن خبر
6. انتخاب دسته‌بندی برای هر خبر

در این تمرین تلاش کنید از کلاس Model و ساختار Controller که در این جلسه ایجاد کردیم مجدداً استفاده کنید.


---
[صفحه قبلی: notification  ➡️](25-notification.md)
|
[فهرست مطالب](README.md)
|
[صفحه بعدی: multi-layout  ➡️](27-multi-layout.md)