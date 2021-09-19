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




```bash
error_log("Hello, World!"); #To print to the console
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

<b>


## 3-a) Using PHP:


`resources/views/products_list.blade.php`
```php
<p>
@php
	echo "This is php using Blade";
@endphp
</p>
```




## 3-b) Passing Variables:




`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/products/', function () {
    $page_inputs = 
    [
    	"title"=>"Products List",
    	"heading"=>"All Products",
    	"just_a_number"=>1
    ];
    return view("products_list",$page_inputs);
});
```





`resources/views/products_list.blade.php`


```php
<head>
	<title>{{ $title }}</title>
</head>
...
<h1>{{ $heading }}:</h1>
```



## 3-c) Conditionals:




`resources/views/products_list.blade.php`


```php
@if($just_a_number>1)
	<p>Blade just_a_number is more than 1</p>
@elseif($just_a_number<1)
	<p>Blade just_a_number is less than 1</p>
@else
	<p>Blade just_a_number is equal to 1</p>
@endif
```







## 3-d) Loops:





`routes/web.php`

```php
// Render Blade PHP Response
Route::get('/products/', function () {
    $products =[
		["name"=>"Labtop","price"=>"50"],
		["name"=>"Mobile","price"=>"20"],
		["name"=>"Desktop","price"=>"100"],
		["name"=>"Table","price"=>"30"],
	];
    $page_inputs = 
    [
    	"products"=>$products
    	...
    ];
    return view("products_list",$page_inputs);
});
```






`resources/views/products_list.blade.php`


```php
@foreach($products as $key=>$product)

<tr> <th>{{ $key }}</th> 
	<td>{{ $product["name"] }} </td> 
<td>${{$product["price"]}}</td>

@endforeach
```























## 3-e) Layouts:








`resources/views/layouts/layout.blade.php`


```php
<!DOCTYPE html>
<html>
<head>
    <title>{{ $title }}</title>
</head>
<body>

@yield("content")

</body>
</html>
```




`resources/views/home.blade.php`


```php
@extends("layouts.layout")

@section("content")

<p>This is Home</p>

@endsection
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










# 5) Public Resources:


**Public Resources** exist on the **root url**.


<b>


`public/css/main.css`


```css
table, th, td{
    border-collapse: collapse;
    border:solid 1px black;
    padding: 3px;
}
```





`resources/views/layouts/layout.blade.php`


```php
<link rel="stylesheet" type="text/css" href="/css/main.css">
```




</b>












