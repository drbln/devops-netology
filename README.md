## Задача 1

```
- Опишите своими словами основные преимущества применения на практике IaaC паттернов.
- Какой из принципов IaaC является основополагающим?
```
## Ответ 

```
- Быстрое и эффективное предоставление требуемой инфрастуктуры
- Имподентность (при многократном повротении всегда один и тот же результат)
```

## Задание 2
```
- Чем Ansible выгодно отличается от других систем управление конфигурациями?
- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

```

## Ответ 
```
- Ansible использует существующую SSH инфраструктуру, в то время какдругие (Saltstack, Chef, Puppet, и пр.) требуют установки специального PKI-окружения.
- Push. Мне кажется что когда с сервера множно выбрать какую конфигурацию и на какую группу серверов отправить и увидеть результат выполнения, то так надежднее
```

## Задание 3

```
Установить на личный компьютер:

- VirtualBox
- Vagrant
- Ansible

*Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.*
```

## Ответ
```
rudnev@z-dell2:~/repo/devops-netology$ vboxmanage --version
WARNING: The character device /dev/vboxdrv does not exist.
         Please install the virtualbox-dkms package and the appropriate
         headers, most likely linux-headers-standard.

         You will not be able to start VMs until this problem is fixed.
6.1.26_Ubuntur145957

rudnev@z-dell2:~/repo/devops-netology$ vagrant -v
Vagrant 2.2.6

rudnev@z-dell2:~/repo/devops-netology$ ansible --version
ansible 2.9.6

```
