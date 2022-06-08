# Домашнее задание к занятию "6.1. Типы и структура СУБД"

## Задача 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД
лучше выбрать для хранения определенных данных.

Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:

- Электронные чеки в json виде
```
Документо-ориентированные - данные хранятся в виде документов как ключ-значение по аналогии со структурой json
```

- Склады и автомобильные дороги для логистической компании
```
Графовые - здесь узлами будут склады, а ребрами - дороги
```

- Генеалогические деревья
```
Иерархические - потому что здесь данные организованы в виде дерева
```

- Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации
```
Ключ-значение - данные представлены в виде уникально ключа и его значения.
```

- Отношения клиент-покупка для интернет-магазина
```
Реляционные - будем хранить данные в таблицах с соотвествующими ссылками между таблицами.
```


Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.

## Задача 2

Вы создали распределенное высоконагруженное приложение и хотите классифицировать его согласно
CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если
(каждый пункт - это отдельная реализация вашей системы и для каждого пункта надо привести классификацию):

- Данные записываются на все узлы с задержкой до часа (асинхронная запись)
```
AP
AP/EL
```

- При сетевых сбоях, система может разделиться на 2 раздельных кластера
```
AP
PA/EL
```

- Система может не прислать корректный ответ или сбросить соединение
```
CP
CP/E
```
А согласно PACELC-теореме, как бы вы классифицировали данные реализации?


## Задача 3

Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?
```
Не могут потому что ACID нацелен на создание высоконадежной системы, а BASE не обеспечивает, например высокую доступность, но такой принцип позволяет создать высокопроизводительные системы.
```

## Задача 4

Вам дали задачу написать системное решение, основой которого бы послужили:

- фиксация некоторых значений с временем жизни
- реакция на истечение таймаута

Вы слышали о key-value хранилище, которое имеет механизм [Pub/Sub](https://habr.com/ru/post/278237/).
Что это за система? Какие минусы выбора данной системы?

```
Redis
Минусы Redis:
Требуются достаточные ресурсы оперативной памяти.
Это NoSQL база, слабые характеристики надежности.  
Все данные храняться в оперативной памяти и при отказе сервера все данные с последней синхронизации с диском будут утеряны.
```
