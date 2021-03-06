## Laravel Time Traveller
Time travelling for Laravel 5 models.
- Uses database table to store model states.
- Can be accessed from ORM or URL Query String.
- Bundled command to keep revisions table healthy.
- Ability to override Revisions model for extension.

## Installation
Run the following command on your project root.
```composer require conceptbyte/time-traveller```

Add the service provider to the ```config/app.php``` file under the ```providers``` key. Make sure to add it after the default laravel service providers.
```php
'providers' => [
    ...,
    'ConceptByte\TimeTraveller\TimeTravellerServiceProvider'
]
```

## Publish Configuration
Run ```php artisan vendor:publish``` to publish the package configurations.

## Modify Configuration
Use the ```config/timetraveller.php``` file to modify the package defaults.
- Revisions Model ```'model' => ConceptByte\TimeTraveller\Models\Revision::class```
- Query String Parameter ```'at' => 'at'```
- Clear audits that are older than ```'clear' => '365'```

## Usage
Enable time traveller on a model by using the trait.
```php
class Post extents Model
{
    use TimeTravel;
}
```

All models that use the trait should implement the abstract function
```asbstract public function by()``` which should return any string.

This function can be used to save any additional attributes such as the 
owner of the change.

Get the state of a record at a specific data/time.
```php
    Post::at('58781813')->find(1);
```

Get the state of a record using a query string.

URL: ```timetravel.app/posts/1?at=58781813```
```php
    Post::find(1);
```

You can clear the audits table records that are older than a specified range.
```php artisan time-traveller:clear```. This will read the config file and clear records that are older than the configured number of days.

## Todo
- Artisan command to clear revisions.
- Return revisions as models/collections - not as nested relations.
- Documentation.
