# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `c`  | a+b  | Не указано что обращаемся к переменным. По этому Баш сложил буквы которые мы ему указали |
| `d`  | 1+2  | Не использовали Let или явно не задекларировали переменную d, по этому Баш думает что переменная строка. A и B мы указали что обращаемся к переменным, но тип переменной D стока, по этому Баш сложил все в одну строку |
| `e`  | 3    | Баш поддерживает все стандартные операции с целыми числами если все записать в скобках |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

### Исправление
```bash
while ((1==1)
do
        curl https://localhost:4757
        if (($? != 0))
        then
                date >> curl.log
	    else 
		break
        fi
done
```

Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
array_int=(0 1 2 3 4)
for I in ${array_int[@]
do
	curl http://192.168.0.1
	if (($? !=0))
	then
		echo “192.168.0.1” >> log.txt
	fi

	curl http://173.194.222.113
	if (($? !=0))
	then
		echo “173.194.222.113” >> log.txt
	fi

	curl http://87.250.250.242
	if (($? !=0))
	then
		echo “87.250.250.242” >> log.txt
	fi	
done
```

## Обязательная задача 3
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
array_int=(0 1 2 3 4)
for I in ${array_int[@]
do
	curl http://192.168.0.1
	if (($? !=0))
	then
		echo “192.168.0.1” >> log.txt
	else 
		break
	fi

	curl http://173.194.222.113
	if (($? !=0))
	then
		echo “173.194.222.113” >> log.txt
	else 
		break
	fi

	curl http://87.250.250.242
	if (($? !=0))
	then
		echo “87.250.250.242” >> log.txt
	else 
		break
	fi	
done
```
