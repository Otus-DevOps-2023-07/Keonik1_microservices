# Keonik1_microservices
Keonik1 microservices repository


# ДЗ 13
Добавлены файлы в docker-monolith для создания контейнера с приложением реддита.
Для запуска приложения:
1. Собрать контейнер командой `docker build -t reddit:latest .`
2. Запустить контейнер командой `docker run --rm --name reddit -d --network=host reddit:latest`
3. Открыть в браузере `http://<ip_машины>:9292`
