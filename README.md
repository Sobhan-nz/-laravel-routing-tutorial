# 📌 آموزش کامل Routing در Laravel

## 1. تعریف Routing در لاراول
در لاراول، **Route (مسیر)** مسئول تعیین اینه که وقتی یک کاربر به یک آدرس خاص (URL) درخواست میده، چه اتفاقی بیفته.  

> Routing پل ارتباطی بین **URL** که کاربر درخواست کرده و **منطق برنامه** (Controller, View, Closure Function) است.

**مثال:**
```
کاربر → مرورگر → /about → Route → Controller یا View → پاسخ به کاربر
```

---

## 2. محل تعریف Route‌ها
فایل‌های اصلی مسیرها در پوشه `routes/`:
- `web.php` → مخصوص درخواست‌های وب (دارای Session و CSRF)
- `api.php` → مخصوص API‌ها (بدون Session)
- `console.php` → مسیرهای کنسول (Artisan Commands)
- `channels.php` → مسیرهای کانال برای Broadcast

---

## 3. انواع تعریف Route
### 3.1 مسیر ساده با Closure
```php
Route::get('/about', function () {
    return 'صفحه درباره ما';
});
```

### 3.2 مسیر به یک View
```php
Route::get('/contact', function () {
    return view('contact');
});
```

### 3.3 مسیر به یک Controller
```php
Route::get('/products', [ProductController::class, 'index']);
```

---

## 4. متدهای HTTP در Route
- **GET** → دریافت داده  
- **POST** → ارسال داده  
- **PUT/PATCH** → بروزرسانی داده  
- **DELETE** → حذف داده  
- **ANY** → هر متدی  
- **MATCH** → چند متد خاص  

مثال:
```php
Route::post('/form', [FormController::class, 'store']);
Route::delete('/product/{id}', [ProductController::class, 'destroy']);
```

---

## 5. پارامتر در Route
### 5.1 اجباری
```php
Route::get('/user/{id}', function ($id) {
    return "کاربر شماره: $id";
});
```

### 5.2 اختیاری
```php
Route::get('/user/{name?}', function ($name = 'Guest') {
    return "سلام $name";
});
```

### 5.3 اعتبارسنجی با Regex
```php
Route::get('/product/{id}', function ($id) {
    return "محصول شماره $id";
})->where('id', '[0-9]+');
```

---

## 6. نام‌گذاری Route
```php
Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');
```
در Blade:
```blade
<a href="{{ route('dashboard') }}">داشبورد</a>
```

---

## 7. گروه‌بندی Route‌ها
### 7.1 با Prefix
```php
Route::prefix('admin')->group(function () {
    Route::get('/users', [AdminController::class, 'users']);
    Route::get('/posts', [AdminController::class, 'posts']);
});
```

### 7.2 با Middleware
```php
Route::middleware(['auth'])->group(function () {
    Route::get('/profile', [UserController::class, 'profile']);
    Route::get('/settings', [UserController::class, 'settings']);
});
```

---

## 8. Route Resource (CRUD سریع)
```php
Route::resource('products', ProductController::class);
```
مسیرهای زیر ساخته می‌شوند:
- index  
- create  
- store  
- show  
- edit  
- update  
- destroy  

---

## 9. Route Fallback (صفحه 404)
```php
Route::fallback(function () {
    return view('errors.404');
});
```

---

## 10. کش کردن Route‌ها
```bash
php artisan route:cache
php artisan route:clear
```

---

## 📚 نتیجه‌گیری
Routing یکی از مهم‌ترین بخش‌های لاراول است که مدیریت درخواست‌ها و پاسخ‌ها را بر عهده دارد. با استفاده درست از Route‌ها می‌توان یک ساختار تمیز، امن و قابل نگهداری ایجاد کرد.
