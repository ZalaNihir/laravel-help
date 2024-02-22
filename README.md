## Laravel custom Helpers
- First have to register helper file in composer:
- Your composer.json looks like this.

```composer
"autoload": {
    "classmap": [
        ...
    ],
    "psr-4": {
        "App\\": "app/"
    },
    "files": [
        "app/helpers.php" // <---- ADD THIS
    ]
},
```

- Next step is run the ```composer dump-autoload``` command.
- Here you registered your helpers file and you can use it globally in project.

## Laravel custom request
- First you have to fire following command in project terminal for registration.

```
php artisan make:request DemoRequest
```

- It will create the ```DemoRequest.php``` File following path: ```App\Http\Requests```
- Your all request file will be Here.
- For example you have to validate your user storing method in ```DemoRequest.php``` than you have to put your validation logic in ```rules()``` method.
- You must have to ```true``` your ```authorize()``` method otherwise it won't working.
```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class DemoRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users,email,' . $this->user,
            'address' => 'required',
            'city' => 'required',
            'country_id' => 'required',
            'phone' => 'required',
            'roles.*' => 'required',
        ];
    }
}

```

- You have to use this in your controller and just put it with request like this.

```php
use App\Http\Requests\DemoRequest;

public function store(DemoRequest $request)
{
    $request->validated();
    return $request->input();
}
```

## Laravel custom providers

- First thing first to create the custom provider you have to run following command, in my case I created the ```NavigationServiceProvider``` for my self so the command is looks like this.

```
php artisan make:provider NavigationServiceProvider
```

- Your provider file is created in ```\App\Http\Providers\``` directory.
- There is 2 method in the providers ```1) boot()``` and ```2) register()``` the convenient way to put logic in ```boot()``` method cause the boot method call with load project and the ```register()``` method is called after the project is loaded which is not convenient.
- The second step is that to register your provider in ```config\app.php``` in providers array.
- Now put you custom logic in ```boot()``` method.
- ```My boot method```

```php
public function boot()
{
    view()->composer('*',function($view){ // $view is for globally in project and  '*' after the composer this said for all URLs.
        $pages = Page::all(); // Here I fetch the all my custom nav links from database.
        return $view->with('pages', $pages);
    });
}
```
- Now the final step is using you can use it where you want in my case I used it globally for that i used in my ```nav.blade.php```
- And my blade file looks like this.
```php
@foreach($pages as $page)
    <a href="/{{ $page->slug }}">{{ $page->title }}</a>
@endforeach
```

## Laravel Mailable class
- After the successfully installation you have to configure the ```.env``` with mailable variables.
- I'm using mailtrap for testing so there is .env file's variables looks like this.

```
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=mailtrapusername
MAIL_PASSWORD=mailtrappassword
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"
```
- For creating mailable class with markdown you have to run following command

```
php artisan make:mail TestMail --markdown=emails.testmail
```
- In ```TestMail.php``` which is in ```App\Mail``` directory.
- There is 2 methods ```1)__construct()``` and ```2)build()```.
- So I put the recipient mail in ```build()``` method and its looks like this.

```php
public function build()
{
    return $this->from('testnihir@gmail.com') // here put your recipient's mail.
                ->markdown('emails.testmail');
}
```
- After all this thing you have to put your logic in controller and simply call your Mailable class it will work fine.

## Laravel custom repositories

- reference: 

```
https://www.youtube.com/watch?v=6xzGBH2jgOY
```

## Laravel custom resource
- reference:
```
https://laravel.com/docs/9.x/eloquent-resources
```
- To generate a resource class, you may use the ```make:resource``` Artisan command. By default, resources will be placed in the ```app/Http/Resources``` directory of your application. Resources extend the ```Illuminate\Http\Resources\Json\JsonResource``` class:

```
php artisan make:resource UserResource
```
     
## Laravel Authentication

- <a href="https://laravel.com/docs/10.x/starter-kits" target="_blank">Breeze</a>

- <a href="https://jetstream.laravel.com/3.x/introduction.html" target="_blank">Jetstream</a>

- <a href="https://github.com/laravel/ui" target="_blank">Ui/Auth</a>

## Laravel Roles Permission

- <a href="https://spatie.be/docs/laravel-permission/v5/introduction" target="_blank">Spatie</a>

## Laravel Admin Packges

- <a href="https://filamentphp.com/" target="_blank">Filament</a>

- <a href="https://backpackforlaravel.com/" target="_blank">Backpack</a>

- <a href="https://orchid.software/en/" target="_blank">Orchid</a>

- <a href="https://nova.laravel.com/" target="_blank">Nova</a>

- <a href="https://infyom.com/open-source" target="_blank">InfyOm</a>

- <a href="https://voyager.devdojo.com/" target="_blank">Voyager</a>

## Laravel DomPDF

- <a href="https://github.com/barryvdh/laravel-dompdf" target="_blank">Barry VDH</a>

## Laravel Sluggable

- <a href="https://github.com/cviebrock/eloquent-sluggable" target="_blank">CIVIBRock Sulg</a>

## Toastr

- <a href="https://php-flasher.io/library/toastr/" target="_blank">PHP Flasher</a>
