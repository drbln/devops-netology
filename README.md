# Домашнее задание к занятию "7.5. Основы golang"

## Задача 1. Установите golang

1. Воспользуйтесь инструкций с официального сайта: https://golang.org/.
2. Так же для тестирования кода можно использовать песочницу: https://play.golang.org/.

~~~
rudnev@Mac-mini ~ % go version
go version go1.18.3 darwin/amd64
~~~

## Задача 3. Написание кода.
Напишите программу для перевода метров в футы (1 фут = 0.3048 метр). Можно запросить исходные данные у пользователя, а можно статически задать в коде. Для взаимодействия с пользователем можно использовать функцию Scanf:
~~~go
package main

import "fmt"

func MtoF(m float64)(f float64) {
    f = m * 3.281
    return
}

func main() {
    fmt.Print("Input length in meters: ")
    var input float64
    fmt.Scanf("%f", &input)

    output := MtoF(input)

    fmt.Printf("Footage: %v\n", output)
}
~~~

Напишите программу, которая найдет наименьший элемент в любом заданном списке, например:
~~~
x := []int{48,96,86,68,57,82,63,70,37,34,83,27,19,97,9,17,}
~~~

~~~go
package main

import "fmt"
import "sort"

func GetMin (toSort []int)(minNum int) {
	sort.Ints(toSort)
	minNum = toSort[0]
	return
}

func main() {
	x := []int{48,96,86,68,57,82,63,70,37,34,83,27,19,97,9,17,}
	y := GetMin(x)
	fmt.Printf("The smallest number in the list: %v\n", y)
}
~~~

Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. То есть (3, 6, 9, …).

~~~go
package main

import "fmt"

func FilterList ()(devidedWithNoReminder []int) {
	for i := 1;  i <= 100; i ++ {
		if	i % 3 == 0 {
			devidedWithNoReminder = append(devidedWithNoReminder, i)
		}
	}
	return
}

func main() {
	toPrint := FilterList()
	fmt.Printf("Числа от 1 до 100, которые делятся на 3: %v\n", toPrint)
}

~~~
