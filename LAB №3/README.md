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

Предварительно создали секреты для хранения имени пользователя и токена DockerHub (смотрите следующий скриншот).
Секреты позволят нам сохранить конфиденциальную информацию, такую как пароли, токены доступа и другие чувствительные данные без риска их утечки.



![img1](https://github.com/V1lou/Clouds/blob/main/LAB%20%E2%84%963/screenshots/secrets.png)





Теперь что касается yml-файла. У нас есть 3 шага "check code" (проверки кода), "go to DockerHub" (вход в DockerHub с использованием секретов), "push and build docker image to DockerHub" (создание Docker-образа, отправление созданного образа в DockerHub):

```
name: build docker
on:
  push:
    branches:
      - 'main'
jobs:
  build-and-push:
    name: build and push
    runs-on: ubuntu-latest
    steps:
      - name: check code
        uses: actions/checkout@v4
        
      - name: go to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_PASSWORD }}
          
      - name: push and build docker image to DockerHub
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: v1lou/image:latest
```
Построение прошло все этапы успешно:



![img2](https://github.com/V1lou/Clouds/blob/main/LAB%20%E2%84%963/screenshots/build-and-push.png)



Теперь можем видеть образ на DockerHub!

![img3](https://github.com/V1lou/Clouds/blob/main/LAB%20%E2%84%963/screenshots/the%20end.png)
