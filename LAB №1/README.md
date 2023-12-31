# Лабораторная работа №1

## Задание

Пользуясь терминалом на компьютере А перенести файл с компьютера Б на компьютер С, находящиеся в одной локальной сети.

## Ход работы

Так как у участников нашей команды операционные системы Windows, то мы предварительно скачали свежий образ Ubuntu, а затем скачали и установили VirtualBox, тем самым создав виртуальную машину. Думаем, скриншоты и объяснения к данному этапу не нужны, так как это не входит в основую идею Лабораторной работы №1.

1)  Для начала подключаем все устройства к единой сети WiFi и убеждаемся в том, что они подключились к 1 локальной сети:
   
![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/3.png)

![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/10.png)

   
Получается IP следующие: ПК A - 172.20.10.4;
                         ПК Б - 172.20.10.5;
                         ПК С - 172.20.10.7.

2) Затем мы установили ssh-соединение, то есть подключили ПК А к ПК Б (смотрите следующий скриншот).

![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/4.png)


4) Финальное - перенести файл на ПК С.

В качестве файла был выбран скриншот рабочего стола - 1.jpeg. :

![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/file.jpg)




Итак, с помощью команды scp передаем файл (указываем распложение файла, имя пользователя и IP-адрес):
  
![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/5.png)



И, наконец, наш файл 1.jpeg был получен ПК С.

## Задание со звёздочкой

Сделать аналогичное, но подключаться при помощи публичных и приватных ключей, а не по логину паролю.

## Ход работы

1) Генерируем ключи на ПК А и ПК Б
   
далее: ключ Б передаем в А; ключ А передаем в С (смотрите скриншоты ниже).

![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/8.png)



![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/7.png)



![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/2.png)



![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/1.png)

Далее уже проверяем, подключается ли у нас ПК А к ПК С без ввода пароля.

2) Затем подключаемся через Б в А и перекидываем файл из ПК А в ПК С.

![file](https://github.com/V1lou/Clouds/blob/main/LAB%20№1/screenshots/6.png)

В итоге получилось следующее: 

1) Б-ключ передали в А;

2) Из А передали ключ в С;

3) Из Б подключились в А;

4) Файл из А перекинули в С и делали через Б.

И, наконец, наш файл был получен ПК С.
