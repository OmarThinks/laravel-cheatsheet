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

<b>





# 2) Routing:



<b>

routes/web.php

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


