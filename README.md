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

# Controllers
php artisan make:controller <Controller Name> # in "app/HTTP/Controllers"

# Migrations
php artisan make:migration create_<table_name>_table # in "database/migrations"
php artisan make:migration add_<field_name>_to_<table_name>_table # in "database/migrations"

php artisan migrate # migrate the new migrations
php artisan migrate:fresh        #Drop all tables and re-run all migrations
php artisan migrate:refresh      #Reset and re-run all migrations
php artisan migrate:reset        #Rollback all database migrations
php artisan migrate:rollback     #Rollback the last database migration
php artisan migrate:status       #Show the status of each migration
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







# 6) Inputs:

## 6-a) Query Prameters:

<b>

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    $name = $request->query("name", 'Unknown');
    # "Unknown" is a fallback value
    error_log($name);
    ...
});
```


http://127.0.0.1:8000/products/?name=Ninja  

Logs: "Ninja"  

http://127.0.0.1:8000/products/ 

Logs: "Unknown"



</b>





## 6-b) URL Parameters:




<b>

```php
use Illuminate\Http\Request;

Route::get('/products/{id}', function ($id) {
    # Now the id is passed
    # We can use it inside the route
    ...
});
```

</b>












# 7) Controllers:

<b>

```bash
php artisan make:controller <Controller Name> # in "app/HTTP/Controllers"
```


```bash
php artisan make:controller ProductsController # in "app/HTTP/Controllers"
```


`app/HTTP/Controllers/ProductsController`
```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ProductsController extends Controller
{
    # This is now called index action
    # Index is just a naming convention
    public function index(Request $request)
    {return view("products_list");}

    # show is also a naming convention
    # used for displaying specific item with id
    public function show($id)
    {return view("products_details",["id"=>$id]);}
}
```






`routes/web.php`

```php
use Illuminate\Support\Facades\Route;
use Illuminate\Http\Request;
use App\Http\Controllers\ProductsController;

Route::get('/products/', [ProductsController::class, 'index']);
Route::get('/products/{id}', [ProductsController::class, 'show']);
```


</b>















# 8) Connecting to MySQL:

- Open Xampp
- Turn on `Apache` and `MySQL`
- Go to this link : http://localhost/phpmyadmin/
- Create a database, let's call it `products_db`
- Inside **`.env`** modify these environemnt variables:

<b>

```
DB_DATABASE=products_db
DB_USERNAME=root
DB_PASSWORD=
```
</b>







# 9) Migrations:

Documentation: https://laravel.com/docs/8.x/migrations  
Column Types: https://laravel.com/docs/8.x/migrations#available-column-types  

<b>


```bash
php artisan make_migration create_<table_name>_table
php artisan make_migration create_products_table
```


Now, This is created:  

`databse/migrations/create_products_table.php`

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateProductsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('products');
    }
}

```




I have used this schema:


```php
$table->id();
$table->timestamps();
$table->string('name',255);
$table->string('description',2000);
#$table->float('price');
```




Now run this command:


```bash
php artisan migrate # migrate the new migrations
php artisan migrate:fresh # drops all the databses and migrate from scratch
```


```bash
php artisan migrate:fresh        #Drop all tables and re-run all migrations
php artisan migrate:refresh      #Reset and re-run all migrations
php artisan migrate:reset        #Rollback all database migrations
php artisan migrate:rollback     #Rollback the last database migration
php artisan migrate:status       #Show the status of each migration
```



```bash
php artisan make:migration add_<field_name>_to_<table_name>_table # in "database/migrations

php artisan make:migration add_price_to_products_table # in "database/migrations
```


`databse/migrations/add_price_to_products_table.php`

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddPriceToProductsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('products', function (Blueprint $table) {
            $table->float('price');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('products', function (Blueprint $table) {
            $table->dropColumn('price');
        });
    }
}
```



</b>
























