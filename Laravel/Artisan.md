# Artisan
***
- `php artisan tinker` - работа в теминале фреймворка Laravel.
- `php artisan serve` - запускает сервер с приложением.
	- `--host` - указывает хост для отладки приложения.
	- `--port` - устанавливает порт.
	- `--tries` - количество TCP портов, которые следует перебрать в поисках свободного.
	- `--no-reload` - не перезапускать отладочный серваер при изменении файлов конфигурации.
- `php artisan migrate` - работа со структурой БД.
- `php artisan db:seed` - запускает генерацию данных в БД.
- `php artisan ui:auth` - запускает генерацию файлов для авторизации в фреймворке.
- `php artisan route:list` - выводит список объявленных путей.
- `php artisan make:police [policy_name]` - создает политику фреймворка с указанным именем.
- `php artisan key:generate` - команда для генерации секретного ключа.
- 