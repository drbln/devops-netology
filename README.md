# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

## Исправление
```json
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175
            }
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
    }

```


## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import socket
import time
import copy
import json
import yaml

addr = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
while 1 == 1:
    for key in addr:
        ip = socket.gethostbyname(key)
        if addr[key] != ip:
            badaddr = addr[key]
            print ("ERROR",key,"IP mismatch",badaddr,ip)
            addr[key] = ip
    with open('addr.json', 'w') as js1:
        js1.write(json.dumps(addr))
    with open('addr.yaml', 'w') as ym1:
        ym1.write(yaml.dump(addr))
    time.sleep(3)
```

### Вывод скрипта при запуске при тестировании:
```
rudnev@it-ubuntu:~/repo/devops-netology$ python3 hw.py 
ERROR drive.google.com IP mismatch 0.0.0.0 173.194.222.194
ERROR mail.google.com IP mismatch 0.0.0.0 142.251.1.83
ERROR google.com IP mismatch 0.0.0.0 74.125.131.101
ERROR mail.google.com IP mismatch 142.251.1.83 142.251.1.19
ERROR mail.google.com IP mismatch 142.251.1.19 142.251.1.18
ERROR google.com IP mismatch 74.125.131.101 74.125.131.138
ERROR google.com IP mismatch 74.125.131.138 74.125.131.101
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{"drive.google.com": "173.194.222.194", "mail.google.com": "142.251.1.18", "google.com": "74.125.131.101"}

```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
drive.google.com: 173.194.222.194
google.com: 74.125.131.101
mail.google.com: 142.251.1.18

```
