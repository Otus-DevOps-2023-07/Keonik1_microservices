# Keonik1_microservices
Keonik1 microservices repository


# ДЗ 13
Добавлены файлы в docker-monolith для создания контейнера с приложением реддита.
Для запуска приложения:
1. Собрать контейнер командой `docker build -t reddit:latest .`
2. Запустить контейнер командой `docker run --rm --name reddit -d --network=host reddit:latest`
3. Открыть в браузере `http://<ip_машины>:9292`


# ДЗ 14
- Добавлены исходники reddit в каталог `src`
- Добавлены файлы для сборки docker образов для сервисов comment, post и ui
## Для сборки контейнеров выполнить команды:
```bash
cd src
docker build -t ui:1.0 -t ui:latest ./ui/
docker build -t comment:1.0 -t comment:latest ./comment/
docker build -t post:1.0 -t post:latest ./post-py
```
## Для запуска сервиса выполнить команды:
```bash
docker pull mongo:3.6.8
docker network create reddit
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v ${PWD}/data/db:/data/db mongo:3.6.8
docker run -d --network=reddit --network-alias=post post:1.0
docker run -d --network=reddit --network-alias=comment comment:1.0
docker run -d --network=reddit -p 9292:9292 ui:1.0
```
