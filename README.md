# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | будет ошибка, т.к. типы не соответсвуют для операции , int и str  |
| Как получить для переменной `c` значение 12?  | привести a к строке:       c=str(a)+b  |
| Как получить для переменной `c` значение 3?  | привести b к целому числу: c=a+int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3
import os
bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3
import os
bash_command = ["cd ~/repo/homework_4.2", "git status"]
path =  ~/repo/homework_4.2"
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path, prepare_result)
       #break

```

### Вывод скрипта при запуске при тестировании:
```
rudnev@it-ubuntu:~/repo/homework_4.2$ python3 hw2.py 
~/repo/homework_4.2 1.txt
~/repo/homework_4.2 2.txt
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
import os
import sys

cmd = os.getcwd()
bash_command = ["cd "+cmd, "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified: ', '')
        print(cmd+prepare_result)
#        break

```

### Вывод скрипта при запуске при тестировании:
```
rudnev@it-ubuntu:~/repo/homework_4.2$ python3 hw2.py
/home/rudnev/repo/homework_4.2  1.txt
/home/rudnev/repo/homework_4.2  2.txt

```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
##!/usr/bin/env python3
import socket
import time
import copy

addr = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
while 1 == 1:
    for key in addr:
        ip = socket.gethostbyname(key)
        if addr[key] != ip:
            badaddr = addr[key]
            print ("ERROR",key,"IP mismatch",badaddr,ip)
            addr[key] = ip
    time.sleep(3)

```

### Вывод скрипта при запуске при тестировании:
```
rudnev@it-ubuntu:~/repo/homework_4.2$ python3 hw4.py 
ERROR drive.google.com IP mismatch 0.0.0.0 64.233.161.194
ERROR mail.google.com IP mismatch 0.0.0.0 64.233.162.18
ERROR google.com IP mismatch 0.0.0.0 173.194.222.138
ERROR mail.google.com IP mismatch 64.233.162.18 64.233.162.83
ERROR google.com IP mismatch 173.194.222.138 173.194.222.102

```
