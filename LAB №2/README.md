# Лабораторная работа №2

## Задание

Написать два Dockerfile – плохой и хороший. Плохой должен запускаться и работать корректно, но в нём должно быть не менее 3 “bad practices”. В хорошем Dockerfile они должны быть исправлены. В Readme описать все плохие практики из кода Dockerfile и почему они плохие, как они были исправлены в хорошем Dockerfile, а также две плохие практики по использованию этого контейнера.

## Ход работы

1)  Для начала создали первый Dockerfile, который будет являться плохим. Папку, содержащую плохой вариант назовем fail.
В нашем «fail» Dockerfile имеется 3 “bad practices”:
- использование последней версии python, так как могут нестабильно работать все компоненты программы.
- использование команды RUN несколько раз может повлечь за собой сложность в управлении контейнером, также увеличивается время создания образа.
- также при запуске мы в fail мы использовали команду CMD, где мы обращаемся к python для запуска python файла. Минус этой команды в том, что при запуске контейнера мы можем переиздавать файл.
  
3 "bad practices" of «fail» Dockerfile:

![ошибка №1](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/fail/1.jpg)

файл python:

![ошибка №1](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/fail/2.jpg)



2)  Далее создали второй Dockerfile, который будет являться хорошим.
Назовем его «good» Dockerfile и покажем, как же мы всё исправили:
- исправили путем определения точной версии python. Например, версия 3.10 позволит утвердить неизменность сборки.
- исправили путем использования только одной команды RUN. 
- здесь CMD заменили на команду ENTRYPOINT. Теперь при запуске контейнера мы не можем переиздавать файл.

3) Запускаем процесс создания образа (docker build)

образ fail:

![образ fail](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/fail/4.jpg)

образ good:

![образ good](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/good/5.jpg)

4)  Запустим контейнер:
   
сам образ:

![образ](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/good/7.jpg)

Контейнер запустили с помощью команды docker run.

![запуск контейнера](https://github.com/V1lou/Clouds/blob/main/LAB%20№2/good/6.jpg)
