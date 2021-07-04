# Laravel Cheat Sheet

## Custom pagination URL
By default, links generated by the paginator will match the current request's
URI. However, the paginator's `withPath` method allows you to customize the URI
used by the paginator when generating links. For example, if you want the
paginator to generate links like `http://example.com/admin/users?page=N`, you
should pass `/admin/users` to the `withPath` method:
<br>
```
$users = User::paginate(15);
$users->withPath('/admin/users');
```

## Append accessors to Eloquent results
Occasionally, when converting models to arrays or JSON, you may wish to add attributes that do not
have a corresponding column in your database. To do so, first define an accessor for the value.
<br><br>
After creating the accessor, add the attribute name to the appends property of your model.
<br>
```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The accessors to append to the model's array form.
     *
     * @var array
     */
    protected $appends = ['is_admin'];

    /**
     * Determine if the user is an administrator.
     *
     * @return bool
     */
    public function getIsAdminAttribute()
    {
        return $this->attributes['admin'] === 'yes';
    }
}
```

## Running external scripts
```
use Symfony\Component\Process\Exception\ProcessFailedException;
use Symfony\Component\Process\Process;

$process = new Process('python3 /path/to/script <additional arguments here>');
$process->run();

// executes after the command finishes
if (!$process->isSuccessful()) {
    throw new ProcessFailedException($process);
}

echo $process->getOutput();
```


## Cache functions
```
Cache::has('key'); # does key exist
Cache::get(); # get the value of key
Cache::pull('key'); # get and delete
Cache::put('key',$amount, $minutes); # add/update
Cache::add('key', 'value', $minutes); # Store If Not Present

Cache::increment('key', $amount=1); # increase by $amount
Cache::decrement('key', $amount=1); # decrease by $amount

Cache::remember('users', $minutes, $default); # Retrieve & Store

Cache::forever('key', 'value'); # perm store
Cache::forget('key'); # remove perm
Cache::flush(); # remove everything
```

## Run artisan command as daemon
```
$ php artisan command:name > /dev/null &
```