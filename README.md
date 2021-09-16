# Laravel Cheatsheet:



# References:


1. Net Ninja Laravel Tutotial Play list:  
https://www.youtube.com/watch?v=zckH4xalOns&list=PL4cUxeGkcC9hL6aCFKyagrT1RCfVN4w2Q&index=1

2. Laravel Documentation: https://laravel.com/docs/


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



## 2-b) Render Template blade.php files:


Create this file:  

<b>

`resources/views/products_list.blade.php`


```html
<!DOCTYPE html>
<html>
<head>
	<title>Products List</title>
</head>
<body>

<h1>Products list</h1>

<ol>
	<li>Products 1</li>
	<li>Products 2</li>
	<li>Products 3</li>
</ol>


</body>
</html>
```





`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/products/', function () {
    return view("products_list");
});
```


</b>







# 3) Blade Syntax:


## 3-a) Passing Variables:



<b>

`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/products/', function () {
    $page_inputs = 
    [
    	"title"=>"Products List",
    	"heading"=>"All Products"
    ];
    return view("products_list",$page_inputs);
});
```





`resources/views/products_list.blade.php`


```html
<head>
	<title>{{ $title }}</title>
</head>
...
<h1>{{ $heading }}:</h1>
```

</b>







# 4) Using PHP in php files:

You can just use PHP like normal php files.



<b>

`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/products/', function () {
    $page_inputs = 
    [
    	"title"=>"Products List",
    	"heading"=>"All Products"
    ];
    return view("products_list",$page_inputs);
});
```





`resources/views/products_list.blade.php`


```php
<h1><?php echo $heading; ?>:</h1>
```

This is equivalent to:

```php
<h1>{{ echo $heading; }}:</h1>
```

</b>


























