# Домашнее задание к занятию "7.1. Инфраструктура как код"

## Задача 1. Выбор инструментов.


1. Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?

~~~
Неизменяемый
Хоть и планируется много частых изменений, но на начальном этапе сразу реализовывать IaC
~~~

2. Будет ли центральный сервер для управления инфраструктурой?

~~~
Нет. Управление будет с любой машины через Terraform/Ansible и Git.
~~~

3. Будут ли агенты на серверах?

~~~
Нет. Используем  Ansible который не требует агента
~~~

4. Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?

~~~
Да. Terraform/Ansible.
~~~

## Задача 2. Установка терраформ.

~~~
rudnev@Mac-mini ~ % brew tap hashicorp/tap              
rudnev@Mac-mini ~ % brew install hashicorp/tap/terraform

rudnev@Mac-mini ~ % terraform --version
Terraform v1.2.1
on darwin_amd64
~~~

## Задача 3. Поддержка легаси кода.

~~~
rudnev@Mac-mini ~ % terraform --version
Terraform v1.2.1

rudnev@Mac-mini ~ % tfenv install
Installing Terraform v1.2.1

tfenv install terraform:1.1.9
Installing Terraform v1.1.9

rudnev@Mac-mini  % tfenv list                                    
  1.2.1
  1.1.9

rudnev@Mac-mini  % tfenv use 1.2.1
Switching default version to v1.2.1
Switching completed

rudnev@Mac-mini  % terraform --version
Terraform v1.2.1

rudnev@Mac-mini ~ % tfenv use 1.1.9     
Switching default version to v1.1.9
Switching completed

rudnev@Mac-mini ~ % terraform --version
Terraform v1.1.9
~~~
