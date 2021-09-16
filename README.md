# Laravel Cheatsheet:


# 1) Important Commands:

<b>

```bash
# Make sure that requirements are installed
composer --version
php --version

# Create Laravel Project
composer create-project laravel/laravel <application name>
cd <application name>
php artisan serve
```

</b>





# 2) Routing:

## 2-a) Return Text or JSON:

<b>

`routes/web.php`

```php
// Return String Response
Route::get('/hi', function () {
    return "Wassup!";
});

// Return json Response
Route::get('/hi/json', function () {
    return ["message"=>"Wassup"];
});
```

</b>



## 2-b) Render blade.php files:


Create this file:  

<b>

`resources/views/hello_world.blade.php`


```html
<!DOCTYPE html>
<html>
<head>
	<title>Hello, World!</title>
</head>
<body>

<h1>Hello. World!</h1>

<p>
Hey!<br>
Wassup?!
</p>

</body>
</html>
```





`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/hello/', function () {
    return view("hello_world");
});
```




</b>

