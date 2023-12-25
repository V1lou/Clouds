# Лабораторная работа №3

## Задание

Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь. 
Например, если результат - текстовый файлик, он должен автоматически сохраниться на локальную машину, в ваш репозиторий или на ваш сервер. 

## Ход работы

1) Для начала мы создали репозиторий, в котором будет сохраняться результат сборки докер образа: https://github.com/V1lou/Docker.

2) Кроме того, мы написали программу на языке Python. Этот код вычисляет количество дней до Нового года и записывает результат в файл с именем "days.txt":
```
from datetime import datetime

# Получаем текущую дату
current_date = datetime.now()

# Получаем дату Нового года для текущего года
new_year = datetime(current_date.year + 1, 1, 1)

# Вычисляем количество дней до Нового года
days_left = (new_year - current_date).days

# Записываем результат в файл
with open('days.txt', 'w') as file:
file.write(f'До Нового года осталось {days_left} дней')
```

3) Следующим шагом было создание Docker-файла, который будет запускать ранее написанный код - days.py:
```
FROM python:3.10

WORKDIR /app/

COPY . /app/

CMD ["python", "days.py"]
```

Этот Docker-файл:
- определяет образ, который использует Python версии 3.10 в качестве базового образа
- устанавливает рабочий каталог внутри контейнера как /app/ с помощью команды WORKDIR
- с помощью команды COPY он копирует все файлы из текущего каталога внутрь контейнера в каталог /app/
- с помощью команды CMD он указывает, что при запуске контейнера должен быть выполнен файл days.py с помощью интерпретатора Python.

Получается, наш Docker-файл создает контейнер, который запускает файл days.py при запуске контейнера с использованием интерпретатора Python.

4) Далее мы создали yml-файл с названием docker.yml, он будет содержать инструкции для автоматической сборки Docker-образа и сохранения результата его сборки после пуша в репозиторий.
Здесь мы создали секреты для хранения имени пользователя и токена DockerHub.

```
name: Build and Save Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-save-docker-image:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Build Docker image
        run: docker build -t my-image .

      - name: Save Docker image to file
        run: docker save my-image -o my-image.tar

      - name: Upload image artifact
        uses: actions/upload-artifact@v2
        with:
          name: my-image
          path: my-image.tar
```
