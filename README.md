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

# ДЗ 15
## Изменение имени проекта
Имя проекта можно изменить указав при запуске параметр `-p <name>` или указав в переменной `COMPOSE_PROJECT_NAME` имя проекта. Переменную можно поместить в файл `.env`.
## Работы выполненные во время ДЗ
- Добавлен файл `docker-compose.yml`, который содержит конфигурацию для запуска Reddit
- Добавлен файл `.env.example`, в котором хранится пример изменяемых данных для запуска приложения.

## Для запуска сервиса выполнить команды:
```bash
cd src
docker-compose up -d
```


# ДЗ 16
- Был поднят инстанс gitlab в докере через `docker-compose`
- Добавлен gitlab-ci пайплайн для reddit приложения

# ДЗ 17
Ссылка на докерхаб: https://hub.docker.com/u/keonik
- Были пересобраны образы для reddit app
- Был создан dockerfile и собран образ для prometheus
- Файлы связанные с докером из прошлы домашек перенесены в папку [docker](./docker/)
- docker-compose.yml обновлен - добавлен prometheus и node exporter для мониторинга системы и приложения

# ДЗ 18
- Добавлен файл docker/docker-compose-logging.yml, который содержит сервисы для логирования, такие как fluentd, zipkin, elasticksearch, kibana
- В сервисы post и ui в файле docker/docker-compose.yml добавлены опции логирования через fluentd и трасировки через zipkin
- В директории logging/fluentd содержатся dockerfile для сборки fluentd и конфиг для работы fluentd
Для сборки образов выполнить команды:
```bash
cd ./docker
docker compose -f ./docker-compose.yml build
docker compose -f ./docker-compose-logging.yml build
```

Для запуска выполнить команды:
```bash
cd ./docker
docker compose -f ./docker-compose-logging.yml up -d
docker compose -f ./docker-compose.yml up -d
```

# ДЗ 19
- Уставновлен kubernetes на двух нодах - одна мастер, другая рабочая с помощью `kubeadm`. 
    - Установка производилась на debian 11 по официальной инструкции для deb систем.
    - Помимо кубернетеса был установен еще cri-dockerd драйвер и докер
- Добавлена директория `kubernetes/reddit`, которая содержит deployments файлы для запуска сервисов
- В директорию `kubernetes` добавлен файл `calico`, для установки calico драйвера.
    ```bash
    cd kubernetes
    kubectl apply -f calico.yaml
    kubectl get nodes
    ```

## Запуск сервисов
Зайти в директорию `kubernetes/reddit` и выполнить команды:
```bash
kubectl apply -f <deployment file>
kubectl get pods
kubectl get deployments.apps
```
## Остановка сервисов
Для остановки сервисов зайти в директорию `kubernetes/reddit` и выполнить команды:
```bash
kubectl delete -f <deployment file>
```
