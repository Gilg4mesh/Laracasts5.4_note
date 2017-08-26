# Laracasts5.4_note

# Install composer

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

Globally

`mv composer.phar /usr/local/bin/composer`

Finding packages

<https://packagist.org/>

Add to path

`export PATH=$HOME/.composer/vendor/bin:$PATH`

# Routs & Views

web.php & views & controllers

sublime using ctrl+p typing 

rweb to web.php

v/ to view folder

# Database Setup

`php artisan migrate `

add `--seed` if using seeder

# Pass Data to Your Views

in web.php :

```
Route::get('/', function () {
    $name = 'Tom';
    $age = '24';
    
    $arr = [
      '1',
      '2',
      '3'
    ];
    return view('welcome', compact(name, age, arr));
});
```

in view :

```
{{ $name }}

{{ $age }}

@foreach ($i as $arr)
  <li>{{ $arr }}</li>
@endforeach

```

# Working With the Query Builder

creating migration `php artisan:migration creat_name_table --creat=yourtablename` 

use `composer dump-autoload` if having error

migrate the database using `php artisan migrate`

in web.php :

```
Route::get('/', function () {
    $arr = DB::table('yourtablename')->where('created_at', '>=')->get()
    $arr = DB::table('yourtablename')->latest()->get()
    $arr = DB::table('yourtablename')->get()
    return view('welcome', compact(arr));
});
```

or for specific user :

```
Route::get('/user/{id}', function ($id) {
    $arr = DB::table('yourtablename')->find($id)
    // dd($arr); // die & dump fordebug
    
    return view('welcome', compact(arr));
});
```

in view / :

```
<li>{{ $arr->colname }}</li>
```

or choosing specific user and direct to view /user :

```
@foreach ($arr as $i)
  <li>
    <a href="/user/{{ $i->id }}">
      {{ $i->colname }}
    </a>
  </li>
@endforeach

// directed

<li>{{ $arr->colname }}</li>
```

# Eloquent 

make model with migration `php artisan make:model Task -m`

and the Task model will exists in app file

you can modify the database by `php artisan tinker`

adding elements

```
$task = new App\Task;
$task->body = "123";
$task->save();
```

simple query

`App\Task::where('completed', 0)->get();`

in Task model you can have a static function under the class

```
public static function incomplete()
{
  return static::where('completed', 0)->get();
}
```

and use `App\Task::incomplete();` in query to have same result

even more

make a scope to have the query as OOP

```
public function scopeIncomplete($query) // you can have more args if needed
{
  return where('completed', 0);
}
```

and use `App\Task::incomplete()->get();` in query to have same result

or  `App\Task::incomplete()->where('id', '>=', 2)->get();` with more

note : You do not need to include the scope prefix when calling the method. You can even chain calls to various scopes, for example:`App\User::popular()->active()->orderBy('created_at')->get();`

# Controllers

make controller

`php artisan make:controller TasksController`

in TasksController.php :

```
public function index()
{
  tasks = Task::all();
  return view ('tasks.index', compact('tasks'));
}
```
in web.php

```
Route::get('/tasks', 'TasksController@index');
```

# Route Model Binding

in TasksController.php :

```
public function show($id)
{
  tasks = Task::find($id);
  return view ('tasks.show', compact('task'));
}
```
in web.php

```
Route::get('/tasks/{task}', 'TasksController@show');
```

can be written in this case :

in TasksController.php :

```
public function show(Task $task)
{
  return view ('tasks.show', compact('task'));
}
```
in web.php

```
Route::get('/tasks/{task}', 'TasksController@show');
```

but make sure the wildcard in Route are the same with the parameter in function show

# Layouts and Structure

simple things about blade structure

```
@extends('layouts.master')
// extending from another blade.php

@yield('content')
// given a yield for others to replace

@section('content')
@endsection
// replace the yield by section
```
# Form Request Data and CSRF

posts

```
GET /posts
GET /posts/create
POST /posts
GET /posts/{id}/edit
GET /posts/{id}
PATCH /posts/{id}
DELETE /posts/{id}
```







