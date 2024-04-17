# Request
***
## Basics
`php artisan make:request [new_request_name]` - команда для создания `FormRequest` в фрейворке. 
**`FormRequest`** - класс для првил валидации форм в фреймворке.
Пример:
``` php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class UpdateUserProfile extends FormRequest
{
	public function rules(): array
  {
   return [
		'email' => ['required', 'email'],
		'name'  => ['required', 'alpha'],
		'age'   => ['integer', 'max:120'],
	 ];
  }
	
	public function messages():array
	{
		return [
		 'email.required' => 'Email необходимо заполнить email'
		];
	}
}
```
