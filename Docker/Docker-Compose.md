# Docker-Compose
***
`docker-compose.yml` - исполняемый файл для запуска контейнеров.
###### Example
```yml
version: '3' // версия композа

services: // начало сервисов
	php: // название
		image: // контейнер
		command: // команда контенеру
		volumes: // пути файлов
		ports: // порты
		build: // сборка
			context: //
			dockerfile: // путь к Dockerfile
		working_dir: // указание рабочей директории контейнера
		enviroment: // окружение
		depends_on: // зависимость от другого контейнера
		comtaimer_name: // название контейнера
```

`docker-compose up --build -d`
## Опции docker-compose
- `docker-compose up` - старт всех сервисов.
- `docker-compose down` - остановка всех сервисов.
- `docker-compose services` - список сервисов.
- `docker-compose images` - образ для сборки контейнера.
- `docker-compose build` - местоположение Dockerfile.
- `docker-compose build --no-cache` - собирает контейнеры заново, сбрасывая весь cache.
- `docker-compose depends_on` - указание на последовательность запуска сервисов.
- `docker-compose envoronment` - переменные окружения.
- `docker-compose volumes` - маппинг папок и томов.
- `docker-compose ports` - маппинг портов.
- `docker-compose command` - команда для запуска внутри контейнера.
